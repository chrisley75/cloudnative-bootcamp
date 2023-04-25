# Tekton Pipeline Build - buildah

## Standards

- Use the Task Catalog to define a `Pipeline` step that creates a Docker image then pushes the image to quay.io

## Starting point

This lesson continues to build upon the earlier lessons, so the starting point for this lesson is the state of the Pipeline after adding `buildah`:

You have the following folder structure:

- `base/`
  - [`nodejs.Pipeline.yaml`](./starting-point/base/nodejs.Pipeline.yaml)
- [`express-sample-app.PipelineRun.yaml`](../02-npm-test/starting-point/express-sample-app.PipelineRun.yaml)

## Lesson

[`buildah`](https://github.com/containers/buildah) is tool for building Open Container Initiative (OCI) containers. buildah, for your case, will use a [Dockerfile to build a container](https://github.com/containers/buildah/blob/master/docs/tutorials/01-intro.md#using-dockerfiles-with-buildah).

`buildah` has been configured as a `ClusterTask` on your cluster:

```shell
$:> oc get clustertasks | grep -i buildah
```

It is therefore available across all namespaces in the cluster.

Like other tasks, you begin at the [documentation](https://github.com/tektoncd/catalog/tree/main/task/buildah/0.1).

If, for any reason, this task is not installed as a `ClusterTask` then you may install it in your namespace with:

```shell
$:>  oc apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/buildah/0.1/buildah.yaml
task.tekton.dev/buildah created
```

It is important to note that for this course you need to use the `0.1` release of the buildah Tekton Task since the `0.2` version is incompatible with OpenShift Container Platform `4.6`.

Since the Dockerfile is located at the project root, you only need to include the image name and workspace named `source` to add the task to your pipeline:

```yaml
apiVersion: tekton.dev/v1beta1
kind: Pipeline
metadata:
  name: nodejs
spec:
  params:
    - name: source-repo
      # ...
    - name: image-repo
      type: string
      description: Docker image repository
  tasks:
    - name: clone-repository
      # ...
    - name: run-tests
      # ...
    - name: create-image
      params:
        - name: IMAGE
          value: "$(params.image-repo):$(tasks.clone-repository.results.commit)"
      runAfter:
        - run-tests
      taskRef:
        name: buildah
        kind: ClusterTask
      workspaces:
        - name: source
          workspace: pipeline-shared-data
  workspaces:
    - name: pipeline-shared-data
```

Don't forget to add a new parameter `image-repo` to the list of pipeline parameters.

After specifying the new pipeline parameter, you also need to provide a value for `image-repo` in your PipelineRun:

```yaml
apiVersion: tekton.dev/v1beta1
kind: PipelineRun
metadata:
  generateName: express-sample-app-
spec:
  params:
    - name: source-repo
      value: https://github.com/<your-account>/express-sample-app
    - name: image-repo
      value: quay.io/<your-account>/express-sample-app
  pipelineRef:
    name: nodejs
  workspaces:
    - name: pipeline-shared-data
      # ...
```

If you already have a `quay.io` account, use that account name for `<your-account>`. If you don't have a `quay.io` account, leave it as it is – we are going to create one in a minute.

If you run the cycle of `oc apply -f` and use `oc create -f` for a new `PipelineRun`, you see the following error at the end of the logs:

```shell
$:> tkn pr logs <your-name>-pipeline-from-scratch-pipeline-run-l8xsb -f
...
[create-image : push] + buildah --storage-driver=overlay push --tls-verify=true --digestfile /workspace/source/image-digest quay.io/andreask/express-sample-app:4ae168595de8f34e5120a75742a9d100e26ff0ec docker://quay.io/upslopeio/express-sample-app:4ae168595de8f34e5120a75742a9d100e26ff0ec
[create-image : push] Getting image source signatures
[create-image : push] unauthorized: authentication required

[create-image : digest-to-results] 2021/05/22 23:06:41 Skipping step because a previous step failed

failed to get logs for task create-image : container step-digest-to-results has failed  : [{"key":"StartedAt","value":"2021-05-22T23:06:41.478Z","resourceRef":{}}]
```

the important error is `unauthorized: authentication required` which indicates the need for authentication before pushing to Quay.io.

To store you password on the cluster you will use a `docker-registry` Kubernetes `Secret`. "A Secret is an object that contains a small amount of sensitive data such as a password, a token, or a key." ([source](https://kubernetes.io/docs/concepts/configuration/secret/)).

**Create a Red Hat account**

Visit `redhat.com`, and create an account if you don't already have one.

1. Next, go to `quay.io` and sign in with your Red Hat account.
2. Create a new repository named `express-sample-app` - make it public and empty.
3. Click your name in the upper right, and select "Account Settings"
4. Click on the "Robot" icon and then click "Create Robot Account"
5. Name the account `quay_io_credentials`
6. Set the permissions for the `express-sample-app` repository to `write`
7. Download `*-quay-io-credentials-secret.yaml` (Gear Icon, View Credentials, Kubernetes Secret, Download)
8. Edit the downloaded file and change the name of the secret to `quay-io-credentials`
9. Change the name of the downloaded file to `base/quay-io-credentials.Secret.example.yaml` **NB** Do not commit this file to any Git repository. Add it to `base/.gitignore` (e.g. as `*.Secret.yaml`).

```shell
$:> oc apply -f base/quay-io-credentials.Secret.example.yaml
```

Next, update the PipelineRun to use your Quay.io account name rather than `<your-account>`:

```yaml
params:
  # ...
  - name: image-repo
    value: quay.io/<your-account>/express-sample-app
```

If you run the `oc create -f <pipeline-run>` / `tkn pr logs -f` cycle, you get the same failure. Despite the Secret existing, it is not yet accessible to the Pipeline.

First, you need to define a `ServiceAccount`. Create a file `base/build-bot.ServiceAccount.yaml`:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: build-bot
secrets:
  - name: quay-io-credentials
```

Then apply the file:

```shell
$:> oc apply -f base/build-bot.ServiceAccount.yaml
```

After creating the `build-bot` service account you add it to the PipelineRun:

```yaml
apiVersion: tekton.dev/v1beta1
kind: PipelineRun
metadata:
  generateName: express-sample-app-
spec:
  serviceAccountName: build-bot
  # ...
```

At this point, if you run the Pipeline, it will authenticate with Quay.io, and your Pipeline succeeds:

```shell
[create-image : push] + buildah --storage-driver=overlay push --tls-verify=true --digestfile /workspace/source/image-digest quay.io/andreask/express-sample-app:c68d5cc5d9658dd236af242e87a866ced28a5833 docker://quay.io/andreask/express-sample-app:c68d5cc5d9658dd236af242e87a866ced28a5833
[create-image : push] Getting image source signatures
[create-image : push] Copying blob sha256:5ae397d2979f4776d5ef6b58a21ef4d1211b4aa37029e169736bf041dc0ec023
[create-image : push] Copying blob sha256:705614f99d37c840f420251c92666b752d8e6ebbb9d2c9a3fbd7d987836efc73
[create-image : push] Copying blob sha256:d8871dbe0b2341426f3c4769eb2e14914b8169d67598be2c0e8cc7d6dac0daff
[create-image : push] Copying blob sha256:ed210c663870a161d0d084b36a8765a6e78f121b9a51b24dfd5f2fd71b99b4f0
[create-image : push] Copying blob sha256:b2d5eeeaba3a22b9b8aa97261957974a6bd65274ebd43e1d81d0a7b8b752b116
[create-image : push] Copying config sha256:a2dee33fedacb80cfe77f655aa34f9a106e12110176f2b7921099e685ef8995a
[create-image : push] Writing manifest to image destination
[create-image : push] Copying config sha256:a2dee33fedacb80cfe77f655aa34f9a106e12110176f2b7921099e685ef8995a
[create-image : push] Writing manifest to image destination
[create-image : push] Writing manifest to image destination
[create-image : push] Storing signatures

[create-image : digest-to-results] + cat /workspace/source/image-digest
[create-image : digest-to-results] + tee /tekton/results/IMAGE_DIGEST
[create-image : digest-to-results] sha256:14336608974258020eafabf3da4c7852446be6120ac33e785be58238080d1fcd
```

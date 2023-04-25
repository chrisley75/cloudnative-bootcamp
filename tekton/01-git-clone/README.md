# Tekton Pipeline Build - `git-clone`

## Standards

- Define `Task`, `Pipeline`, and `PipelineRun`
- Use the Task Catalog to build a `Pipeline` that clones a Git repository

## Lesson

Throughout the "Tekton Pipeline Build" lessons, you will implement the steps of a Continuous Integration (CI) pipeline, via Tasks from the Tekton catalog.

For a CI pipeline to work, it first needs to acquire its main input: the codebase that you want to build and deploy. In this first version of the pipeline, you will have two Tasks:

1. clone the source repository
2. run `cat README.md` as confirmation that `git-clone` (and the workspace) are functioning as intended

Luckily, the [`git-clone` Tekton Task](https://github.com/tektoncd/catalog/tree/main/task/git-clone/0.2) will take care of most of the heavy lifting.

`git-clone` has been configured as a `ClusterTask` on your cluster:

```shell
$:> oc get clustertasks | grep -i git-clone
```

It is therefore available across all namespaces in the cluster.

Have a look at the [`git-clone` documentation](https://github.com/tektoncd/catalog/tree/main/task/git-clone/0.3) and you will notice that the least amount of data you may provide to the `git-clone` task is a workspace and URL of the Git repository to clone. For simplicity, you will use [this public repository](https://github.com/upslopeio/express-sample-app) so that you can avoid the complexities of authentication.

What is a [Pipeline](https://tekton.dev/docs/pipelines/pipelines/)? The documentation provides a good definition:

> A Pipeline is a collection of Tasks that you define and arrange in a specific order of execution as part of your continuous integration flow. Each Task in a Pipeline executes as a Pod on your Kubernetes cluster.

You begin your pipeline by first creating a project for it:

```shell
$:> oc new-project <your-name>-express-sample-app
```

You will build the application from the source in [this repository](https://github.com/upslopeio/express-sample-app) which is a sample, containerized Express application.

First, fork the repository at <https://github.com/upslopeio/express-sample-app>.

Throughout this course, you will use a naming convention of `<metadata.name>.<kind>.yaml` for all of your yaml files. As with Kubernetes, the `metadata.name` and `kind` combination uniquely identifies a resource.

Now create `nodejs.Pipeline.yaml`:

```shell
mkdir -p ~/workspace/nodejs.Pipeline/base
touch ~/workspace/nodejs-pipeline/base/nodejs.Pipeline.yaml
```

Add the following to `~/workspace/nodejs-pipeline/base/nodejs.Pipeline.yaml`

```yaml
apiVersion: tekton.dev/v1beta1
kind: Pipeline
metadata:
  name: nodejs
spec:
  params:
    - name: source-repo
      type: string
      description: Source code repository
  tasks:
    - name: clone-repository
      params:
        - name: url
          value: "$(params.source-repo)"
      taskRef:
        name: git-clone
        kind: ClusterTask
      workspaces:
        - name: output
          workspace: pipeline-shared-data
  workspaces:
    - name: pipeline-shared-data
```

Then create the Pipeline on the Cluster:

```shell
$:> cd ~/workspace/nodejs-pipeline/
$:> oc create -f base/nodejs.Pipeline.yaml
pipeline.tekton.dev/nodejs created
```

Recall that each task runs in a separate Pod.

**_But wait..._** Since each Task in a pipeline runs on a different Pod, and Pods by default have ephemeral storage (i.e. `emptyDir`), then by default, there is no way to share code between the steps in your build! When each Pod terminates, `emptyDir` for that Pod is flushed. However, a Pipeline must have a way to share files between Tasks.

**There is a solution:** a repository can be shared between build steps if you use a [`PersistentVolume`](https://kubernetes.io/docs/concepts/storage/persistent-volumes/). This introduces [Kubernetes storage](https://kubernetes.io/docs/concepts/storage/). For your use case in this course, the easiest approach is to create a [`PersistentVolumeClaim` (PVC)](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistentvolumeclaims) which will manage the `PersistentVolume` binding process for you.

One important detail about Tekton is that a `Pipeline` will expect its `PipelineRun` to provide the workspaces and Volume bindings. Similar to `TaskRun`, you will use a generated name and `oc create -f <file-name>.yaml` with a separate `PipelineRun` file.

Next, create `express-sample-app-pipeline-run.yaml` to define the PipelineRun:

```shell
touch ~/workspace/nodejs-pipeline/express-sample-app.PipelineRun.yaml
```

Then, add the following to `express-sample-app.PipelineRun.yaml`

```yaml
apiVersion: tekton.dev/v1beta1
kind: PipelineRun
metadata:
  generateName: express-sample-app-
spec:
  params:
    - name: source-repo
      value: https://github.com/<your-account>/express-sample-app
  pipelineRef:
    name: nodejs
  workspaces:
    - name: pipeline-shared-data
      volumeClaimTemplate:
        spec:
          resources:
            requests:
              storage: 1Gi
          accessModes:
            - ReadWriteOnce
```

In this case you use a [`volumeClaimTemplate`](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) to ask the cluster for enough resources for your PipelineRun.

Next, create the PipelineRun in the cluster:

```shell
$:> oc create -f express-sample-app.PipelineRun.yaml
pipelinerun.tekton.dev/express-sample-app-fht8b created
```

Then, check the logs:

```shell
$:> tkn pr logs express-sample-app-fht8b
```

Finally, your goal is to run `cat README.md` to confirm that you are sharing data between Tasks (Pods) in your Pipeline.

Now you will define a new task named `print-readme`, then add a `print-readme` step to your pipeline inside of `nodejs.Pipeline.yaml`:

```shell
touch ~/workspace/nodejs-pipeline/base/print-readme.Task.yaml
```

```yaml
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: print-readme
spec:
  steps:
    - name: print
      image: quay.io/ibmgaragecloud/alpine-curl
      script: |
        cat dir-with-readme/README.md
  workspaces:
    - name: dir-with-readme
```

Then add the task to the nodejs pipeline

```yaml
apiVersion: tekton.dev/v1beta1
kind: Pipeline
metadata:
  name: nodejs
spec:
  params:
    - name: source-repo
      type: string
      description: Source code repository
  tasks:
    - name: clone-repository
      params:
        - name: url
          value: "$(params.source-repo)"
      taskRef:
        name: git-clone
        kind: ClusterTask
      workspaces:
        - name: output
          workspace: pipeline-shared-data
    - name: print-readme
      runAfter:
        - clone-repository
      taskRef:
        name: print-readme
        kind: Task
      workspaces:
        - name: dir-with-readme
          workspace: pipeline-shared-data
  workspaces:
    - name: pipeline-shared-data
```

This final YAML shows three important concepts:

1. using an existing `Task` from the Tekton Catalog
1. writing a custom `Task`
1. starting with a clean build folder and sharing files across tasks by using a `volumeClaimTemplate`. (A `PersistentVolumeClaim` would have used the same persistent volume for every pipeline run rather than a new volume for each pipeline run.)

If you complete the final cycle of `oc apply -f` / `oc create -f` / `tkn pr logs` then you should see the desired output:

```shell
[clone-repository : clone] + CHECKOUT_DIR=/workspace/output/
[clone-repository : clone] + '[[' true '==' true ]]
[clone-repository : clone] + cleandir
[clone-repository : clone] + '[[' -d /workspace/output/ ]]
[clone-repository : clone] + rm -rf /workspace/output//Dockerfile /workspace/output//README.md /workspace/output//app.js /workspace/output//bin /workspace/output//package.json /workspace/output//public /workspace/output//routes /workspace/output//views
[clone-repository : clone] + rm -rf /workspace/output//.git /workspace/output//.gitignore
[clone-repository : clone] + rm -rf '/workspace/output//..?*'
[clone-repository : clone] + test -z
[clone-repository : clone] + test -z
[clone-repository : clone] + test -z
[clone-repository : clone] + /ko-app/git-init -url https://github.com/upslopeio/express-sample-app -revision  -refspec  -path /workspace/output/ '-sslVerify=true' '-submodules=true' -depth 1 -sparseCheckoutDirectories
[clone-repository : clone] {"level":"info","ts":1621322133.0496006,"caller":"git/git.go:169","msg":"Successfully cloned https://github.com/upslopeio/express-sample-app @ d11ec2928f3110d2246afabe053faff71e41430c (grafted, HEAD) in path /workspace/output/"}
[clone-repository : clone] {"level":"info","ts":1621322133.1167107,"caller":"git/git.go:207","msg":"Successfully initialized and updated submodules in path /workspace/output/"}
[clone-repository : clone] + cd /workspace/output/
[clone-repository : clone] + git rev-parse HEAD
[clone-repository : clone] + RESULT_SHA=d11ec2928f3110d2246afabe053faff71e41430c
[clone-repository : clone] + EXIT_CODE=0
[clone-repository : clone] + '[' 0 '!=' 0 ]
[clone-repository : clone] + echo -n d11ec2928f3110d2246afabe053faff71e41430c
[clone-repository : clone] + echo -n https://github.com/upslopeio/express-sample-app

[print-readme : print] + cat dir-with-readme/README.md
[print-readme : print] # Express Sample Application
[print-readme : print]
[print-readme : print] If you are seeing this page as output in a Tekton Pipeline, then you are GOOD TO GO!
```

# Tekton Pipeline Build - gitops

## Standards

- Push a successfully deployed configuration (i.e., `manifests.yaml`) to a GitOps repository as the final step in the pipeline

## Starting point

This lesson continues to build upon the earlier lessons, so the starting point for this lesson is the following:

You have the following folder structure:

- `base/`
  - [`build-bot.ServiceAccount.yaml`](../05-triggers/starting-point/base/build-bot.ServiceAccount.yaml)
  - [`create-configuration.Task.yaml`](../05-triggers/starting-point/base/create-configuration.Task.yaml)
  - [`deploy.Task.yaml`](../05-triggers/starting-point/base/deploy.Task.yaml)
  - [`kustomization.yaml`](./starting-point/base/kustomization.yaml)
  - [`nodejs.Pipeline.yaml`](./starting-point/base/nodejs.Pipeline.yaml)
  - [`quay-io-credentials.Secret.yaml`](../04-kustomize/starting-point/base/quay-io-credentials.Secret.yaml) You will need to add your credentials to this file.
  - [`webhook-receiver.EventListener.yaml`](./starting-point/base/webhook-receiver.EventListener.yaml)
- [`express-sample-app.PipelineRun.yaml`](../05-triggers/starting-point/express-sample-app.PipelineRun.yaml)

## Lesson

The final step of the pipeline is to update the GitOps repository. In other words, this lesson completes the "Continuous Integration" portion of the CI pipeline.

For this task we opted to use a shell script but could likely refactor this script into a series of steps with catalogue tasks:

```yaml
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: save-configuration
spec:
  params:
    - name: manifest
      description: path of the manifest file in the source workspace
    - name: app-name
      description: name of the app
    - name: sha
      description: sha of the current commit
  results:
    - name: commit
      description: The HEAD commit SHA from the target repository
  steps:
    - name: gitops
      image: quay.io/ibmgaragecloud/ibmcloud-dev:v2.0.4
      env:
        - name: NAMESPACE
          valueFrom:
            fieldRef:
              fieldPath: metadata.namespace
        - name: GIT_PROTOCOL
          valueFrom:
            configMapKeyRef:
              name: production-repository
              key: protocol
              optional: true
        - name: GIT_HOST
          valueFrom:
            configMapKeyRef:
              name: production-repository
              key: host
        - name: GIT_ORG
          valueFrom:
            configMapKeyRef:
              name: production-repository
              key: org
        - name: GIT_REPO
          valueFrom:
            configMapKeyRef:
              name: production-repository
              key: repo
        - name: GIT_BRANCH
          valueFrom:
            configMapKeyRef:
              name: production-repository
              key: branch
              optional: true
      script: |
        #!/usr/bin/env bash
        set -e

        trap 'catch $? $LINENO' EXIT ERR

        catch() {
          if [ "$1" != "0" ]; then
            echo "Error $1 occurred on $2"
          fi
        }

        GIT_USERNAME=$(cat git-credentials/username)

        if [[ -z "${GIT_USERNAME}" ]]; then
          echo "Username for GitOps repo not set in secret git-credentials"
          exit 1
        fi

        GIT_PASSWORD=$(cat git-credentials/password)

        if [[ -z "${GIT_PASSWORD}" ]]; then
           echo "Password for GitOps repo not set in secret git-credentials"
           exit 1
        fi

        if [[ -z "${NAMESPACE}" ]]; then
           echo "NAMESPACE is not set"
           exit 1
        fi

        if [[ -z "$(params.app-name)" ]]; then
           echo "app-name not set"
           exit 1
        fi

        if [[ -z "$(params.sha)" ]]; then
           echo "sha not set"
           exit 1
        fi

        APP_NAME="$(params.app-name)"
        SHA="$(params.sha)"

        BRANCH_CMD=""
        if [[ -n "${GIT_BRANCH}" ]]; then
          BRANCH_CMD="-b ${GIT_BRANCH}"
        fi

        PROTOCOL="${GIT_PROTOCOL}"
        if [[ -z "${PROTOCOL}" ]]; then
          PROTOCOL="https"
        fi

        git config --global user.email "${APP_NAME}@${NAMESPACE}"
        git config --global user.name "${NAMESPACE}/${APP_NAME}"

        cd $(workspaces.target.path)

        echo "git clone ${BRANCH_CMD} ${PROTOCOL}://${GIT_USERNAME}:<redacted>@${GIT_HOST}/${GIT_ORG}/${GIT_REPO}"

        GIT_URL="${PROTOCOL}://${GIT_USERNAME}:${GIT_PASSWORD}@${GIT_HOST}/${GIT_ORG}/${GIT_REPO}"
        git clone ${BRANCH_CMD} ${GIT_URL} git-ops
        cd git-ops

        mkdir -p ${NAMESPACE}/${APP_NAME}/
        cp ../../source/$(params.manifest) ${NAMESPACE}/${APP_NAME}/

        if [[ $(git status -s | wc -l) -eq 0 ]]; then
          echo "No changes"
        else
          git add "${NAMESPACE}/${APP_NAME}/."
          git commit -m "update ${NAMESPACE}/${APP_NAME} to ${SHA}"
          git push
        fi

        RESULT_SHA="$(git rev-parse HEAD)"
        EXIT_CODE="$?"
        if [ "$EXIT_CODE" != 0 ] ; then
          exit $EXIT_CODE
        fi

        echo -n "$RESULT_SHA" > $(results.commit.path)
  workspaces:
    - name: source
      description: contains the cloned git repo
    - name: target
      description: contains the cloned target git repository
    - name: git-credentials
```

Save this task in a file called `base/save-configuration.Task.yaml`, and also reference this file in the `base/kustomization.yaml`.

To use the above task, you first need to [Generate a GitHub Personal Access Token](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token#:~:text=In%20the%20upper%2Dright%20corner,Click%20Generate%20new%20token.)

Next, create a secret with your GitHub Personal Access Token (PAT) and save it in `base/git-credentials.Secret.yaml`:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: git-credentials
type: kubernetes.io/basic-auth
stringData:
  username: <your-github-username>
  password: <your-github-personal-access-token>
```

Then create `base/production-repository.ConfigMap.yaml` with the relevant information for the production repository:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: production-repository
data:
  branch: main
  host: github.com
  org: cloud-native-garage-method-cohort
  repo: <cohort-name>-production
```

> NOTE: `repo` will be `emea-X-production` where `X` is the cohort number supplied by the instructor.

Don't forget to add these two new files to the `base/kustomization.yaml`.

Finally, add a step to your `nodejs.Pipeline.yaml`:

```yaml
apiVersion: tekton.dev/v1beta1
kind: Pipeline
metadata:
  name: nodejs
spec:
  # ...
  tasks:
    - name: clone-repository
      # ...
    - name: run-tests
      # ...
    - name: create-image
      # ...
    - name: create-configuration
      # ...
    - name: deploy
      # ...
    - name: save-configuration
      params:
        - name: manifest
          value: "$(tasks.create-configuration.results.manifest)"
        - name: app-name
          value: "$(params.app-name)"
        - name: sha
          value: "$(tasks.clone-repository.results.commit)"
      runAfter:
        - deploy
      taskRef:
        name: save-configuration
      workspaces:
        - name: source
          workspace: pipeline-shared-data
        - name: target
          workspace: gitops-repository
        - name: git-credentials
          workspace: git-credentials
  workspaces:
    - name: pipeline-shared-data
    - name: gitops-repository
    - name: git-credentials
```

Next, update the `PipelineRun`'s to supply the `git-credentials` workspace required by the `save-configuration` task and `nodejs` pipeline.

The workspaces in `webhook-receiver.EventListener.yaml` **and** `express-sample-app.PipelineRun.yaml`.

```yaml
# ...
pipelineRef:
  name: nodejs
workspaces:
  - name: pipeline-shared-data
    # ...
  - name: gitops-repository
    volumeClaimTemplate:
      spec:
        resources:
          requests:
            storage: 1Gi
        accessModes:
        - ReadWriteOnce
  - name: git-credentials
    secret:
      secretName: git-credentials
```

Then run `kubectl apply -k base` to update the pipeline.

Next, make a commit to your `express-sameple-app` repository and observe the entire pipeline kick off and complete.

Finally, open the GitOps repository and make sure your build committed the `manifests.yaml` file to the appropriate folder.

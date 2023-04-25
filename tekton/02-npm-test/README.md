# Tekton Pipeline Build - `npm test`

## Standards

- Use the Task Catalog to define a `Pipeline` step that runs the tests for a node application.

## Starting point

This lesson continues to build upon the earlier lesson, _before_ adding the `cat README.md` step, so the starting point for this lesson is the following:

You have the following folder structure:

- `base/`
  - [`nodejs.Pipeline.yaml`](./starting-point/base/nodejs.Pipeline.yaml)
- [`express-sample-app.PipelineRun.yaml`](./starting-point/express-sample-app.PipelineRun.yaml)

## Lesson

One of the most important steps in a Continuous Integration (CI) pipeline is running the tests. Since your application uses Node, you will use the `npm install-ci-test` command to validate that the current code does not introduce test failures.

This requires another step after cloning the repository:

1. `npm install-ci-test` - run a clean installation of the project dependencies, then run all the tests.

Sticking with the theme of using Tekton's Catalog Task, you will use the [npm Tekton Task](https://github.com/tektoncd/catalog/tree/main/task/npm/0.1).

Like other Catalog Tasks, you begin by installing the `npm` task:

```shell
$:> tkn hub install task npm
```

Note that installing the `npm` task using the above command may print the following error:

```shell
Error: Task npm(0.1) requires Tekton Pipelines min version v0.17.0 but found v0.XX.XX
```

It turns out that the `npm` task is not compatible with Tekton Pipelines less than version v0.17.0. However, we have tested the `npm` task with the installed version of Tekton pipelines, and it does work. You can bypass the version validation by installing the task using `kubectl` or `oc`.

```shell
$:> oc apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/npm/0.1/npm.yaml
```

Next add a `run-tests` step to your pipeline:

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
      params:
        - name: ARGS
          value:
            - install-ci-test
      runAfter:
        - clone-repository
      taskRef:
        name: npm
      workspaces:
        - name: source
          workspace: pipeline-shared-data
  workspaces:
    - name: pipeline-shared-data
```

Finally, you use `oc apply -f base` to update the pipeline and then `oc create -f .` to create a new `PipelineRun` and `oc pr logs -f` to observe the progress of the pipeline in the terminal. You can also see the `PipelineRun` and its progress in the OpenShift console. Open the OpenShift console by running `oc console` in the terminal.

Both steps in the pipeline should run sequentially and complete successfully.

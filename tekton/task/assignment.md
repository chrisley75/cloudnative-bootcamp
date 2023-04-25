# Tekton Task Assignment

## Standards to Achieve

By completing this assignment, you demonstrate the following Standards:

- Define Tekton `Task`
- Write a Tekton `Task` and run it via `TaskRun`

## Instructions

Inside the `assignments` repository, first create a new branch from `main`, then create a folder named `<your-name>/tekton-task-assignment`.

Your goal in this assignment is to write your own Tekton task, in YAML, that utilizes the CLI commands we have seen so far.

For this assignment you will use the [`alpine-curl` image](https://quay.io/ibmgaragecloud/alpine-curl) image to make a `Task` that has multiple steps.

**IMPORTANT:** Before you begin this project, first use `icc` to connect to the correct cluster. Next, **_make sure you are not in the `default` namespace_** using `oc config current-context` or `oc project`. If you are in the wrong namespace you can list all projects with `oc projects`, then use `oc project <my-project-name>` to change to your namespace or `oc new-project <my-project-name>` to create a new project and change context to it.

### Problem 1

Your Tekton `Task` should be defined in a file named `get-non-root-steps.Task.yaml`, a `Task` with a name of `get-non-root-steps`, and have the following steps:

- `retrieve-yaml`

  Retrieve the contents of the file at this address [https://raw.githubusercontent.com/tektoncd/pipeline/main/examples/v1beta1/taskruns/run-steps-as-non-root.yaml](https://raw.githubusercontent.com/tektoncd/pipeline/main/examples/v1beta1/taskruns/run-steps-as-non-root.yaml) and store it in a folder in `/workspace` named `k8s`. You should preserve the file name `run-steps-as-non-root.yaml`.

- `extract-task`

  Create a new file in `/workspace/k8s` named `run-steps-as-non-root.Task.yaml` that contains only the first 24 lines of the file you just downloaded.

- `swap-image`

  Use the file created in the last step (`run-steps-as-non-root.Task.yaml`) and [`sed`](https://www.cyberciti.biz/faq/how-to-use-sed-to-find-and-replace-text-in-files-in-linux-unix-shell/) to change the `image` from `ubuntu` to `quay.io/ibmgaragecloud/alpine-curl` and create a new file named `run-steps-as-non-root-alpine.Task.yaml`.

- Run your `Task` using `tkn`

- Take the output of your `TaskRun` store it in a file named `get-non-root-steps.Task.txt`

- Put `get-non-root-steps.Task.txt` and `get-non-root-steps.Task.yaml` file in the `<your-name>/tekton-task-assignment/base/` folder in the `assignments` repository and make a commit with it.

### Problem 2

- In a file named `<your-name>/tekton-task-assignment/get-non-root-steps.TaskRun.yaml`, create a `TaskRun` for the Task you wrote in the assignment using [the Documentation](https://tekton.dev/vault/pipelines-v0.14.3/taskruns/). Your `TaskRun` should automatically generate the `TaskRun`'s name. Commit this `TaskRun` to the same folder as Problem 1 in a file name.

### Problem 3

- Read about [pipe (`|`) use for the CLI](https://www.geeksforgeeks.org/piping-in-unix-or-linux/). Rewrite the assignment from Problem 1 as a `Task` with **one (1)** step that uses `|`s in the `script` section to achieve everything.
- Save the task as `get-non-root-steps-pipe.Task.yaml` file. Commit this file to the same folder as Problem 1.

### Problem 4 (Optional/Stretch)

So far we have used the `alpine-curl` image from Quay.io. However, [the `ibmgaragecloud` organization](https://quay.io/organization/ibmgaragecloud) hosts other images. Choose one of these images and create a new `Task` of your choosing.

## Submit the Assignment

1. `cd` to the root of the `assignments` repository.
2. if you have not already created a new branch, follow [the steps](../../git/git-workflow-step-by-step.md) outlined in the git workflow to create a new branch.
3. Make a commit that includes **_only_** the changes for the assignment, using a [good commit message](https://chris.beams.io/posts/git-commit/) that describes what the commit is about. (_hint:_ What do you need to do first? Why might your changes be rejected?)
4. Push your changes to the remote branch.
5. Create a new Pull Request on GitHub.com from your branch to `main` and provide a message according to this schema: `<assignment> assignment for <full-name>"`.

**_IMPORTANT:_** Do not merge Pull Requests yourself.

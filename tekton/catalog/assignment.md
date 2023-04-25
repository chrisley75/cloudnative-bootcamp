# Tekton Catalog Assignment

## Standards

- Use the Task Catalog to run `Tasks`

## Instructions

For this assignment, make a new folder named `<your-name>/tekton-catalog-assignment` in the `assignments` repository.

Before beginning this assignment, create a new OpenShift project with a unique name:

```shell
$:> oc new-project <your-name>-tekton-catalog-assignment
```

Read the output to ensure that your terminal is now connected to the correct workspace or confirm with `oc config current-context`. If you want to see a list of existing projects you can use `oc projects`.

Use the Tekton catalog to solve the following problems.

**_Recall:_** Sometimes Catalog tasks do not "work out of the box." How will you "make it work?" What would you do on a client site if this happens?

### Problem 1

Use `curl` in a `TaskRun` to retrieve 2 dog facts from [Dog-facts-API](https://dukengn.github.io/Dog-facts-API/). Your YAML file should be named `curl.TaskRun.yaml`.

Made a commit to the `assignments` repository that contains `curl.TaskRun.yaml`.

### Problem 2

Use the [`github-add-comment` Tekton Catalog Task](https://github.com/tektoncd/catalog/blob/45742091e57f996e4ea758bc7404f59a4f04e952/task/github-add-comment/0.4/README.md) to add a comment on [this GitHub Issue](https://github.com/upslopeio/ibm-cloud-garage-training/issues/9) which lives on a public repository. Your YAML file should be named `add-comment-to-issue.TaskRun.yaml`. Please keep the comments work appropriate and positive. Made an additional commit to the `assignments` repository that contains `add-comment-to-issue-task-run.yaml`.

### Problem 3 (Stretch/Optional)

Look at the [Tekton Catalog's Task List](https://github.com/tektoncd/catalog/tree/main/task). What is missing? Come up with a proposal for a missing Tekton task that you could use on a client site (or would have had a past use for). Write that task, include in comments your rationale for choosing this `Task`. Make a commit with this `Task` and/or `TaskRun`.

## Submit the Assignment

1. `cd` to the root of the `assignments` repository.
2. if you have not already created a new branch, follow [the steps](../../git/git-workflow-step-by-step.md) outlined in the git workflow to create a new branch.
3. Make a commit that includes **_only_** the changes for the assignment, using a [good commit message](https://chris.beams.io/posts/git-commit/) that describes what the commit is about. (_hint:_ What do you need to do first? Why might your changes be rejected?)
4. Push your changes to the remote branch.
5. Create a new Pull Request on GitHub.com from your branch to `main` and provide a message according to this schema: `<assignment> assignment for <full-name>"`.

**_IMPORTANT:_** Do not merge Pull Requests yourself.

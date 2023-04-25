# Tekton Pipeline Build - Kustomize and Triggers Assignment

## Standards

- Use the Task Catalog to build a `Pipeline` that clones a Git repository, runs the tests, creates a Docker image, pushes the image to quay.io, uses Kustomize to generate YAML, and has an available trigger for the build

## Instructions

This assignment builds on the [`buildah` assignment](../03-buildah/assignment.md). **If you have not completed that assignment, please complete it first.**

Return to your express application folder. You will again use the application you created earlier, likely in `~/workspace` of your local machine.

### Problem 1 - Add Kustomize to your Application

First, `cd ~/workspace/<path-to-your-application>`.

In your application create a `k8s` folder and add the necessary files for `kustomize`. Remember that you can develop with `kustomize` locally to see how your YAML will turn out.

Next, modify your `Pipeline` to use Kustomize.

When you complete this make a commit to the GitHub repository that contains your application.

### Problem 2 - Add Triggers to the Build

Add Build Triggers to your pipeline.

Since we have not yet added GitHub commit hooks, _how can you test your Trigger?_

Make a file named `README.md` in the `<your-name>/tekton-pipeline-assignment` folder. In the file answer these questions:

```
1. What is a Tekton Trigger? What Tekton entities are required for this to work with our Pipeline?
1. Since we have not yet added GitHub commit hooks, how did you test your Trigger?
```

_Hint:_ `curl` may be helpful

The `<your-name>/tekton-pipeline-assignment` folder should contain a file named `repository-url.txt` from the `buildah` assignment. If not, copy the URL to your Express repository into the text file and commit it.

#### Stretch (Optional)

Research and implement a solution to connect your GitHub code repository to the Trigger created in this assignment.

## Submit the Assignment

**_IMPORTANT:_** Please make sure **all** members of the Pair submit the assignment.

1. `cd` to the `assignments` repository.
2. Follow [the steps](../../git/git-workflow-step-by-step.md) outlined in the git workflow to create a new branch.
3. Create a new file named `<your-name>/tekton-pipeline-assignment/triggers.md`.
4. Add the names of **all** members of the Pair to the `triggers.md` file and save it.
5. Make a commit that includes **_only_** the changes for this assignment. (_hint:_ What do you need to do first? Why might your changes be rejected?)
6. Push the changes to the remote branch.
7. Create a new Pull Request on GitHub.com with the title `triggers Assignment for <your-name>`

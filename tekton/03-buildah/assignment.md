# Tekton Pipeline Build - buildah Assignment

## Standards

- Use the Task Catalog to build a `Pipeline` that clones a Git repository, runs the tests, creates a Docker image, pushes the image to quay.io

## Instructions

For this assignment the two problems will build on each other. **_Do not_** create the application for this assignment in the `assignments` repository, instead create a new folder in `~/workspace` of your local machine (or any folder other than the one where you have cloned the `assignments` repository).

### Common Errors

| Error Message                                                            | Solution                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| MatchExpressions:[]v1.LabelSelectorRequirement(nil)}: field is immutable | labels used for deployment, et al, are immutable. In other words, once a deployment is created, changing the label selector is not allowed. Your only option is to either use a new name for the deployment with the new labels or delete the existing deployment so it can be recreated with the new labels. See https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#label-selector-updates for more information |

### Problem 1 - Create the Application

First, switch into your workspace folder (e.g. `~/workspace`).

Create a new application with the [Express generator](https://expressjs.com/en/starter/generator.html) by running: `npx express-generator <application-name>`, name it however you would like.

Then, `cd <application-folderr>` and run `npm install`

Then, add your name (or everyone's name if working in a pair/ensemble) to `view/index.jade` so it looks like the following:

```pug
extends layout

block content
  h1= title
  p Welcome to #{title}
  p Created by: <replace-this-with-names>
```

Next, create a `Dockerfile` for the application that uses a [Quay.io](https://quay.io) hosted version of the node image to run it, like [this one](https://quay.io/repository/ibmgaragecloud/node).

Make sure you add `.gitignore` and `.dockerignore` files that include the `node_modules` folder and commit your changes.

Create a new GitHub repository in the cohort's GitHub team.

Commit and push your changes to the remote repository.

### Problem 2

In your project, create a `tekton` folder. In the `tekton/` folder, create a CI Pipeline that uses `buildah` to build your image and push it to quay.io.

When you have completed this and see the image in Quay.io, create a screenshot of your pushed image in your quay.io account, name it `image.png` (or the appropriate extension).

In the `assignments` repository, make a new folder named `<your-name>/tekton-pipeline-assignment` and put the `image.png` into this folder.

Next create a new file named `repository-url.txt` in the `tekton-pipeline-assignment` folder. Copy the URL to your source code repository into the text file and commit it.

### Stretch (Optional)

So far you have only built Node applications, in part for simplicity of build steps. While Node is being used more frequently in the Enterprise, many re-platforming efforts will more likely be based on C#/.NET applications or Java/Spring.

For this Stretch problem, you will create a pipeline that uses buildah to build a new application, you may pick your challenge:

1. _C#/.NET_ - Follow [this guide](https://docs.microsoft.com/en-us/dotnet/core/docker/build-container?tabs=windows) and Dockerize a .NET Core application. Build a pipeline that uses buildah to build the application.
1. _Java/Spring_ - Follow [this guide](https://spring.io/guides/gs/spring-boot-docker/) and Dockerize a Spring application. It may be helpful to know about the [Maven Tekton Task](https://github.com/tektoncd/catalog/tree/main/task/maven/0.2).

## Submit the Assignment

**_IMPORTANT:_** Please make sure **all** members of the Pair submit the assignment.

1. `cd` to the `assignments` repository.
2. Follow [the steps](../../git/git-workflow-step-by-step.md) outlined in the git workflow to create a new branch.
3. Create a new file named `<your-name>/tekton-pipeline-assignment/buildah.md`.
4. Add the names of **all** members of the Pair to the `buildah.md` file and save it.
5. Make a commit that includes **_only_** the changes for this assignment. (_hint:_ What do you need to do first? Why might your changes be rejected?)
6. Push the changes to the remote branch.
7. Create a new Pull Request on GitHub.com with the title `buildah Assignment for <your-name>`

# Tekton Pipeline Build - `git-clone` Assignment

## Standards

- Define `Task`, `Pipeline`, and `PipelineRun`
- Use the Task Catalog to build a `Pipeline` that clones a Git repository and uses a Catalog Task on the cloned repository

## Instructions

For this assignment, make a new folder named `<your-name>/tekton-git-clone-assignment` in the `assignments` repository.

For this assignment you will use the [`git-clone`](https://github.com/tektoncd/catalog/tree/main/task/git-clone/0.3) Task from the Tekton Catalog.

### Problem 1

[Linting](https://stackoverflow.com/questions/8503559/what-is-linting#:~:text=Linting%20is%20the%20process%20of,in%20C%20language%20source%20code.) is one process that helps eliminate errors in production code. Before linting production code or Dockerfiles, we start with a simpler example, Markdown.

Find a Tekton Task for Linting Markdown files. You will use [this public GitHub repository](https://github.com/upslopeio/lint-markdown-files) as an input. Create a Pipeline that uses `git-clone` and the `Task` you found to lint the Markdown files in the provided repository.

If the pipeline **fails** with the following error, then you are done:

    STEP-LINT-MARKDOWN-FILES

      ./README.md:3: MD013 Line length
      ./README.md:4: MD013 Line length
      ./README.md:5: MD013 Line length
      ./README.md:7: MD013 Line length
      ./README.md:9: MD013 Line length
      ./README.md:11: MD013 Line length
      ./README.md:13: MD013 Line length
      ./SECOND_EXAMPLE.md:18: MD009 Trailing spaces
      ./SECOND_EXAMPLE.md:3: MD013 Line length
      ./SECOND_EXAMPLE.md:4: MD013 Line length
      ./SECOND_EXAMPLE.md:8: MD013 Line length
      ./SECOND_EXAMPLE.md:12: MD013 Line length
      ./SECOND_EXAMPLE.md:16: MD013 Line length
      ./SECOND_EXAMPLE.md:18: MD013 Line length
      ./SECOND_EXAMPLE.md:21: MD013 Line length
      A detailed description of the rules is available at https://github.com/markdownlint/markdownlint/blob/master/docs/RULES.md

## Commit changes

Make a commit to the `assignments` repository with the following files in `<your-name>/tekton-git-clone-assignment/problem-1`:

1. any YAML files for this problem, AND
2. a text file named `markdown-lint.Pipeline.txt` that contains the Logs of your pipeline run.

### Problem 2

[YAML](https://yaml.org/) is the format used by Kubernetes and Tekton. Find an open source Tekton Task for Linting YAML files.

Next, create a _public repository_ in the cohort's GitHub organization. In that repository put a mixture of valid and invalid YAML files.

Create a Pipeline that uses `git-clone` and the `Task` you found to lint the YAML files in your repository.

Correct your invalid YAML and commit that to the repository you made. Re-run your pipeline.

Make a commit to the `assignments` repository with the following files in `<your-name>/tekton-git-clone-assignment/problem-2`:

1. any YAML files for this problem, AND
2. Copy the URL to your repository into the `repository-url.txt`.
3. a text file, named `yaml-linting.Pipeline.txt`, that contains the Logs of your pipeline run.

### Stretch 1

Public repositories are not going to be the normal approach on a client site. Instead, repositories will be private and our pipelines will need to know how to interact with them.
Find an open source Tekton Task, or write your own, to interact with private repositories.

To show you have done this, create a new, private repository with only the valid YAML from problem 2 (valid YAML syntax). Create a Pipeline that reads from this private repository and runs the linter.

Make a commit to the `assignments` repository with the following files in `<your-name>/tekton-git-clone-assignment/stretch-1`:

1. all YAML files for this problem (remember to obfuscate any secret before committing the yaml files, e.g. `password: <YOUR-SECRET>`)
2. a text file, named `repository-url.txt`, that contains the URL to your repository
3. a text file, named `private-repository.Pipeline.txt`, that contains the logs of your pipeline run

### Stretch 2

Review the [Tekton Catalog Tasks](https://github.com/tektoncd/catalog/tree/main/task) and choose one of these tracks:

1. Find a Task that sounds familiar and useful and try to implement it by adding it to one of the pipelines you created in this assignment.
1. Git to the max: Create a GitHub repository with any content and two branches that differ (in any way), named `main` and `dev`. Write a Tekton Pipeline using Catalog tasks that merges (your choice of strategy) the contents `dev` into `main`.
1. Documentation improver: Find out how to publicly contribute to the Tekton Task Catalog and improve the documentation in the spots that were pain points today (_hint:_ one spot is the `git-clone` documentation's missing Installation Instructions). | The solution to remove a change depends on the change. Is it a deleted file? Restore the file. Is it a new file? Delete the file. |

## Submit the Assignment

**_IMPORTANT:_** Please make sure **all** members of the Pair submit the assignment.

1. `cd` to the `assignments` repository.
2. Follow [the steps](../../git/git-workflow-step-by-step.md) outlined in the git workflow to create a new branch.
3. Create a new file named `<your-name>/tekton-git-clone-assignment/README.md`.
4. Add the names of **all** members of the Pair to the `README.md` file and save it.
5. Make a commit that includes **_only_** the changes for this assignment. (_hint:_ What do you need to do first? Why might your changes be rejected?)
6. Push the changes to the remote branch.
7. Create a new Pull Request on GitHub.com

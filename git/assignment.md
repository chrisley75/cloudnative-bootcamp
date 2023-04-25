# Git Assignment

## Standards to Achieve

By completing this assignment, you demonstrate the following Standards:

- Explain the add/commit/push flow including untracked files
- Clone a GitHub repo
- Create a repo locally and push it to a remote
- Add / commit / pull / push changes
- Set config for your username / email, ignorecase...
- Create branches, make commits on branches and push code to branches
- Create pull requests and merge pull requests

## Important

This assignment sets you up for the remainder of this course. All assignments will rely on Git for submission to the `assignments` repository, so understanding how to interact with a Git repository is key.

Since these instructions are shared between cohorts, we will refer to the repository as `assignments`. You will likely need to change these instructions to something _like_ `cohort-XYZ-assignments`, depending on the cohort-specific name of the repository.

If you are fairly new to git, use the [Git Workflow - Step-by-step instructions](./git-workflow-step-by-step.md) as your guide until you get comfortable with git.

## Instructions

1. First, begin by cloning the provided cohort assignment repo (check the cohort's Slack channel for this - it is pinned to the channel).

2. At a terminal change into the `assignments` folder (i.e. where you cloned the `assignments` repository).

3. Follow [the steps](../git/git-workflow-step-by-step.md) outlined in the git workflow to create a new branch.

4. Next, create a folder named `<your-name>/git-assignment/`, using [kebab-case](https://en.wikipedia.org/wiki/Letter_case#Special_case_styles), e.g. if your name is `John Doe` your folder will be `john-doe`. Be careful to make sure your folder name does not contain spaces. (This identifies the assignment as belonging to you. All future assignments will be sub-folders of the `<your-name>` folder.).

5. Inside the `git-assignment` folder, create a text file named `README.md`.

6. Inside of `README.md` paste the following Markdown-formatted text, and replace each `` `<your answer here>` `` with your responses:

```markdown
# Git Questions

- What is _one_ command to create _and_ switch to a new branch?

  `<your answer here>`

- Describe the lifecycle of a file in a Git repository, **AND** include the commands that change the state of the file.

  `<your answer here>`

- Why do developers work in branches?

  `<your answer here>`

- What is the command to change to an existing branch?

  `<your answer here>`

- Have you used Git before? If so, in what context? Did you team used branches? Pull Requests? Some other approach?

  `<your answer here>`
```

## Submit the Assignment

1. `cd` to the root of the `assignments` repository.
2. if you have not already created a new branch, follow [the steps](../git/git-workflow-step-by-step.md) outlined in the git workflow to create a new branch.
3. Make a commit that includes **_only_** `<your-name>/git-assignment/README.md`, using a [good commit message](https://chris.beams.io/posts/git-commit/) that describes what the commit is about. (_hint:_ What do you need to do first? Why might your changes be rejected?)
4. Push your changes to the remote branch.
5. Create a new Pull Request on GitHub.com from your branch to `main` and provide a message according to this schema: `<assignment> assignment for <full-name>"`.

**_IMPORTANT:_** Do not merge Pull Requests yourself.

### Common Pull Request Issues

| Issue                                                          | Solution                                                                                                                          |
| -------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| Contains files outside of `<your-name>/git-assignment/` folder | `mv` the files to the correct location under your folder.                                                                         |
|                                                                | You have changes that are not related to this assignment.                                                                         |
|                                                                | Merge in changes from main and push the branch again. The changes you see in the PR at this point do not exist in main.           |
|                                                                | The solution to remove a change depends on the change. Is it a deleted file? Restore the file. Is it a new file? Delete the file. |

## I am done, what now?

_Note:_ During this course we will attempt to provide supplemental material beyond the core material for anyone who finishes early. This material is supplemental and will not be reviewed as part of the main course flow, but will help you improve rapidly.

If you have finished this assignment early, the next step to advance your Git mastery is to begin on [Git Katas](https://github.com/eficode-academy/git-katas).

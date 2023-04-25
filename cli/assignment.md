# CLI Assignment

## Standards to Achieve

By completing this assignment, you demonstrate the following Standards:

- Change directories (up, down)
- Print the working directory
- List the contents of a directory
- Move and copy files
- Navigate history using arrow keys and `history` command
- Clear the screen using both `clear` and `CMD+K`
- Create and edit a file at the command line using `touch` and `vim` or `code`
- Describe how `PATH` works
- Determine if an executable is in `PATH` with `which`

## Instructions

1. Change into the `assignments` folder (i.e. where you cloned the `assignments` repository).

2. At a terminal checkout the `main` branch with `git checkout main`.

3. Make sure your `main` branch is up-to-date with the remote. For instance, with `git pull`.

4. Create a new branch with a unique name, identifying you _and_ the assignment according to this schema: `<first>-<last>-<assignment>`. For example: `john-doe-cli`.

5. Next, create a folder named `<your-name>/cli-assignment`, identifying this assignment.

6. [Shell Scripts](https://www.shellscript.sh/) are a convenient way use CLI commands repeatably. For this exercise, you will create a Shell Script named `cli.sh`. The behavior of this script is as follows:

   1. generates a file named `cli-output.txt`
   2. populates the contents of `cli-output.txt` with the following question and its answer: "Describe how PATH works?"
   3. creates a directory named `archive`, if the directory already exists, your script should continue to work
   4. copies `cli-output.txt` into a file named `how-path-works.txt` in `archive/`
   5. overwrites the contents of `cli-output.txt` with the following question and its answer: "What command shows us previously run commands?"
   6. copies `cli-output.txt` into a file named `previously-run-commands-command.txt` in `archive/`
   7. writes to the screen `"Script run successful."`

_Recall:_ To run your script locally, you will need to modify its permissions with `chmod ????` - what fills in the `????`?

8. Once you are done with this,

## Submit the Assignment

1. `cd` to the root of the `assignments` repository.
2. if you have not already created a new branch, follow [the steps](../git/git-workflow-step-by-step.md) outlined in the git workflow to create a new branch.
3. Make a commit that includes **_only_** `cli.sh`, using a [good commit message](https://chris.beams.io/posts/git-commit/) that describes what the commit is about. (_hint:_ What do you need to do first? Why might your changes be rejected?)
4. Push your changes to the remote branch.
5. Create a new Pull Request on GitHub.com from your branch to `main` and provide a message according to this schema: `<assignment> assignment for <full-name>"`.

**_IMPORTANT:_** Do not merge Pull Requests yourself.

If you are done and there is still time remaining for the exercise, move on to the next section "I am done, what now?"

### Common Pull Request Issues

| Issue                                                                                                 | Solution                                                                                                                          |
| ----------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| Contains files outside of `<your-name>/cli-assignment/` folder                                        | `mv` the files to the correct location under your folder.                                                                         |
|                                                                                                       | You have changes that are not related to this assignment.                                                                         |
|                                                                                                       | Merge in changes from main and push the branch again. The changes you see in the PR at this point do not exist in main.           |
|                                                                                                       | The solution to remove a change depends on the change. Is it a deleted file? Restore the file. Is it a new file? Delete the file. |
| Contains `cli-output.txt`, `archive`, `how-path-works.txt` , or `previously-run-commands-command.txt` | Delete these files and also add them to `<your-name>/cli-assignment/.gitignore`                                                   |

## I am done, what now?

If you have finished this assignment early, the next step to advance your CLI mastery is to begin on [Unix Command Exercises](https://nsrc.org/workshops/2018/afnog-bootcamp/exercises/exercises-commands.md.htm).

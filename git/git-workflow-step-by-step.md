# Git Workflow - Step-by-step

## Starting a new assignment

This assumes an existing git repository is cloned to your local machine, and you have cd's into the repository folder:

1. `git switch main`
2. `git pull`
3. `git switch -c <first-last-name>-<assignment-name>`
4. Do your work - create folders and files, etc.
5. `git add -A`
6. `git commit -m "<assignment-name> for <first-last-name>"`
7. cd to the root of the assignment repository.
8. `git switch main`
9. `git pull`
10. `git switch <first-last-name>-<assignment-name>`
11. `git merge main`
    1. this may open `vi` or `vim` to write the commit message. Press `esc` then `:wq` to save the file and quit.
12. `git push` (you may need to use `git push --set-upstream origin <branch-name>` git will let you know)
13. Create a new Pull Request on GitHub.com from your branch to `main` and provide a message according to this schema: `<assignment> assignment for <full-name>"`. For instance, `"git assignment for John Doe"`.
    1. On GitHub.com verify your Pull Request. Ensure that it contains all the required files, and no extra files.
    2. Let the instructor merge the Pull Requests. Do not merge PR's yourself.
    3. Assignments with unrelated or incorrectly located files will not be merged. e.g.; `.DS_Store` files, files outside `<your-name>` folder.
    4. Assignments with missing fill not be merged. Make sure you commit all the files requested in the assignment.
    5. Watch out for feedback from the instructor (check the email account associated with your GitHub account). Your goal is to have the Pull Request merged by one of the instructors.
14. When you start the next assignment, start at step 1
    1. you may need to create some folders again because the earlier assignment has not been merged again. This is okay, git does not track folders.

## Updating a previously created Pull Request

Note: Changes to the pull request branch will automatically update the pull request. There is no need to create a new pull request.

1. `git switch <original-branch-name>`
2. Do your work - create folders and files, etc.
3. `git add -A`
4. `git commit -m "<assignment-name> for <first-last-name>"`
5. cd to the root of the assignment repository.
6. `git switch main`
7. `git pull`
8. `git switch <original-branch-name>`
9. `git merge main`
   1. this may open `vi` or `vim` to write the commit message. Press `esc` then `:wq` to save the file and quit.
10. `git push`

## Common PR Issues

| Instructor comment                                       | description                                                                                                                                        | Solution                                                                                                                                                                                                                                 |
| -------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Please only add files under `<your-name>` folder         | You added files to a folder other then your own folder.                                                                                            | `mv` the files to the correct location under your folder.                                                                                                                                                                                |
| Please include only one assignment per Pull Request (PR) | You have changes in the PR that are not related to the assignment in the PR title. These may be deleted files, other assignments, almost anything. | Merge in changes from main and push the branch again. The changes you see at this point do not exist in main. The solution to remove this change depends on the change. Deleted file? Restore the file. New file added? Delete the file. |

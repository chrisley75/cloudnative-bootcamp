# Git

## Standards

- Explain the add/commit/push flow including untracked files
- Clone a GitHub repo
- Create a Git repo locally and push it to a remote (ie GitHub)
- Add / commit / pull / push changes
- Set config for your username / email, ignorecase...
- Create branches, make commits on branches and push code to branches
- Create pull requests and merge pull requests

## Lesson

[Git](https://git-scm.com/) is a free and open source, distributed source code repository management tool. For the purpose of this course, we will use [GitHub](https://github.com), an industry standard for Git repository management.

So, [what is Git?](https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F#what_is_git_section)

> Git is a version control system (VCS). Unlike other VCS solutions, Git does not store data as a series of changesets or differences, but instead as a series of snapshots.

An important principle in Git is the file lifecycle. What does that mean?

1. files begin as `untracked`
1. a file is then added to the repository using `git commit`
1. the local version of the repository is synced with the remote via `git pull --rebase`
1. any merge conflicts are dealt with and, potentially, commits are made
1. local changes are pushed to the remote via `git push`

When a file changes, we use the same process. This forms a series of snapshots of a miniature filesystem, as it evolves over time. This visualization from [Getting Started - What is Git?](https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F) helps illustrate this:

![](https://git-scm.com/book/en/v2/images/snapshots.png)

To be efficient, if files have not changed, Git doesn’t store the file again, just a link to the previous identical file it has already stored. Git thinks about its data more like a **stream of snapshots**.

The following is a graphical representation of the overall process with the commands to show state transitions ([source](https://support.nesi.org.nz/hc/en-gb/articles/360001508515-Git-Reference-Sheet)):

![](https://support.nesi.org.nz/hc/article_attachments/360004194235/Git_Diagram.svg)

Let's make this more concrete by doing an example.

Your instructor will create the `assignments` repository that will be used throughout the course.

The first step is to initialize the repository locally:

```shell
$:> mkdir -p ~/workspace/assignments
$:> cd assignments
$:> git init
```

You should see output similar to:

```shell
Initialized empty Git repository in /Users/<your-user-name>/workspace/assignments/.git/
```

So far, this repository is empty.

**Git Repository convention:** each repository should contain a `README.md` which describes how to use and/or set up the repository. So we create the `README`:

```shell
$:> touch README.md
```

Using a text editor, we will populate the repository's `README.md` with the following Markdown contents:

```markdown
# Assignments Repository

## Purpose

This repository is where all text files will be submitted for this course. For much of this course we will use a mixture of text files with written answers and submitted YAML to demonstrate progress on assignment.

## Directions

After cloning this repository, create a folder with your name, using [kebab-case](https://en.wikipedia.org/wiki/Letter_case#Special_case_styles), e.g. if your name is `Andreas Kavountzis`, your folder will be `andreas-kavountzis`. Be careful to make sure your folder name does not contain spaces.

**Note:** All of your work goes in your named folder.

## FAQ

- Is having a single repository for all students' work problematic?

  No. All of your work will occur in your named folder, so you should not have conflicts with your classmates' work. Additionally, this will get you in the habit of `git pull` / `git push` as a normal part of everyday work.
```

if we check the `status` of the repository:

```shell
$:> git status
```

we see:

```shell
$:> git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md

nothing added to commit but untracked files present (use "git add" to track)
```

if we look carefully, Git is telling us something important, `nothing added to commit but untracked files present (use "git add" to track)`.

We have one (1) untracked file, `README.md`. In other words, the repository has never seen this file before, therefore it cannot track its revisions.

The command output above tells us to use `git add` to track the file, so we follow issue the command:

```shell
$:> git add README.md
```

if we check the status of the repository again:

```shell
$:> git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   README.md
```

our file is now _staged_ or, in other words, _ready to be committed_. Now we can create our commit with a message:

```shell
$:> git commit -m "Initial commit"
[master (root-commit) 715f586] Initial commit
 1 file changed, 17 insertions(+)
 create mode 100644 README.md
```

Since we are using GitHub, [these directions](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-repository-on-github/creating-a-new-repository) are great for showing how to create a repository.

Before following the commands from GitHub's UI, make sure you have `git` configured correctly:

```shell
$:> git config --global --list
```

If these values are missing or wrong, change them with:

```shell
$:> git config --global user.name "John Doe"
$:> git config --global user.email johndoe@example.com
```

Recently, teams have moved away from using `master` as the primary branch name, for this course we will use `main`:

```shell
$:> git branch -M main
```

Finally:

```shell
$:> git push origin main
```

Returning to GitHub we see the contents of our repository in the cloud.

#### Branching

The `main` branch serves a special purpose for teams that practice CI/CD. Since the code is always being shipped to production, we consider `main` as _the version of the code running in production, right now_. Similarly, our GitOps practice later in this course will use `main` to represent the _currently applied, desired state of the infrastructure_. The name `main` is a traditional choice, but any team-agreed upon name works.

So what about a place to do work? If the `main` branch is what is currently deployed, then how do developers actively work on features? Wouldn't that break production?

One solution is [Git branches](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell). A branch is a divergence from `main` at a given point in time. In order to bring branches back together, developers perform a process known as ["merging"](https://git-scm.com/docs/git-merge). The skills behind merging are out of scope for this course; the course materials are constructed so that you will use a Pull Request to submit your work, but that the Pull Request will not require a manual merge.

There are many ways to create a branch, this is just one:

```shell
$:> git checkout -b <branch-name>
```

To push a branch to the remote:

```shell
$:> git push origin <branch-name>
```

#### Pull Requests

Conceptually a Pull Request represents a developer (or pair) asking if changes are acceptable to go into the `main` branch. Mapping this into the CI/CD world, a Pull Request represents a developer asking if changes are ready to go to production.

Pull requests, in the format used here, are a part of the GitHub.com platform, as opposed to Git itself.

Now that we have pushed a branch to the remote, we click "Compare and Pull Request" in the UI to create a Pull Request.

# Command Line Interface (CLI)

## Standards

- Change directories (up, down)
- Describe how `PATH` works
- Change `PATH` in `.zshrc`
- Determine if an executable is in `PATH` with `which`
- Print the working directory
- List the contents of a directory
- Move and copy files
- Navigate history using arrow keys and history command
- Clear the screen using both `clear` and `CMD+K`
- Create and edit a file at the command line using `touch` and `vim`

## Lesson

The [Command Line Interface (CLI)](https://en.wikipedia.org/wiki/Command-line_interface) is a powerful, text-based way to interact with computers. The CLI is a primary tool for using Docker, interacting with a Kubernetes cluster, and building Tekton Pipelines. Since the computers that run applications do not have monitors (i.e. "headless"), text is our only way to interact with these machines.

Files and directories are the building blocks of the [Unix filesystem](https://en.wikipedia.org/wiki/Unix_filesystem). The base folder is `/`. Your user's (`whoami`) home folder is `~`.

If we _change directory_ and _print working directory_ we can see the location of our home folder, relative to the filesystem root path, `/`:

```shell
$:> cd ~
$:> pwd
/Users/<your-user-name>
```

With the CLI we can complete [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (or CRUDL) on files, directories, applications, etc.

CRUDL stands for:

- Create
- Read
- Update
- Delete
- List - a common extension to this paradigm

CRUDL is a dominant pattern for input/output across various resource types, from entries in a database to files on your hard drive to JSON-based representations used by web application you use. Can you think of an example and generate a table like the one below?

The cycle for a file is as follows:

| Action | Command                              |
| ------ | ------------------------------------ |
| Create | `touch <file-name>.<file-extension>` |
| Read   | `cat <file-name>.<file-extension>`   |
| Update | `vim <file-name>.<file-extension>`   |
| Delete | `rm <file-name>.<file-extension>`    |
| List   | `ls`                                 |

The cycle for a directory is as follows:

| Action      | Command                                            |
| ----------- | -------------------------------------------------- |
| Create      | `mkdir <directory>`                                |
| Read        | `ls <directory>`                                   |
| Update/Move | `mv <existing-directory> /path/to/<new-directory>` |
| Delete      | `rm -rf <directory>`                               |
| List        | `ls`                                               |

Sometimes it can be helpful to copy files or directories, in both cases we use the `cp` command:

| Type      | Command                                                |
| --------- | ------------------------------------------------------ |
| file      | `cp <source-file> <target-file>`                       |
| directory | `cp -R <source-directory> /path/to/<target-directory>` |

In the above command for copying a directory, we see the flag `-R` which means

> If `source_file` designates a directory, `cp` copies the directory and the entire subtree connected at that point.

To see more about a command's usage we can use `man cp`. `man` (short for _manual_) pages are largely a relic of the past, and you will use Google to answer most questions.

For utilities that we install, typically to see usage we will do something like: `<cli-utility> --help`, `<cli-utility> -h` or `<cli-utility> help`.

When we install utilities at the CLI, our machine needs to know:

- where to look for the installed utility or _binary_ of the application
- in what order to look in those locations

For example, if we install the toy CLI application [Nyancat](https://github.com/klange/nyancat) with `brew`:

```shell
$:> brew install nyancat
```

we now have the `nyancat` command available to us:

```shell
$:> nyancat
```

We see a fun display that we stop with `CTRL+C` (on Mac).

If we want to find out where `nyancat` is installed, we can use the `which` command:

```shell
$:> which nyancat
/usr/local/bin/nyancat
```

If we investigate further, using `ls -lah`:

```shell
$:> ls -lah /usr/local/bin/nyancat
lrwxr-xr-x  1 andreaskavountzis  admin  35B May 13 23:26 /usr/local/bin/nyancat -> ../Cellar/nyancat/1.5.2/bin/nyancat
```

on the far left we see the file's [permissions for read, write, and execution](https://www.linux.com/training-tutorials/understanding-linux-file-permissions/), followed by the user (`andreaskavountzis`), the user's group (`admin`), the size and unit (`35B`), the time of the last file modification (`May 13 23:26`), and in our case, a [symlink](https://man7.org/linux/man-pages/man1/ln.1.html) to a different location that has the binary, `../Cellar/nyancat/1.5.2/bin/nyancat`.

If we example the contents of `../Cellar/nyancat/1.5.2/bin/nyancat` with `cat`, we will see a bunch of garbled text since we are attempting to print the contents of a binary file:

```shell
$:> cat /usr/local/Cellar/nyancat/1.5.2/bin/nyancat
```

The way that Homebrew works is it create a `Cellar` which has references to the software binaries. The power behind Homebrew is that it takes care of making sure that your machine can find the correct piece of software to execute, with minimal configuration on your part.

But how does your machine know where to find CLI applications? How was `which` able to locate the directory of our installed software?

What ties this together is the [`PATH` variable](<https://en.wikipedia.org/wiki/PATH_(variable)>). The `PATH` variable contains an ordered list of directories that that CLI uses to look for applications on the CLI, we can inspect the value of `PATH` (or any other CLI variable) with `echo`:

```shell
$:> echo $PATH
/Users/andreaskavountzis/.rvm/gems/ruby-2.7.0/bin:/Users/andreaskavountzis/.rvm/gems/ruby-2.7.0@global/bin:/Users/andreaskavountzis/.rvm/rubies/ruby-2.7.0/bin:/Users/andreaskavountzis/bin:/usr/local/opt/yq@3/bin:/Users/andreaskavountzis/Downloads/google-cloud-sdk/bin:/Users/andreaskavountzis/.nvm/versions/node/v12.20.0/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/share/dotnet:~/.dotnet/tools:/Library/Frameworks/Mono.framework/Versions/Current/Commands:/Applications/Xamarin Workbooks.app/Contents/SharedSupport/path-bin:/Users/andreaskavountzis/.rvm/bin
```

What happens if we try a nonsensical command like `xhsdgsda`?

```shell
$:> xhsdgsda
zsh: command not found: xhsdgsda
```

In other words, there is no binary in any directory listed in `$PATH` that matches `xhsdgsda`, the CLI indicates `command not found`.

Sometimes your terminal can become hard to follow, in which case you can clear the screen with either:

- `clear` - CLI utility to clear terminal
- `⌘ + K` - keyboard shortcut to clear terminal

The final concept is history, there are two techniques for this:

- `up arrow`/`down arrow` - this allows you to scroll through commands
- `history` - this CLI tool shows a log of the commands run at your machine

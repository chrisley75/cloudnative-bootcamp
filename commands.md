# Commands

The most powerful way to find your way around the Command-line is to use the build in help of the commands.

Most commands provide useful information on how to use them if you call them without arguments, or with arguments like `--help`, `-h`, and `help`:

```bash
git
git help commit
oc
oc --help
oc logs -h
```

Well maintained CLI tools also come with a manual. You can open the manual with the `man` command:

```bash
man git
man curl
```

Then, there is tons of documentation on the internet.

And some people prefer to create a Command Cheat Sheet as they go along.

Using the built-in help or man pages is like finding your way around with a map and a compass. Using a cheat sheet is like using a navigation system like Google Maps.

A navigation system is nice, but useless when you don't have reception, or when you ran out of battery. Maps and compasses don't need to be recharged and work without reception.

## Command Cheat Sheet

| COMMAND                                   | DESCRIPTION                                 |
| ----------------------------------------- | ------------------------------------------- |
| **CONTEXT SWITCHING**                     |
| `kubeon`                                  | show current context in command-line prompt |
| `kubeoff`                                 | hide current context in command-line prompt |
| `icc --generate`                          | re-create list of clusters                  |
| `icc`                                     | list clusters                               |
| `icc <cluster>`                           | switch cluster                              |
| `oc config current-context`               | show current config                         |
| `oc new-project <namespace>`              | create project                              |
| `oc project <namespace>`                  | switch project                              |
| `oc console`                              | open web console                            |
| **RESOURCES**                             |
| `oc create -f <yaml-file>`                | create resource from yaml file              |
| `oc apply -f <yaml-file>`                 | update resource from yaml file              |
| `oc get tasks`                            | list tasks                                  |
| `oc get taskruns`                         | list taskruns                               |
| `oc get pods`                             | list pods                                   |
| `oc get pods --sort-by=.status.startTime` | list pods sorted by start time              |
| `oc describe pod <pdf>`                   | describe pod                                |
| `oc logs <pod> -f`                        | print logs and wait for more logging        |
| **TASKS**                                 |
| `tkn task start <task>`                   | start task                                  |
| `tkn taskruns list`                       | list taskruns                               |
| `tkn taskrun logs <taskrun> -f`           | print logs of taskrun                       |

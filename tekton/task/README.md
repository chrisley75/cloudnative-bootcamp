# Tekton `Task`

## Standards

- Define Tekton `Task`
- Write a Tekton `Task` and run it via `TaskRun`

## Lesson

> Tekton is a cloud native continuous integration and delivery (CI/CD) solution. It allows developers to build, test, and deploy across cloud providers and on-premise systems.

Tekton is built on Kubernetes as a set of [Custom Resource Defintions](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/). Tekton is a newer ecosystem that is rapidly evolving and has a [bold mission statement](https://github.com/tektoncd/community/blob/main/roadmap.md):

> Be the industry-standard, cloud-native CI/CD platform components and ecosystem.

We mention the new nature of Tekton because you may find that Googling for help is difficult. For example, if you start at the [Tekton Concepts](https://tekton.dev/docs/concepts/) page you will notice a section on "Input and output resources." However, if you look deeper into the project you will see that this concept has [already been deprecated between `v1alpha1` and `v1beta1`](https://github.com/tektoncd/pipeline/blob/main/docs/migrating-v1alpha1-to-v1beta1.md#replacing-pipelineresources-with-tasks). This course will not cover `PipelineResources` at all for this reason.

Tasks are a building block for Tekton Pipelines. We begin to learn about `Task` from [this documentation](https://tekton.dev/docs/pipelines/tasks/) and focus on this definition:

> A Task is a collection of Steps that you define and arrange in a specific order of execution as part of your continuous integration flow. A Task executes as a Pod on your Kubernetes cluster. A Task is available within a specific namespace, while a ClusterTask is available across the entire cluster.

Looking at the [Configuring a `Task`](https://tekton.dev/docs/pipelines/tasks/#configuring-a-task) section, the fields for a `Task`'s YAML file are split by "required" and "optional". The required list is:

- `apiVersion` - Specifies the API version, `tekton.dev/v1beta1`.
- `kind` - Identifies this resource object as a `Task` object.
- `metadata` - Specifies `metadata` that uniquely identifies the Task resource object. For example, a `name`.
- `spec` - Specifies the configuration information for this `Task` resource object.
- `steps` - Specifies one or more container images to run in the `Task`.

In order to write your own `Task`, the YAML that you write will have to contain at least the required fields.

We begin with a ["Hello, world!"](https://en.wikipedia.org/wiki/%22Hello,_World!%22_program) Tekton task:

Add the following to `hello-world.Task.yaml`:

```yaml
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: hello-world
spec:
  steps:
    - name: speak
      image: quay.io/ibmgaragecloud/alpine-curl
      script: |
        echo 'Hello, world!'
```

This task has a name of `hello-world` and, when run, prompts the user for the value of a parameter named `hello-world-message`. The value of `hello-world-message` will be displayed on standard out using the [`echo`](http://www.linfo.org/echo.html) command.

The easiest way to run this `Task` is to use `tkn`:

```shell
tkn task start hello-world
TaskRun started: hello-world-run-krgkh

In order to track the TaskRun progress run:
tkn taskrun logs hello-world-run-krgkh -f -n hello-world-task-dev
```

to see the logs we simply copy/paste the command from the output above:

```
tkn taskrun logs hello-world-run-krgkh -f -n hello-world-task-dev
```

which shows us a log _like_:

```
[first-step] + echo Hello, world!
[first-step] Hello, world!
```

What just happened? You created a `TaskRun` for your `Task` named `hello-world`. From the [documentation](https://tekton.dev/docs/pipelines/), a `TaskRun`:

> Instantiates a Task for execution with specific inputs, outputs, and execution parameters. Can be invoked on its own or as part of a Pipeline.

To make an analogy to programming, a Task is like a function (or method) where each step is defined, but not yet _called_. When we ran `tkn task start` it created a `TaskRun`, which is like _calling a function_ with its arguments. Similarly, while a function can exist on its own, it may also be used in a larger program.

For a more detailed example we use [`wget`](https://www.geeksforgeeks.org/wget-command-in-linux-unix/#:~:text=Wget%20is%20the%20non-interactive%20network%20downloader%20which%20is%20used%20to%20download%20files%20from%20the%20server). `wget` is a generic CLI data transfer tool. The image we use (`alpine:3`) includes `wget` so we can use `wget` from the `Task` to get information from the web and record it:

Add the following to `download.Task.yaml`:

```yaml
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: download
spec:
  params:
    - name: url
      type: string
  steps:
    - name: get
      image: alpine:3
      script: |
        mkdir $(params.url)
        wget $(params.url) -P $(params.url)
```

The workflow we will get used to is:

1. `oc apply -f download.Task.yaml`
2. `tkn task start download` <- `download` is not the file name, it is the value in the metadata.name field in the `download.Task.yaml`.
   When we do this we are prompted for a value for the `url`. The output after hitting enter tells us where to find the logs, it will look _like_:
   ```shell
   ? Value for param `url` of type `string`? www.google.com
   TaskRun started: download-run-krgkh
   ```
3. Copy and paste the last line of output from the previous step to see `TaskRun` logs (add `-f` to stream)

Each time we use the Tekton Pipeline CLI (`tkn`) to start a `Task`, what actually happens is that a new entity, called a `TaskRun` is generated, we can see this using `oc get trs` (or `tkn taskruns list`):

```shell
$:> oc get TaskRuns
NAME                 SUCCEEDED   REASON      STARTTIME   COMPLETIONTIME
download-run-2s6zc   True        Succeeded   26m         26m
download-run-7b6f9   True        Succeeded   29m         28m
download-run-lnqc5   False       Failed      20m         19m
download-run-pjn6f   True        Succeeded   24m         24m
download-run-pkrvr   True        Succeeded   25m         24m
download-run-t6w6v   True        Succeeded   23m         23m
```

if we want to see the logs of an individual task run, to inspect what happened, we can use `tkn taskrun logs <taskrun>`. Intuitively, you may expect that since all `TaskRun` are K8s `Pod`s then we should be able to use `oc` or `kubectl` to see the logs, however `oc logs download-run-2s6zc` displays an Error since the `Pod` has been terminated.

We extend the example a step further by using the [`head`](http://www.linfo.org/head.html) command to take the first ten lines of the `index.html` file and put the output in a new file, the `hello-world` step has been removed to improve the clarity of the example:

```yaml
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: download
spec:
  params:
    - name: url
      type: string
  steps:
    - name: get
      image: alpine:3
      script: |
        mkdir $(params.url)
        wget $(params.url) -P $(params.url)
    - image: alpine:3
      name: put-first-ten-lines-to-new-file
      script: |
        cd $(params.url)
        head -n 10 index.html > first-ten-lines.html
        cat first-ten-lines.html
```

> Aside: If `head` gets content from the start of a file, what command gets content from the end of a file?

If we `oc apply -f / tkn task start / tkn taskrun logs`, we see that this change works as expected.

The detail here is that each time a `Task` is created, it creates a `Pod` which has ephemeral storage. In the Kubernetes world this `Pod`-level ephemeral storage is `emptyDir`. We are able to use this technique because all `Steps` run on that same `Pod`, meaning that they have access to the `workspace/` folder that all `TaskRuns` do.

In fact, `Task` declares [two directories](https://tekton.dev/vault/pipelines-main/tasks/#reserved-directories) whose use is reserved, `/workspace` and `/tekton`. It is important to not forget the leading `/`.

**Note:** this approach will only work for `Task` and `TaskRun`:

> `emptyDir` volumes are not suitable for sharing data among `Tasks` within a `Pipeline`. However, they work well for single `TaskRuns` where the data stored in the `emptyDir` needs to be shared among the `Steps` of the `Task` and discarded after execution.

With that we now have a Tekton `Task` with multiple `Steps`.

# Kubernetes

## Standards

- Define Object in K8s
- Describe one use case for Pods
- Describe K8s including things like etcd and the control plane
- Use the command line to debug an application using `get`, `describe`, `logs`, etc.
- Describe the role of YAML in working with K8s

## Lesson

[Kubernetes](https://kubernetes.io/) is a platform of platforms. At its core, Kubernetes is "a portable, extensible, open-source platform for managing containerized workloads and services, that facilitates both declarative configuration and automation." ([source](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/)). _What does that mean?_

"Containerized workloads" _sounds like_ something potentially related to Docker. "Declarative configuration" _sounds like_ it means that we would interact primarily with text files where we declare the configuration we want. "Automation" _sounds like_ things will be done for us. We will see if this intuition is correct or not, given this difficult-to-understand definition.

Kubernetes is the result of an evolution in the way workloads are deployed ([source](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/)).

![The evolution of deployment](../img/container_evolution.svg)

This [section of the documentation](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/#why-you-need-kubernetes-and-what-can-it-do) gives a list of the benefits of Kubernetes that is worth reviewing. Kubernetes handles many traditional deployment concerns including load balancing, maintaining cluster-wide health, automatically self-healing to maintain the "desired state" of the cluster, and automated roll out/roll back, as examples.

There are many things Kubernetes is not. This approach of selective focus is powerful and enables the "plug and play" platform approach. Kubernetes is _not_ a fully-featured PaaS, but you can use Kubernetes to build a PaaS. OpenShift is one example of a PaaS that is built on top of Kubernetes. Google Cloud Platform is another example of a PaaS built on Kubernetes.

![Kubernetes cluster components](../img/components-of-kubernetes.svg)

The important components of the _Control Plane_ to note are:

- `kube-apiserver` - This API is the front end for the Kubernetes control plane.
- `etcd` - The cluster's storage mechanism for all cluster data. It is a highly available key/value store.
- `kube-schedular` - takes care of looking for newly created Pods that are not associated with a Node and selects the Node for the new Pods to run on.
- `kube-controller-manager` - Runs controller processes. For example:
  - Node controller: Responsible for noticing and responding when nodes go down.
  - Job controller: Watches for Job objects that represent one-off tasks, then creates Pods to run those tasks to completion.
- `cloud-controller-manager` - Responsible for cloud-provider specific behaviors. Links your cluster into your provider's cluster APIs and provides isolation between cloud platform components and components for your cluster.

Within the cluster there are Nodes that are used for processing workloads. All Nodes have the following three components running:

- `kubelet` - ensures containers are running on Pods. Takes a set of PodSpecs and makes sure that they are followed.
- `kube-proxy` - a network proxy that manages access into/out of the Pods in a Node.
- Container runtime - this will be Docker for us

Looking at these definitions, a theme emerges; declare the state of the cluster that we want then Kubernetes "makes it so."

We will use `kubectl` to introspect the state of our cluster while working, but otherwise direct interaction with Kubernetes will be minimal.

For completeness, the final topic to understand in Kubernetes is [Objects](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/). The documentation gives a great explanation:

> Kubernetes objects are persistent entities in the Kubernetes system. Kubernetes uses these entities to represent the state of your cluster. Specifically, they can describe:
>
> What containerized applications are running (and on which nodes)
> The resources available to those applications
> The policies around how those applications behave, such as restart policies, upgrades, and fault-tolerance"".

Since we will use OpenShift to deploy our code, the details of some Kubernetes Objects is abstracted from us.

As an example we can connect to a previous Bootcamp's cluster, in a namespace, and create a Kubernetes Deployment directly. First we ensure that we are in the correct context for running commands:

```shell
$:> kubectl config view
$:> kubectl config get-contexts
$:> kubectl config use-context <past-cohort-contextcontext>
$:> kubectl config current-context
$:> kubectl get pods
$:> kubectl get pod <pod-name> -o yaml
```

_Note:_ In this example, if `oc` is substituted for `kubectl` then all commands still work as intended.

For example, a Deployment, which is a type of Pod, would look like this example from the documentation:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2 # tells deployment to run 2 pods matching the template
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.14.2
          ports:
            - containerPort: 80
```

If we put this in a file named `nginx-deployment.yaml`, then we can apply this file to the cluster as a new desired state:

```shell
$:> kubectl apply -f nginx-deployment.yaml
```

looking again at `kubectl get pods` we see our newly created pods:

```shell
$:> kubectl get pods -o wide
NAME                                            READY   STATUS             RESTARTS   AGE     IP               NODE            NOMINATED NODE   READINESS GATES
nginx-deployment-66b6c48dd5-nwfsj               0/1     CrashLoopBackOff   4          2m22s   172.30.243.119   10.188.25.120   <none>           <none>
nginx-deployment-66b6c48dd5-r4jdx               0/1     CrashLoopBackOff   4          2m22s   172.30.0.235     10.188.25.121   <none>           <none>
```

If we want to see why the Pod's status is `CrashLoopBackOff` we can examine the logs using:

```shell
$:> kubectl logs <pod-name>
```

To further investigate this failure, we use `kubectl describe pod`:

```shell
$:> kubectl describe pod <pod-name>
```

For the purpose of this course, the "Understand" level of Bloom's Taxonomy is sufficient for our use of Kubernetes directly. The above commands will get us most of what we need.

**Historical Aside:** Kubernetes was [open sourced by Google](https://kubernetes.io/blog/2015/04/borg-predecessor-to-kubernetes/) in 2014.

## Glossary

| Term                     | Description                                                                                                                                                                                                                                                                      |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Cloud Controller Manager | Responsible for cloud-provider specific behaviors. Links your cluster into your provider's cluster APIs and provides isolation between cloud platform components and components for your cluster.                                                                                |
| Cluster                  | Control Plane, collection of Nodes                                                                                                                                                                                                                                               |
| Control Plane            | API Server, Scheduler, etcd, Controller Manager, Cloud Controller Manager                                                                                                                                                                                                        |
| Controller Manager       | Runs _controller_ processes.                                                                                                                                                                                                                                                     |
| etcd                     | The cluster's storage mechanism for all cluster data. It is a highly available key/value store.                                                                                                                                                                                  |
| Job controller           | A _controller_ that watches for Job objects that represent one-off tasks, then creates Pods to run those tasks to completion.                                                                                                                                                    |
| Kube API Server          | This API is the front end for the Kubernetes control plane.                                                                                                                                                                                                                      |
| Kube Proxy               | a network proxy that manages access into/out of the Pods in a Node.                                                                                                                                                                                                              |
| Kube Scheduler           | takes care of looking for newly created Pods that are not associated with a Node and selects the Node for the new Pods to run on.                                                                                                                                                |
| Kubelet                  | Node agent (registers node with API server).                                                                                                                                                                                                                                     |
| Kubernetes               | Open-source system for automating deployment, scaling, and management of containerized applications, based on a declarative, YAML-based syntax that takes as an input the desired state of the cluster and automatically works to make the cluster conform to the desired state. |
| Node                     | Virtual machine that hosts a group of Pods.                                                                                                                                                                                                                                      |
| Node controller          | A _controller_ watches for when node go down or new nodes become available.                                                                                                                                                                                                      |
| OpenShift                | PaaS built on top of Kubernetes and bridges Tekton, ArgoCD, and Kubernetes. OpenShift adds a useful layer of abstraction between its users and Kubernetes. This includes supporting a UI-driven approach to cluster management.                                                  |
| Pod                      | Group of one or more Docker Containers, with shared filesystem volumes and network resources.                                                                                                                                                                                    |

# Kubernetes (k8s) Assignment

## Standards

- Define Object in K8s
- Describe one use case for Pods
- Describe the role of YAML in working with K8s
- Describe K8s including things like `etcd` and the control plane

## Instructions

For this assignment, make a new folder named `<your-name>/k8s-assignment` in the `assignments` repository. Copy the following question and answer template into a file named `README.md` and answer all questions.

```markdown
# Kubernetes Questions

- Describe the role of YAML in working with K8s.

  `<your answer here>`

- Describe a Kubernetes Pod, you may use the documentation but use your own words to answer this.

  `<your answer here>`

- Describe K8s including things like `etcd` and the control plane.

  `<your answer here>`

- What is your experience with deploying software?

  `<your answer here>`

- Have you used Kubernetes before?

  `<your answer here>`

- If so, how? Have you deployed an application to a data center?

  `<your answer here>`
```

When you have completed this, create a commit with `README.md`, push it to the `assignments` repository, and create a new Pull Request.

## Submit the Assignment

**_IMPORTANT:_** Please make sure **all** members of the Pair submit the assignment.

1. `cd` to the `assignments` repository.
2. Follow [the steps](../git/git-workflow-step-by-step.md) outlined in the git workflow to create a new branch.
3. Make a commit that includes **_only_** the changes for this assignment. (_hint:_ What do you need to do first? Why might your changes be rejected?)
4. Push the changes to the remote branch.
5. Create a new Pull Request on GitHub.com

## Stretch Material

Deploy an application to the cohort's cluster.

Login to the cluster with `icc <cluster-name>` and create a new project with `oc new-project <your-name>-<app-name>`.

(If not done yet) push an existing docker image to [quay.io](https://quay.io). Make sure the image is tagged properly (don't use `latest`).

Then create a yaml file that specifies a `Deployment` which references the docker image on quay.io. Also specify a `Service`, and a `Route` to make the app accessible from the internet.

Use commands like `kubectl create|apply|delete -f <filename>` to have Kubernetes create the resources according to your specifications.

Finally, verify that your app is available and responding by opening it in the browser. You can obtain the URL with `kubectl get routes`.

In case of any errors, start debugging the problem and use any resource available to you to understand and fix the problem.

Here are a few helpful commands to debug k8s resources:

```bash
kubectl get all
kubectl get pods
kubectl logs <pod-name>
kubectl describe pod <pod-name>
kubectl diff -f <filename>
```

# Continuous Delivery/Deployment

## Standards

- Describe the difference between Continuous Delivery and Continuous Deployment
- Differentiate between Push-based and Pull-based GitOps models

## Lesson

First off, what is "Software Delivery"? This term is used widely and will be referenced heavily throughout this lesson, so it is important to define it. We will use this definition for our purposes:

> Software delivery is the act of a developer, or team or developers, moving a line of code from idea to production.

That encompasses **so many things**. What are some elements of Software Delivery?

- Product Ownership and story management
- Team development skill
- Organizational goal alignment and clarity
- Deployment procedures and processes
- Operations procedures and processes
- Security procedures and processes

All this is to say, it is often not simple to take a line of code from a developer's desk to production, quickly, for many reasons.

In our pipeline, so far, we have focused on the creation of an artifact for deployment; an image. The current final step of our Pipeline is a test deployment of our `manifests.yaml`. Continuous Delivery describes the process between creating the `manifests.yaml` and applying it to Production.

This course teaches CI/CD with an end state of Continuous Integration / Continuous Deployment, with CD implemented as Pull-based GitOps. It is valuable to understand the evolution from Continuous Delivery to Continuous Deployment as you may experience this evolution on a client site.

Early in the movement to deployment early and often with automated testing and deployment (CI/CD), business leaders, product owners, and (virtually all other) non-software rolls were hesitant. And, for good reason. _Why?_

Dominant movements prior to CI/CD for delivery included Waterfall and Scrum, both of which introduce delivery pitfalls:

- **Waterfall** - Delivery timeline is so expansive that every release has a _high degree of risk_ with _human testing as the measure of quality_.
- **Scrum** - Delivery timelines are vastly improved from Waterfall, but Scrum is a delivery focused, rather than technology focused, methodology. This means that _quality varies greatly due to the practices a given team chooses_.

In both methodologies, quality is not at the forefront of development efforts.

Since quality is too variable, when presented with "we are going to go to production multiple times a day!" the response from everyone else was dread.

With manual QA out of the question, but a high concern around quality, what would a _reasonable_ business do? Implement some type of small quality check.

In practice, this small quality check was the Product Manager (aka Product Owner) in a Staging environment, before giving the developers approval to "click the deploy button."

As we did in a previous lesson, let's challenge the Product Manager, can they do this approval process for:

- 10 tiny features per day without quitting?
- 15 tiny features per day without quitting?
- 20 tiny features per day without quitting?
- 50 tiny features per day without quitting?
- 100 tiny features per day without quitting?

What arose naturally is that, for the same reason Continuous Integration requires _no manual QA process_, Continuous Deployment requires _no manual approval process_.

"Click the deploy button" indicates a team is practicing _Continuous Delivery_. In the evolution of this movement, Product Managers learned over time that developers practicing TDD, DevOps, CI/CD, and the other practices of Extreme Programming (and in turn the Garage Method) produce code that is less likely to cause issues. Moreover, the code produced by XP developers is easier to roll forwards or backwards from because the changes are much smaller. Eventually this led to the removal of the "deploy button" and the emergence of _Continuous Deployment_.

The final step of our Continuous Integration (CI) pipeline will introduce a GitOps repository.

From the [documentation](https://www.gitops.tech/#how-does-gitops-work):

> GitOps organizes the deployment process around code repositories as the central element. There are at least two repositories: the application repository and the environment configuration repository. The application repository contains the source code of the application and the deployment manifests to deploy the application. The environment configuration repository contains all deployment manifests of the currently desired infrastructure of a deployment environment. It describes what applications and infrastructural services (message broker, service mesh, monitoring tool, …) should run with what configuration and version in the deployment environment.

There are two important points:

1. the codebase and deployment manifests live in the application's repository
1. a second repository contains only the desired state of infrastructure for a given deployment environment

This second point sounds suspicious, it deals with the _desired state_ of a system. That sounds _a lot like something Kubernetes would be great at_.

This diagram from the documentation does a good job of showing the overall concept behind the Push-based model of GitOps:

![GitOps push model](../img/gitops-push-model.png)

Everything flows as a series of triggers, in a single direction. The end result is a deployment. For our course, we will demonstrate this flow using only Tekton to demonstrate the difference between Tekton's approach to CI/CD and using a mixture of Tekton and ArgoCD.

The other model is Pull-based:

![GitOps pull model](../img/gitops-pull-model.png)

In this case our `Operator` will be [ArgoCD](https://argoproj.github.io/argo-cd/) and deployment environment will be OpenShift/Kubernetes.

Finally, the "core idea" statement at the beginning of the documentation sums it up best:

> The core idea of GitOps is having a Git repository that always contains declarative descriptions of the infrastructure currently desired in the production environment and an automated process to make the production environment match the described state in the repository.

We want a single spot to change that updates all of our declarative infrastructure.

This is a departure from traditional models of deployment that typically did not use declarative infrastructure, were exclusively push-based or time-based, and relied heavily on manual processes. This approach also gives an option for isolation of the production environment from developers (for example if required for compliance).

_Why should a customer care?_ Aside from obvious reasons like the repeatability of automation and ability to "fix a problem once":

- having a commit log of the production environment is a mechanism for performing fast rollbacks and audits
- developers already know Git and are comfortable in the tooling
- centralizes environment configuration while still allowing for parameterization
- ability to segregate developer access ("left of Image Registry and Environment Repository") versus production access ("only access Image Registry and Environment Repository")

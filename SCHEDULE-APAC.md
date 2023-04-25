# CI/CD from First Principles Schedule

## Overview

**Objective** Build a CI/CD pipeline in the IBM Cloud from first principles.

**Description** This course is intended for Software Development and Operations professionals with prior
experience who would like to deploy applications quickly, using the Cloud and DevOps.

**Pre-requisite Skills** To be successful in this course, students should have:

- basic familiarity with a Command Line Interface (CLI)
- previous use of a Text Editor or Integrated Development Environment (IDE)
- familiarity with the Software Development Lifecycle (any methodology)
- a desire to learn rapidly and try new things

**Software Requirements**

Follow the [computer setup](https://github.com/upslopeio/ibm-cloud-garage-training/tree/main/computer-setup/apac-emea/README.md) instructions.

## Schedule

### Day 1 - Git, CLI

#### (70 minutes)

- [Introductions](./student-introductions/README.md) (30 minutes)
- [Course Introduction](./course-introduction/README.md) (10 minutes)
- [Git](./git/README.md) (30 minutes)

#### Break (15 minutes)

#### (70 minutes)

- [Git Assignment](./git/assignment.md) (20 minutes)
- [CLI](./cli/README.md) (50 minutes)

#### Break (15 minutes)

#### (70 minutes)

- [CLI Assignment](./cli/assignment.md) (40 minutes)
- [Computer setup](.https://github.com/upslopeio/ibm-cloud-garage-training/tree/main/computer-setup/apac-emea/README.md) (30 minutes)

#### Day 1 Self Study (4 hours)

- [Self Study – CLI](cli/self-study.md)

### Day 2 - Docker, Cloud Native, Kubernetes

#### (70 minutes)

- [Where are we?](./progress/after-day-1.md)
- [Pair Programming](./pairing/README.md) (10 minutes)
- [Docker](./docker/README.md) (60 minutes)

#### Break (15 minutes)

#### (70 minutes)

- [Docker Assignment](./docker/assignment.md)

#### Break (15 minutes)

#### (70 minutes)

- [Cloud](./cloud/README.md) (30 minutes)
- [Kubernetes](./kubernetes/README.md) (30 minutes)
- [Kubernetes Assignment](./kubernetes/assignment.md) (10 minutes)

#### Day 2 Self Study (4 hours)

- [Self Study – Docker](docker/self-study.md)

### Day 3 - Tekton `Task`, `TaskRun`, `Pipeline`, `PipelineRun`

#### (70 minutes)

- [Where are we?](./progress/after-day-2.md)
- [Tekton Task Demo: Hello, World!](./tekton/task/README.md) (30 minutes)
- [Tekton Task Assignment: file operations](./tekton/task/assignment.md) (40 minutes)

#### Break (15 minutes)

#### (70 minutes)

- [Tekton Task Assignment: file operations](./tekton/task/assignment.md) (30 minutes)
- [Tekton Catalog](./tekton/catalog/README.md) (20 minutes)
- [Tekton Catalog Assignment](./tekton/catalog/assignment.md) (20 minutes)

#### Break (15 minutes)

#### (70 minutes)

- [Tekton Catalog Assignment](./tekton/catalog/assignment.md) (70 minutes)

#### Day 3 Self Study (4 hours)

- [Self Study – Tekton Tasks](tekton/catalog/self-study.md)

### Day 4 - CI Pipelines, `Pipeline`, `PipelineRun`, Cloning a repository

#### (70 minutes)

- [Where are we?](./progress/after-day-3.md)
- [CI Pipelines](./ci-cd/README.md) (45 minutes)
- [Pipeline Build: git-clone](./tekton/01-git-clone/README.md) (45 minutes)

#### Break (15 minutes)

#### (70 minutes)

- [Pipeline Build: git-clone assignment](./tekton/01-git-clone/assignment.md) (70 minutes)

#### Break (15 minutes)

#### (70 minutes)

- [Pipeline Build: `npm test`](./tekton/02-npm-test/README.md) (15 minutes)
- [Pipeline Build: buildah](./tekton/03-buildah/README.md) (55 minutes)

#### Day 4 Self Study (4 hours)

- [Self Study – Tekton Pipelines](tekton/01-git-clone/self-study.md)

### Day 5 - Pipeline Build: buildah, Kustomize, Triggers

#### (70 minutes)

- [Where are we?](./progress/after-day-4.md)
- [Pipeline Build: buildah assignment](./tekton/03-buildah/assignment.md) (70 minutes)

#### Break (15 minutes)

#### (70 minutes)

- [Pipeline Build: kustomize](./tekton/04-kustomize/README.md) (70 minutes)

#### Break (15 minutes)

#### (70 minutes)

- [Build Triggers](./tekton/05-triggers/README.md) (30 minutes)
- [Pipeline Build: kustomize, Triggers assignment](tekton/05-triggers/assignment.md) (40 minutes)

#### Day 5 Self Study (4 hours)

- [Self Study – Kustomize, Triggers](tekton/05-triggers/self-study.md)

### Day 6 - Push-Based GitOps, Pull-Based GitOps with ArgoCD, Continuous Integration / Continuous Deployment End to End

#### (70 minutes)

- [Introduction to Continuous Deployment/Delivery](continuous-deployment/README.md) (20 minutes)
- [Pipeline Build: GitOps](tekton/06-gitops/README.md) (20 minutes)
- [Introduction to GitOps](./git-ops/README.md) (30 minutes)

#### Break (15 minutes)

#### (70 minutes)

- [Pipeline Build: Pull-Based GitOps with ArgoCD](./argocd/README.md) (15 minutes)
- [CI Pipeline Build: End to End Assignment](./tekton/end-to-end/assignment.md) (55 minutes)

#### Break (15 minutes)

#### (70 minutes)

- [CI Pipeline Build: End to End Assignment](./tekton/end-to-end/assignment.md) (70 minutes)
- [Where are we?](./progress/pipeline-complete.md)

#### Day 6 Self Study (4 hours)

- [Self Study – Pipeline Advanced](tekton/06-gitops/self-study.md)

### Day 7 - Project Day 1

## Content

#### (70 minutes)

- [Team Project Kick-off](./project/README.md) (10 minutes)
- Work on Team Project

#### Break (15 minutes)

#### (remainder)

- Work on Team Project
- Last 5 minutes: Stand Down/Transition to Self Study

#### Day 7 Self Study (4 hours)

- [Self Study – Another Pipeline](tekton/end-to-end/self-study.md)

### Day 8 - Project Day 2

## Schedule

- Work on Team Project
- One (1) hour before the end of the course, we will have a presentation from each team and, if time allows, a brief retrospective. Your goal is to present in ten (10) minutes or less:

  - A brief overview of what CI/CD is, as if you were presenting to a client who does not have a technical background
  - A CI/CD Pipeline that uses Pull-based GitOps
  - Your application running in the cloud as a demo (e.g. show a "Hello, world!" screen)
  - List of the challenges that your Squad faced in delivering this application to the Cloud.

  You may use any presentation medium of your choosing.

#### Day 8 Self Study (4 hours)

- [Self Study – Application Continuum](./tekton/application-continuum/self-study.md)

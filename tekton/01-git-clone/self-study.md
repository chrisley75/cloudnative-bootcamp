# Self Study – Tekton Pipelines

### Cloud Engineer (Developer AND Operations)

Finish any part of today's assignments that you were unable to finish during class. Make sure to commit any new work you have completed to the `assignments` repository.

### Cloud Engineer (Developer)

1. Generate a new application in the stack of your choice. Create a new GitHub repository and push your application to GitHub. You will use this application over the coming days of Self Study. Containerize your application with Docker. Next, create a new pipeline in the namespace created today (e.g. for me this was `oc new-project andreas-kavountzis-pipeline-from-scratch` you will replace my name with your name). Have your pipeline use the `git-clone` Tekton Catalog Task as was used in class today.
1. Once you have a `Pipeline` and way to create a `PipelineRun`, then go back to your application and begin to implement features using [Test Driven Development](https://en.wikipedia.org/wiki/Test-driven_development).

### Cloud Engineer (Operations)

1. Find an open source application in the stack of your choice. Clone the code locally and then create a new Git/GitHub repository and push your copy of the application to GitHub. Alternatively, use a public GitHub Fork. You will use this application over the coming days of Self Study. Containerize your application with Docker if it is not already. Next, create a new pipeline in the namespace created today (e.g. for me this was `oc new-project andreas-kavountzis-pipeline-from-scratch` you will replace my name with your name). Have your pipeline use the `git-clone` Tekton Catalog Task as was used in class today.
1. Once you have a `Pipeline` and way to create a `PipelineRun`, find your Pipeline and its builds in the OpenShift Console UI. Inspect the YAML for the `Pipeline` and a `PipelineRun` - what is different between the cluster's YAML and the YAML you gave `oc apply -f`? Make a list of the differences then research the lifecycle of how/when these changes occur and what part of OpenShift (or Kubernetes) is responsible for it.

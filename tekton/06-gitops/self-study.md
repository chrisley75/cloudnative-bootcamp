# Self Study – Pipeline Advanced

## Cloud Engineer (Developer AND Operations)

Continuing with your application from yesterday, the goal today is to finish the pipeline implementation by having the `PipelineRun` be trigger by a `git commit` to your GitHub repository. Since you are focused on the deployment rather than the development, updates to the `README.md` are likely the easiest way to test the build trigger.

## Cloud Engineer (Developer)

[odo](https://docs.openshift.com/container-platform/4.7/cli_reference/developer_cli_odo/odo-release-notes.html) is a CLI tool aimed at providing "Developer level" abstractions to OpenShift. Fork and clone the sample Express application used in this course or generate a new one. Next, [install `odo`](https://docs.openshift.com/container-platform/4.7/cli_reference/developer_cli_odo/installing-odo.html). Finally, [use `odo` to deploy the Express application](https://docs.openshift.com/container-platform/4.7/cli_reference/developer_cli_odo/creating_and_deploying_applications_with_odo/creating-a-single-component-application-with-odo.html).

## Cloud Engineer (Operations)

Operators are out of scope for this course but may be relevant for your role. Read about the [Operator Framework](https://www.redhat.com/en/blog/introducing-operator-framework-building-apps-kubernetes) and [What are Operators?](https://docs.openshift.com/container-platform/4.7/operators/understanding/olm-what-operators-are.html). Next install [`opm`](https://docs.openshift.com/container-platform/4.7/cli_reference/opm-cli.html). Is it possible to host our own Catalog? If so, why would we care? Moreover, why would we use this approach?

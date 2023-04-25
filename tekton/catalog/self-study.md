# Self Study – Tekton Tasks

## Cloud Engineer (Developer AND Operations)

Finish any part of today's assignments that you were unable to finish during class. Make sure to commit any new work you have completed to the `assignments` repository.

## Cloud Engineer (Developer AND Operations)

1. Make sure you are in your own namespace and **not** `default`.
1. Use the [`curl` Task](https://github.com/tektoncd/catalog/tree/main/task/curl/0.1) to get a response from an API. You can find the API via Google and choose something that interests you.
1. Choose any task from the Tekton Catalog that may be run independently. Next, create a `Task` and `TaskRun` using that task - try to get creative with what your task does.
1. Next, read about the [Scorecards](https://github.com/tektoncd/catalog/tree/main/task/scorecard/0.1) task. Use this Task to run a Scorecard either for your own repository or an open source one.
1. (Stretch) Find one part of the Tekton documentation that is missing key details, like installation instructions in the Catalog. Next, determine how to contribute as a developer to Tekton. Fix the documentation and submit your solution to the maintainers.
1. (Stretch) What if you could have multiple tasks occur in order? What about at the same time? Ahead of tomorrow's class, try to use the documentation and what you have learned so far to build a `Pipeline` that uses multiple `Task`s.

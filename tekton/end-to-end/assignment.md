# Tekton End-to-End Assignment

## Standards

- Create a `Pipeline` that clones a Git repository, runs the tests, builds the production version, creates a Docker image, pushes the image to quay.io and deploys the application, that is triggered by a Git push to the repository

## Instructions

1. Use [Next.js](https://nextjs.org/) to generate an application (`npx create-next-app`) in your workspace folder. [Next.js](https://nextjs.org/) uses Static Rendering to deliver performant, minimalist, server-rendered pages using React. You can [read more here](https://nextjs.org/docs/basic-features/pages).
   - Make sure you are able to run the application locally.
   - Then commit the app to a new GitHub repository.
2. Make a new folder named `<your-name>/tekton-e2e-assignment` in the `assignments` repository. Include a `README.md` in the `tekton-e2e-assignment` folder with the names of everyone that contributed. Copy the URL to your Next.js repository into the `README.md` file and commit it.
3. Your goal is to create a Tekton `Pipeline` that does the following:
   - trigger the pipeline by a Git push to the repository.
   - clones the Git repository
   - runs the tests
   - builds the production version (if applicable)
   - creates a Docker image
   - pushes the image to quay.io
   - generate the k8s configuration to deploy the application
   - deploys the application using the generated configuration (not to production)
   - commits the k8s configuration to a GitOps repository
4. Create a Pull Request for the branch created in step 2.

## Stretch

Work on the following problems in any order.

### Lean Docker Image

Considering that you are dockerizing a Next.js app, does your docker image contain unnecessary files? Can you make it smaller?

### DRY Pipeline

Looking at the pipelines you created over the last few days, is there any duplication of yaml code that can be eliminated? How about using something like `oc apply -k ...` to eliminate duplication?

### Divide & Conquer

Deploy an API and use it in your Next.js application. Can you connect to the API without hard-coding the endpoint?

### Long-term Memory

Talk to a database like PostgreSQL. Connect your API or your Next.js application to a database you have deployed separately into your OpenShift cluster.

Can you connect to different database instances without changing any code in the app repository?

### Bad Apples

If the build fails, can you find a way to indicate that the image produced should not be used? Or can you remove it from the registry?

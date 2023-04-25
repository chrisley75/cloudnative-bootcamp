# Docker Assignment

## Standards to Achieve

By completing this assignment, you demonstrate the following Standards:

- Define Docker image and Docker container
- Run a container in the foreground, mapping ports
- List images and containers
- Show running containers
- Pull and push images
- Create a Dockerfile containing `nginx` and build it

## Instructions

For this assignment you will containerize a React application and serve it using `nginx`.

> **NOTE** For Windows users, run all commands from within Ubuntu (WSL)

## Create a React app

### Set up a new React app

Create a workspace in your home folder (`~`) of your local machine (or any folder **other** than the one where you have cloned the `assignments` repository):

```
mkdir ~/workspace
cd ~/workspace
```

Now, create a new React app:

```
npx create-react-app react-intro
cd react-intro
```

_Notice:_ The app is **not** created under your `assignments` folder.

If you do not have [`npx`](https://www.npmjs.com/package/npx) installed, you can install it with `npm install -g npx`.

### Start the React app

```
npm start
```

This should open `http://localhost:3000` and you should see a React welcome page.

### Run Tests

```
npm test
```

This starts an interactive test window. Tests should be green.

Press `q` or `CTRL+C` to exit.

### Commit your changes

Within the application folder `react-intro` initialize a new git repository and commit your changes:

```
git init
git add .
git commit -m "create react app"
```

### Create a Remote GitHub Repository and Push

Go to [GitHub](https://github.com), make sure you have authenticated, then create an empty repository with a unique name in the [cloud-native-garage-method-cohort](https://github.com/cloud-native-garage-method-cohort) organization. A good convention is to begin with your cohort, then name, then assignment, for example: `xyz-1-john-doe-react`.

Copy the commands for pushing an existing repository and execute them where you created the React application in the previous steps.

```
git remote add origin git@github.com:cloud-native-garage-method-cohort/<your-repository-name>.git
git branch -M main
git push -u origin main
```

## Dockerize the React App

Since React is a client-side JavaScript framework, we need something to act as a webserver. For our purposes we select [Nginx](https://www.nginx.com/).

`nginx.conf` (same as above):

```
server {
    listen       8080;
    server_name  localhost;
    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
        try_files $uri $uri/ /index.html =404;
    }
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}
```

`Dockerfile`

```
FROM quay.io/upslopeio/node-alpine as build
WORKDIR /app
COPY . .
RUN npm install
RUN npm run build

FROM quay.io/upslopeio/nginx-unprivileged
COPY --from=build /app/build /usr/share/nginx/html
COPY --from=build /app/nginx.conf /etc/nginx/conf.d/default.conf
```

Then from the command line, to build you would execute the following commands:

```
# no need to run npm build
docker build -t quay.io/<user-name>/react-intro .
docker run -it -p 8080:8080 --rm quay.io/<user-name>/react-intro
```

Then open `http://localhost:8080` in your browser to see it work.

When you have completed this, make a commit to your React app repository.

### Multistage build

The above `Dockerfile` is a [multistage build](https://docs.docker.com/develop/develop-images/multistage-build/) file. In short, this means there is more than one `FROM` statement in the `Dockerfile`

**Pros**: You don't need to build the React application locally before building the docker image.

**Cons**: The Dockerfile is more complex, and may re-download npm packages and re-run the build which might not be necessary (depending on your build system).

## Submit the Assignment

**_IMPORTANT:_** Please make sure **all** members of the Pair submit the assignment.

1. `cd` to the `assignments` repository.
2. Follow [the steps](../git/git-workflow-step-by-step.md) outlined in the git workflow to create a new branch.
3. Create a new file named `<your-name>/docker-assignment/README.md`.
4. Add the names of **all** members of the Pair to the `README.md` file and save it.
5. Create a new file named `<your-name>/docker-assignment/repository-url.txt`.
6. Copy the URL to your React repository into the text file and save it.
7. Make a commit that includes **_only_** the changes for this assignment. (_hint:_ What do you need to do first? Why might your changes be rejected?)
8. Push your changes to the remote branch.
9. Create a new Pull Request on GitHub.com

**_IMPORTANT:_** Do not merge Pull Requests yourself.

### Common Pull Request Issues

| Issue                                                                           | Solution                                                                                                                          |
| ------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| Contains files outside of `<your-name>/docker-assignment/` folder               | `mv` the files to the correct location under your folder.                                                                         |
|                                                                                 | You have changes that are not related to this assignment.                                                                         |
|                                                                                 | Merge in changes from main and push the branch again. The changes you see in the PR at this point do not exist in main.           |
|                                                                                 | The solution to remove a change depends on the change. Is it a deleted file? Restore the file. Is it a new file? Delete the file. |
| Contains any file other than `<your-name>/docker-assignment/repository-url.txt` | Delete these files and also add them to `<your-name>/git-assignment/.gitignore`                                                   |

## Stretch Material

1. Push your react docker image to your [quay.io](https://quay.io) account. Create a dedicated image repository on quay.io for that and tag your image properly (don't use `latest`).
1. If you have finished this assignment early, start on this series of [Docker Katas](https://github.com/eficode-academy/docker-katas/tree/master/labs) or try the [In Browser Scenarios with Docker on Katacoda](https://www.katacoda.com/courses/docker).
1. (Build and Burn) Attempt to recreate what you just did without referring to what you just did. _Hint:_ the first command is `npx create-react-app <my-app-name>`.
1. Find an open source application in a stack of your choosing. Attempt to write a simple Dockerfile for that stack that works for development environment, locally. If you complete this, please commit your `Dockerfile` to the `assignments` repository in the `docker-assignment` folder.

---

🛑 Everything in the next section is **optional**, you may skip this if you are not interested.

---

### Background

Public DockerHub images are [severely rate-limited](https://www.docker.com/increase-rate-limits).

Quay.io does not have rate limits on public repositories. See the [Docker Lab](https://cloudnative101.dev/lectures/containers/activities/) for more information on how to create a Quay.io account.

On a client site, you will have an internal Docker registry. In fact, even in class there's an [internal docker registry](https://docs.openshift.com/container-platform/3.3/install_config/registry/accessing_registry.html) on OpenShift Container Platform which you can use.

You can see access information about the OpenShift image repository by running `igc credentials`.

For this tutorial we're referencing images pushed to a personal Quay.io account.

❌️❌ WARNING: these images are not maintained up-to-date and may contain un-patched security vulnerabilities. DO NOT USE on a production application or client site. ❌❌

If you want a more recent image, do the following:

```
export QUAY_USER=<your quay.io username>

docker pull node:alpine
docker tag node:alpine quay.io/$QUAY_USER/node-alpine
docker push quay.io/$QUAY_USER/node-alpine

docker pull nginxinc/nginx-unprivileged
docker tag nginxinc/nginx-unprivileged quay.io/$QUAY_USER/nginx-unprivileged
docker push quay.io/$QUAY_USER/nginx-unprivileged
```

### Build then Build 😉

React applications (as well as other single-page applications) compile down to static files (HTML, CSS, fonts, etc...).

In order to build these applications, you need to add two files:

1. Dockerfile
1. nginx.conf

`nginx.conf`:

```
server {
    listen       8080;
    server_name  localhost;
    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
        try_files $uri $uri/ /index.html =404;
    }
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}
```

`Dockerfile`

```
FROM quay.io/upslopeio/nginx-unprivileged
COPY build /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
```

Then from the command line, to build you would execute the following commands:

```
npm run build
docker build -t dockerized-react-app .
docker run -it -p 8080:8080 --rm dockerized-react-app
```

Then open `http://localhost:8080` in your browser to see it work.

**Pros** Your Dockerfile is super simple.

**Cons** You need to build the application before building the Dockerfile.

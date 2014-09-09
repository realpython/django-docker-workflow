# Testing with Docker

Let's look at a nice workflow for testing with Docker.

Tools we'll be using:

1. Python/Django
1. Docker
1. Fig
1. Github
1. Jenkins
1. Docker Hub

## Docker Explained

"[Docker](https://www.docker.com/whatisdocker/) is an open-source engine that automates the deployment of any application as a lightweight, portable, self-sufficient container that will run virtually anywhere."

Docker provides the isolation of a virtual machine, wrapping the filesystem, processes, evironment variables, etc. into a nice container - but without all the overhead since it uses the host machine's kernal. Your app, processes, and code run in *containers*, while *images* save the state of the containers for easy recreation on other machines. Using the popular .git analagy, images are akin to repositores while containers are akin to a local clone.

**Why use Docker?**

1. Containers are isolated like a true VM. Forget about messing with virtualenv.
1. Docker images are shareable and handle version control at the system-level. Easily distrubute your working environment amongst your enture team.

Finally, you can *truly* mimic your production environment at the local level, without any perfromance loss.

**You can add Docker into your development workflow to test your deployments locally in an isolated, lightweight VM-like environment, all without touching virtualenv.** Pretty cool.

## Fig Explained

Fig automates the (re)creation of a Docker environment.

## Quick Start

### Setup Docker for Mac

1. Follow the instructions [here](http://www.fig.sh/install.html) to install Docker and Fig.
1. Sanity check

    ```
    $ docker-osx shell
    $ fig --version
    fig 0.5.2
    ```

1. docker-osx commands: run `docker-osx` to view the available commands

### Create a Dockerfile

A Dockerfile is a configuration file that automates the creation of your container from an image.

```
# start with a base image
FROM ubuntu:13.10
MAINTAINER Real Python <info@realpython.com>

# install dependencies
RUN apt-get -qq update
RUN apt-get install -y python python-pip

# grab contents of source directory
ADD ./src /src/

# specify working directory
WORKDIR /src

# build app
RUN pip install -r requirements.txt
RUN python manage.py syncdb --noinput

# expose port 8000 for us to use
EXPOSE 8000

CMD python manage.py runserver 0.0.0.0:8000
```

### Django Project

Clone the repo to get started quickly. Alternatively, use your own Django Project. Just be sure to re-organize the project to match mine and add the *fig.yml* file. If you don't have any tests, be sure to add a few.

PUSH this up to Github.

> Want to test it locally? Simply run `fig up`, then navigate to [http://localdocker:8000/](http://localdocker:8000/). You should see "Hello, World!"

### Docker Hub Setup

1. Signup [here](https://hub.docker.com/account/signup/), if you don't already have an account.
1. Create an *Automated Build* from the code your just PUSHed to Github. Make sure to uncheck the *When active we will build when new pushes occur* box.
1. Now we need to create a trigger so that Docker Hub know when to create the builds. Navigate to the newly created repository and then click *Build Triggers*. Turn the trigger status to on and copy the Trigger URL.

https://registry.hub.docker.com/u/mjhea0/django-docker-workflow/trigger/8b212b14-3797-11e4-a515-464291d9ac34/

### Jenkins Setup

1. Navigate to the jenkins directory and then create a new image: `docker build -t jenkins-docker`.
1. Now run the image `docker run -p 8080:8080 --privileged jenkins-docker`. By using the `--privileged` flag, Docker actually runs itself within the container. Pretty cool.
1. Launch Jenkins. Install the Github Plugin.


## Deployment Workflow

1. Code locally
1. PUSH to Github
1. Jenkins runs tests
1. If tests fail, process ends
1. If tests pass:
    1. Docker Hub builds new image
    1. Jenkis Deploys to Heroku

## Code Locally

In the normal flow, you would ideally be a point where you're ready to deploy code to production. Perhaps you fixed a major bug or implemented a new feature. Let's start with a pre-build Project. Clone the repo. Alternatively, use your own Django Project. Just be sure to re-organize the project to match mine and add the *fig.yml* file. If you don't have any tests, be sure to add a few.

Commit your changes.

## PUSH to Github

PUSH your changes to Github.


## Resources

https://github.com/wsargent/docker-cheat-sheet
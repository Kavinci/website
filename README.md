# website

How to Add Docker to a .Net Core application that uses Angular or React on the front end
This is a cross platform scenario, developing on Windows and deploying to a Linux Docker container.

### Docker Support
`Right click on the solution -> add -> Docker Support`
This adds a Dockerfile to the project to get the .Net Core portion running
Node is unfortunately not listed in the docker file and needs to be added.
Below the first FROM statement i.e. `FROM microsoft/dotnet:2.1-aspnetcore-runtime AS base`, your .Net Core version may vary,
Copy and Paste:

If you are using Angular
`RUN apt-get update && \
    apt-get install -y wget && \
    apt-get install -y gnupg2 && \
    wget -qO- https://deb.nodesource.com/setup_11.x | bash - && \
    apt-get install -y build-essential nodejs && \
	npm install -g -y typescript && \
	npm install -g -y @angular/cli`

or If using React or React Redux
`RUN apt-get update && \
    apt-get install -y wget && \
    apt-get install -y gnupg2 && \
    wget -qO- https://deb.nodesource.com/setup_11.x | bash - && \
    apt-get install -y build-essential nodejs`

This bit of code will update your Linux environment and install Node.js globally along with npm which is needed for these front-end frameworks.
The stock npm in the initial project is installed locally and causes a server failure when you try and launch the application in a Docker container.

### Docker Compose
`Right click on the solution -> Add -> Container Orchestrator Support, then select Docker Compose`
Add docker compose so we synchronize our host code with what is in the container, launching containers takes a good bit of time.

If you run into issues, especially the 'The DOCKER_REGISTRY variable is not set. Defaulting to a blank string.' error after a failed build with Docker Compose,
then make sure you are running Visual Studio as an Administrator and you are logged in to Docker. If that doesn't work try restarting Docker for Windows.

The error has something to do with the way Visual Studio launches Docker Compose. I do not want to go in depth on this error but more info can be gained here [Github Microsoft DockerTools issue 130](https://github.com/Microsoft/DockerTools/issues/130#issuecomment-402936232)

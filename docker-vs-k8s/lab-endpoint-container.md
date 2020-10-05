# Lab: End-Point in a Container

1. Open a web browser to https://console.cloud.google.com/
1. We will use [Cloud Shell](https://cloud.google.com/shell) for in-browser development environment
1. And later we will deploy the Docker container to a Kubernetes cluster running on GCP
1. Open Cloud Shell in a new browser tab/window and open the editor as well
1. Open a terminal and clone the repo with end-point source code: `git clone https://github.com/vkhazin/courseware-nodejs-container.git`
1. Review the *.js files under api folder to get an idea of the source code
1. Docker daemon is pre-installed, you can check the version: `sudo docker --version`
1. To build a new docker image execute: `sudo docker build ./ -t node/end-point` from the same folder where Dockerfile is located
1. Expected result: `...Successfully tagged node/end-point:latest`
1. To run the newly created image we need to create a container from it
1. Images are immutable, containers can mutate over their lifecycle
1. To launch a new container execute: `sudo docker run --name node-end-point -d --rm -p 3001:3001 node/end-point`
1. --name gives our container a name
1. -p 3001:3001 maps host port 3001 to container port 3001
1. -d starts detached to be able to continue using the terminal
1. --rm deletes the container when it is stopped
1. And now you are able to access the endpoint: curl http://localhost:3001/?name=John
1. This is how you create a node.js end-point running in a container
1. To delete the container: `sudo docker rm node-end-point -f`
1. To delete the image: `sudo docker rmi node/end-point`
1. There are a couple of yml files in the repository we will discuss in the next labs
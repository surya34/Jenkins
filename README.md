# Jenkins

####### setup and run jenkins using docker /podamn containers 

**Create the following volumes to persist the Jenkins data using the following docker volume create commands**

 podman volume create jenkins-data
 
**Download the jenkinsci/blueocean image and run it as a container in podman using the following podman container runcommand**


podman container run \
  --name jenkins-blueocean \
  --rm \
  --detach \
  --privileged \
  --publish 8080:8080 \
  --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  jenkinsci/blueocean

( Optional ) Specifies the Docker container name for this instance of the jenkinsci/blueocean Docker image. This makes it simpler to reference by subsequent docker container commands.

( Optional ) Automatically removes the Docker container (which is the instantiation of the jenkinsci/blueocean image below) when it is shut down. This keeps things tidy if you need to quit Jenkins.

( Optional ) Runs the jenkinsci/blueocean container in the background (i.e. "detached" mode) and outputs the container ID. If you do not specify this option, then the running Docker log for this container is output in the terminal window.

Connects this container to the jenkins network defined in the earlier step. This makes the Docker daemon from the previous step available to this Jenkins container through the hostname docker.

Specifies the environment variables used by docker, docker-compose, and other Docker tools to connect to the Docker daemon from the previous step.

Maps (i.e. "publishes") port 8080 of the jenkinsci/blueocean container to port 8080 on the host machine. The first number represents the port on the host while the last represents the container’s port. Therefore, if you specified -p 49000:8080 for this option, you would be accessing Jenkins on your host machine through port 49000.

( Optional ) Maps port 50000 of the jenkinsci/blueocean container to port 50000 on the host machine. This is only necessary if you have set up one or more inbound Jenkins agents on other machines, which in turn interact with the jenkinsci/blueocean container (acting as the "master" Jenkins server, or simply "Jenkins master"). inbound Jenkins agents communicate with the Jenkins master through TCP port 50000 by default. You can change this port number on your Jenkins master through the Configure Global Security page. If you were to change your Jenkins master’s TCP port for inbound Jenkins agents value to 51000 (for example), then you would need to re-run Jenkins (via this docker run …​ command) and specify this "publish" option with something like --publish 52000:51000, where the last value matches this changed value on the Jenkins master and the first value is the port number on the Jenkins master’s host machine through which the inbound Jenkins agents communicate (to the Jenkins master) - i.e. 52000. Note that WebSocket agents in Jenkins 2.217 do not need this configuration.

Maps the /var/jenkins_home directory in the container to the Docker volume with the name jenkins-data. Instead of mapping the /var/jenkins_home directory to a Docker volume, you could also map this directory to one on your machine’s local file system. For example, specifying the option

--volume $HOME/jenkins:/var/jenkins_home would map the container’s /var/jenkins_home directory to the jenkins subdirectory within the $HOME directory on your local machine, which would typically be /Users/<your-username>/jenkins or /home/<your-username>/jenkins. Note that if you change the source volume or directory for this, the volume from the docker:dind container above needs to be updated to match this.
 
Maps the /certs/client directory to the previously created jenkins-docker-certs volume. This makes the client TLS certificates needed to connect to the Docker daemon available in the path specified by the DOCKER_CERT_PATH environment variable.

The jenkinsci/blueocean Docker image itself. If this image has not already been downloaded, then this docker container run command will automatically download the image for you. Furthermore, if any updates to this image were published since you last ran this command, then running this command again will automatically download these published image updates for you.

Note: This Docker image could also be downloaded (or updated) independently using the docker image pull command:
docker image pull jenkinsci/blueocean

# Jenkins

####### setup and run jenkins using docker /podamn containers 

**Create the following volumes to persist the Jenkins data using the following docker volume create commands:

 podman volume create jenkins-data
 
Download the jenkinsci/blueocean image and run it as a container in podman using the following podman container runcommand

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

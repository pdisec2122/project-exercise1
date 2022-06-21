First, create a Jenkins container with additional permissions to access docker.sock (docker socket so it can create images)


Build the custom docker image in this folder, then start the container:

# Permission to the docker socket
sudo chmod 777 /var/run/docker.sock



# Build the container image
docker build . -t customjenkins

# Run the container jenkins
sudo mkdir -p /var/jenkins_home

sudo chmod -R 777 /var/jenkins_home

docker run -p 8080:8080 -p 50000:50000 -v /var/jenkins_home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock --name jenkins -d customjenkins:latest

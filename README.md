#I use this script for Ubuntu 22-04 server it is fine.
```
nano install_docker.sh

```

# docker_installation_bath_scripts
```


#!/bin/bash

# Update the package index
sudo apt-get update

# Install necessary dependencies
sudo apt-get install -y ca-certificates curl gnupg

# Create the directory for the apt keyrings
sudo install -m 0755 -d /etc/apt/keyrings

# Download Docker's GPG key and add it to the apt keyring
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Set appropriate permissions for the Docker GPG key
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add Docker repository to the apt sources list
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Update the package index again after adding the Docker repository
sudo apt-get update

# Install Docker packages
sudo apt-get install -y docker-ce docker-ce-cli containerd.io

# Install Docker Buildx plugin
sudo apt-get install -y docker-buildx

# Install Docker Compose
sudo apt-get install -y docker-compose

# Add the current user to the 'docker' group to run Docker commands without sudo
sudo usermod -aG docker $USER

# Start and enable the Docker service
sudo systemctl start docker
sudo systemctl enable docker

# Test Docker installation by running the hello-world container
sudo docker run hello-world
```
```
chmod +x install_docker.sh
sudo ./install_docker.sh
```
```
ayk@ubuntu-server-22-04:~$ docker images
permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/images/json": dial unix /var/run/docker.sock: connect: permission denied
ayk@ubuntu-server-22-04:~$ ls -l /var/run/docker.sock
srw-rw---- 1 root docker 0 May 21 13:55 /var/run/docker.sock
ayk@ubuntu-server-22-04:~$ sudo chmod 666 /var/run/docker.sock
[sudo] password for ayk:
ayk@ubuntu-server-22-04:~$ ls -l /var/run/docker.sock
srw-rw-rw- 1 root docker 0 May 21 13:55 /var/run/docker.sock
ayk@ubuntu-server-22-04:~$ docker images
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
hello-world   latest    d2c94e258dcb   12 months ago   13.3kB
ayk@ubuntu-server-22-04:~$

```

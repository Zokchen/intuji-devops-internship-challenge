### 1) create bash script for docker installation

***
#!/bin/bash

# Update the package index
sudo apt update

# Install packages to allow apt to use a repository over HTTPS
sudo apt install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common

# Add Docker's official GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Set up the stable Docker repository
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Update the package index again
sudo apt update

# Install Docker
sudo apt install -y docker-ce docker-ce-cli containerd.io

# Add your user to the 'docker' group to run Docker commands without sudo
sudo usermod -aG docker $USER

# Display a message to inform the user to log out and log back in to apply group changes
echo "Docker has been installed. Please log out and log back in to apply group changes."

***

### 2)  create an executable file : chmod +x install_docker.sh

### 3) run the file: ./install_docker.sh

### 4) create docker file:

   ***********

# Use an official Nginx runtime as a parent image
FROM nginx:latest

# Set the working directory to the default Nginx web root directory
WORKDIR /usr/share/nginx/html

# Copy the current directory contents into the container at the working directory
COPY . .

# Expose port 80 to allow external access to the web application
EXPOSE 80

# Define the default command to run when the container starts
CMD ["nginx", "-g", "daemon off;"]


**************

### 5) build the docker image: docker build -t my-php-app .

### 6) run the docker image: docker run -p 8080:80 my-php-app

### 7) tag docker image: docker tag my-php-app:latest zoks/my-php-app:latest

### 8) login into docker: docker login

### 9) push docker image: docker push zoks/my-php-app:latest

### 10) create docker-compose.yml file:

****

version: '3'

services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    volumes:
      - ./dockerized-php-app:/usr/share/nginx/html
    depends_on:
      - php

  php:
    build:
      context: ./dockerized-php-app
      dockerfile: Dockerfile
    volumes:
      - ./dockerized-php-app:/var/www/html
****


### 11) run command for docker compose: docker-compose up -d

### 12) Create a directory to store Jenkins data: mkdir -p ~/jenkins_home

### 13) Run Jenkins as a Docker container: docker run -d -p 8081:8080 -p 50000:50000 -v ~/jenkins_home:/var/jenkins_home --name jenkins jenkins/jenkins:lts

### 14) Copy the password and paste it in the Jenkins web interface to unlock Jenkins: docker exec -it jenkins cat /var/jenkins_home/secrets/initialAdminPassword

### 15) Install Jenkins Plugins

Docker Pipeline
GitHub

### 16) Set Up GitHub Integration

Add your GitHub credentials.

### 17) Create a Freestyle Project

In the project configuration:

Source Code Management:

Select "Git."
Enter your GitHub repository URL.
Add your GitHub credentials.

Build:
Click "Add build step" > "Execute shell."

Enter the build commands, such as:
bash
Copy code
docker build -t my-php-app .

Post-build Actions:
Click "Add post-build action" > "Archive the artifacts."
Specify the files or directories to archive (e.g., dockerized-php-app).
Save the project configuration.

### 18) Trigger the Build
Back on the Jenkins dashboard, click on your project.

Click "Build Now" to trigger the build.



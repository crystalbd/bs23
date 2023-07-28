Project Deployment

Step 1: Develop Laravel Application
Create the Laravel application in respective src directory.

Step 2: Create Dockerfile
Create Dockerfile for application in respective build directory.
See app2/build/Dockerfile for the dockerfile configuration.

Step 3: Host Docker Image on DockerHub
Build and push the Docker images to your DockerHub account.
Replace <DOCKERHUB_USERNAME> to your DockerHub account username.

# cd build
# docker build -t <DOCKERHUB_USERNAME>/app2:latest .
# docker push <DOCKERHUB_USERNAME>/app2:latest

Step 4: Create Kubernetes Deployment, ConfigMap, and Service YAMLs
Create the Kubernetes Deployment YAML file for the application.
Configuration for the app2-deployment.yaml is under deploy/app2-deployment.yaml

Review the nodeSelector field in your app2-deployment.yaml file to ensure that this correctly configured to match the node labels.

Create the Kubernetes ConfigMap YAML file for app2 environment variables.
Configuration for the app2-configmap.yaml is under deploy/app2-configmap.yaml

Create the Kubernetes Service YAML file for the application.
Configuration for the app2-service.yaml is under deploy/app2-service.yaml

Step 5: Apply these YAML files in the Kubernetes (k3s) cluster
change directory
# cd deploy
Apply the ConfigMaps first
# kubectl apply -f app2-configmap.yaml
Apply the Deployment
# kubectl apply -f app2-deployment.yaml
Apply the Service
# kubectl apply -f app2-service.yaml

Step 6: Check node & pod status
# kubectl get nodes
# kubectl get pods
# kubectl get pods -o wide

The services for app2 are of type NodePort, which will allow you to access the applications using the Node IP and NodePort as mentioned:

App2 URL: http://nodeip:nodeport/app2


Step 7: set up Jenkins using Docker Compose
Create a docker-compose.yml file to run Jenkins as a container.
For run jenkins docker-compose up -d

version: '3'
services:
  jenkins:
    image: jenkins/jenkins:lts
    container_name: jenkins
    ports:
      - "8080:8080"
      - "50000:50000"
    volumes:
      - jenkins_home:/var/jenkins_home
      - /var/run/docker.sock:/var/run/docker.sock
    restart: always
volumes:
  jenkins_home:


Step 8:  Create Jenkins pipeline
Jenkinsfile configuration under Jenkins/Jenkinsfile.
Modify your Jenkinsfile to make use of the environment variable for DockerHub credentials.
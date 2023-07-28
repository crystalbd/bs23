Project Deployment

Step 1: Develop Laravel Application
Create the Laravel application in respective src directory.

Step 2: Create Dockerfile
Create Dockerfile for application in respective build directory.
See app1/build/Dockerfile for the dockerfile configuration.

Step 3: Host Docker Image on DockerHub
Build and push the Docker images to your DockerHub account.
Replace <DOCKERHUB_USERNAME> to your DockerHub account username.

 cd build
 docker build -t <DOCKERHUB_USERNAME>/app1:latest .
 docker push <DOCKERHUB_USERNAME>/app1:latest

Step 4: Create Kubernetes Deployment, ConfigMap, and Service YAMLs
Create the Kubernetes Deployment YAML file for the application.
Configuration for the app1-deployment.yaml is under deploy/app1-deployment.yaml

Review the nodeSelector field in your app1-deployment.yaml file to ensure that this correctly configured to match the node labels.

Create the Kubernetes ConfigMap YAML file for app1 environment variables.
Configuration for the app1-configmap.yaml is under deploy/app1-configmap.yaml

Create the Kubernetes Service YAML file for the application.
Configuration for the app1-service.yaml is under deploy/app1-service.yaml

Step 5: Apply these YAML files in the Kubernetes (k3s) cluster
change directory
 cd deploy
Apply the ConfigMaps first
 kubectl apply -f app1-configmap.yaml
Apply the Deployment
 kubectl apply -f app1-deployment.yaml
Apply the Service
 kubectl apply -f app1-service.yaml

Step 6: Check node & pod status
 kubectl get nodes
 kubectl get pods
 kubectl get pods -o wide

The services for app1 are of type NodePort, which will allow you to access the applications using the Node IP and NodePort as mentioned:
App1 URL: http://nodeip:nodeport/app1

Step 7:  Create Jenkins pipeline
Jenkinsfile configuration under Jenkins/Jenkinsfile.
Modify your Jenkinsfile to make use of the environment variable for DockerHub credentials.

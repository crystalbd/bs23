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

For creating Jenkins Pipeline we need to login to the Jenkins server that we configured earlier.
Here are bellow steps to configure the pipeline:
  1. From the Dashboard click on New Item.
  2. Enter your pipeline name in the item name field.
  3. Select the Pipeline tab then click ok.
  4. In General Section click GitHub project & paste GitHub URL.
  5. In the Build Triggers section click the GitHub hook for GITScm polling.
  6. In the Pipeline section select Pipeline script to paste your pipeline config from app1> Jenkins> Jenkinsfile in the project directory.
     Click Pipeline Syntax to store secret credentials. From the Sample Step section use 'withCredentials: Bind credentials to variables'. Click on add in the 
     Bindings section and select 'Secret text'. Write a variable name & click Add > Jenkinns. In the kind section select Secret text. In the Secret section write your 
     DockerHub credential & click on Add. Then click on Generate Pipeline Script. cope the pipeline script to update the push to docker hub section.

  7. If we want to use remote Jenkinsfile then skip point 6 and select Pipeline script from SCM in the Pipeline section. In the SCM section select Git. In the Repositories      section paste the project github url in the Repository URL field. In the Credentials section click Add > Jenkins then select Secret text as Kind. In the Secret field set your DockerHub Credentials. In ID field write the variable name that is set on the env section on Jenkinsfile. set your branch in the Branch Specifier field. Script Path should be a remote Jenkinsfile path. In this project, we use app1/Jenkins/Jenkinsfile. Now click on save.

  8. To run this pipeline click on the Build Now tab. 
  9. To monitor the running process click the last created build number then console output.

# BS23 Assignment

Directory Structure

directory root                                                                                                                                                      
├──  app1                                                                                                                                                           
│    ├── src                                                                                                                                                       
│    ├── build                                                                                                                                                      
│    └── deploy                                                                                                                                                     
│        ├── app1-deployment.yaml                                                                                                                                   
│        ├── app1-service.yaml                                                                                                                                      
│        └── app1-configmap.yaml                                                                                                                                    
│    └── Jenkinsfile                                                                                                                                                
       └── Jenkinsfile                                                                                                                                              
│    └── readme.md                                                                                                                                                  
├──  app2                                                                                                                                                           
│    ├── src                                                                                                                                                        
│    ├── build                                                                                                                                                      
│    └── deploy                                                                                                                                                     
│       ├── app2-deployment.yaml                                                                                                                                   
│       ├── app2-service.yaml                                                                                                                                     
│       └── app2-configmap.yaml                                                                                                                                    
│    └── Jenkinsfile                                                                                                                                                
        └── Jenkinsfile                                                                                                                                             
│    └── readme.md                                                                                                                                                  
├──  nginx                                                                                                                                                          
│    └── deploy                                                                                                                                                     
│       ├── nginx-deployment.yaml                                                                                                                                  
│       └── nginx-configmap.yaml                                                                                                                                   
│    └── readme.md                                                                                                                                                  
├──  infra                                                                                                                                                          
│    ├── config                                                                                                                                                     
│    └── readme.md
├──  Istio                                                                                                                                                          
│    └── readme.md                                                                                                                                                  
├──  Jenkins                                                                                                                                                        
│    └── docker-compose.yml                                                                                                                                         
└── readme.md                                                                                                                                                                                        

Project Guide:

1.  Set up the Project Repository
    Create a Git repository on GitHub/GitLab/Bitbucket with the folder structure as mentioned above.
    [Project Repository](https://github.com/crystalbd/bs23)

3.  Create Laravel PHP Applications
    Create two Laravel PHP applications (App 1 and App 2) with the respective static HTML pages as mentioned in the requirements.
    [App1 Source](https://github.com/crystalbd/bs23/tree/main/app1/src) [App2 Source](https://github.com/crystalbd/bs23/tree/main/app2/src)

5.  Create Dockerfiles for the Laravel Applications
    Create two Dockerfiles, one for each application, to build Docker images.
    [App1 Dockerfile](https://github.com/crystalbd/bs23/tree/main/app1/build/Dockerfile) [App2 Dockerfile](https://github.com/crystalbd/bs23/tree/main/app2/build/Dockerfile)

7.  Push Docker Images to DockerHub
    Build the Docker images for App 1 and App 2 and push them to your DockerHub account.
    [App1](https://github.com/crystalbd/bs23/tree/main/app1) [App2](https://github.com/crystalbd/bs23/tree/main/app2)

9.  Create Kubernetes YAML Files
    Create separate YAML files for the Kubernetes deployment, ConfigMap, and Service for each application. Also, create YAML files for the Nginx deployment.
    [App1 Kubernetes YAML](https://github.com/crystalbd/bs23/tree/main/app1/deploy) [Instructions](https://github.com/crystalbd/bs23/tree/main/app1)
    [App2 Kubernetes YAML](https://github.com/crystalbd/bs23/tree/main/app2/deploy) [Instructions](https://github.com/crystalbd/bs23/tree/main/app2)   

11.  Set up Lightweight Kubernetes Cluster
    Choose the lightweight Kubernetes distribution of your choice (k3s, minikube, k0s) and set it up on your local machine. Provide detailed instructions in the infra folder on how to set up the chosen Kubernetes distribution.
[Kubernetes k3s Installation](https://github.com/crystalbd/bs23/tree/main/infra)

13. Create Jenkins Pipelines
    Create two Jenkins pipelines (Jenkinsfile) for each application to build and deploy the applications into the lightweight Kubernetes cluster. Instructions for setting up Jenkins using Docker Compose should be provided in the root Jenkinsfile.
    [App1 Jenkins Pipeline}(https://github.com/crystalbd/bs23/tree/main/app1/Jenkinsfile/Jenkinfile) [Instruction](https://github.com/crystalbd/bs23/tree/main/app1) 
[App2 Jenkins Pipeline}(https://github.com/crystalbd/bs23/tree/main/app2/Jenkinsfile/Jenkinfile) [Instruction](https://github.com/crystalbd/bs23/tree/main/app1)
[Jenkins Settings](https://github.com/crystalbd/bs23/blob/main/Jenkins/)

15.  Customize Kubernetes Deployment Files
    Update the Kubernetes deployment files to use the Docker images built by the Jenkins pipeline. This can be achieved by using placeholders for image names and environment variables. During deployment, the placeholders can be replaced with the actual image names and environment variables through the Jenkins pipeline.
[App1 Kubernetes Deployment Files](https://github.com/crystalbd/bs23/tree/main/app1/deploy) [App2 Kubernetes Deployment Files](https://github.com/crystalbd/bs23/tree/main/app2/deploy)

17.  Deploy the Applications and Nginx
    Provide detailed instructions in the respective readme.md files on how to deploy the applications (App 1 and App 2) and Nginx to the lightweight Kubernetes cluster using the provided YAML files.
[Nginx Deployment](https://github.com/crystalbd/bs23/tree/main/nginx)

19. Common Anti-Patterns and Security Concerns
    Address common anti-patterns and security concerns by ensuring proper permissions, using secrets for sensitive information, setting resource limits for containers, enabling Pod Security Policies (PSP), and implementing RBAC (Role-Based Access Control) to restrict access to sensitive resources.

20. Scaling the Solution
    To scale the solution in the future, consider using Kubernetes Horizontal Pod Autoscaler (HPA) and Cluster Autoscaler for automatic scaling based on resource utilization. Implement a CI/CD pipeline to automate the process of building and deploying applications and updating the Kubernetes YAML files.

21. Bonus: Integrating Service Mesh
    If you wish to integrate a service mesh for API traffic routing, consider using tools like Istio or Linkerd. Service mesh can handle advanced traffic management, observability, and security features for microservices-based architectures.
    [Integrating Service Mesh](https://github.com/crystalbd/bs23/tree/main/nginx)

With this folder structure, codebase, Dockerfiles, Kubernetes YAML files, Jenkins pipelines, and detailed instructions, anyone can start Jenkins in a container using Docker Compose, set up two pipelines using Jenkinsfiles from the public repository, build the code, and push Docker images to their DockerHub account. They can then use these Docker images with Kubernetes YAML files to deploy App 1 and App 2 into the lightweight Kubernetes cluster. The separate instructions provided for deploying Nginx and updating its config to point to App 1 and App 2 will help users in achieving the desired outcome.



# About Me:                                                                                                                                                         
N M Atikur Rahman                                                                                                                                                   
DevOps Engineer                                                                                                                                                     
E-mail: atikur.it@gmail.com                                                                                                                                         
Linkdin: linkedin.com/in/atikurbd/                                                                                                                                  

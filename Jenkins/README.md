Run Jenkins Container using Docker Compose file

Step 1: Create a Docker Compose file to run Jenkins.
Create a file named docker-compose.yml with the following content:

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
      - .env:/var/jenkins_home/.env                                                                                                                             
    restart: always                                                                                                                                
volumes:                                                                                                                                               
  jenkins_home:                                                                                                                              


Step 2: Apply Docker Compose file
For run the Jenkins container apply the docker-compose.yml file with following command
docker compose up -d                                                                                                                                          

Step 3: Login Jenkins
To login the Jenkins container simply browse from any browser with hostip:8080

Step 4: Configure Jenkins
To configure Jenkins follow the browser instructions.
For access the secret from Jenkins server use docker exec -it jenkins cat /var/jenkins_home/secrets/initialAdminPassword
  then set user & pass. Configure plugins as recommended. Finish the steps to complete the configuration.
  
Step 5: To install plugins as required like (ex. docker) from the dashboard click Manage Jenkins > Plugins > Available plugins > search required plugins > Install.

Step 6: To configure Remote Nodes, From Dashboard click Manage Jenkins then Nodes and Clouds. Click New Node Tab. Write the Node name and set as Permanent Agent then click Create. Set the Remote root directory for this Node. Set the Identical Label that we use in Jenkins Pipeline. Launch method will be 'Launh agents via SSH'. Configure Host & Access Credentials for the remote server. Host Key Verification Strategy will be 'Non verifying Verification Strategy'. Click on the Save button. Remote Node will be configured successfully. To ensure check Node status.

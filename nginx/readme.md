Nginx Deployment in Kubernetes (k3s)

Step 1: Create the necessary YAML files:
1. nginx-deployment.yaml - Deployment for nginx that will handle routing to  
   app1 and app2 based on the API address.
   configuration details in nginx-deployment.yaml under deploy folder.

2. nginx-configmap.yaml - ConfigMap for nginx configuration.
   configuration details in nginx-configmap.yaml under deploy folder.

3. Update Node IP & Port in nginx-configmap.yaml at proxy_pass section.
To see Service running & get IP use this command
# kubectl get services

Step 2:  Apply these YAML files in the Kubernetes (k3s) cluster:
Apply the ConfigMaps first
# kubectl apply -f nginx-configmap.yaml

Apply the Deployments
# kubectl apply -f nginx-deployment.yaml





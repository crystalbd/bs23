# Set Up and Configure Istio

Install Istio
Step 1: Download and extract the Istio release                                                                                                                   
   curl -L https://istio.io/downloadIstio | sh -                                                                                                              
   cd istio-X.X.X                                                                                                                                                 
   export PATH=$PWD/bin:$PATH                                                                                                                                     
   istioctl install --set profile=demo                                                                                                                                                                                                                                                      
Step 2: Enable Istio Sidecar Injection to the namespace                                                                                                           
   kubectl label namespace app1 istio-injection=enabled                                                                                                           
   kubectl label namespace app2 istio-injection=enabled                                                                                                           

Step 3: Update Kubernetes Deployment Files                                                                                                             
Update the Kubernetes deployment files for app1 and app2 to use Istio as the sidecar proxy.
Add bellow lines to the app2/deploy/app2-deployment.yaml container section 
Add the Istio sidecar proxy container

      - name: istio-proxy                                                                                                                                         
        image: docker.io/istio/proxyv2:1.12.0                                                                                                                     
        args:                                                                                                                                                     
        - proxy
        - sidecar                                                                                                                                                 
        - --configPath                                                                                                                                            
        - /etc/istio/proxy                                                                                                                                        
        - --binaryPath                                                                                                                                            
        - /usr/local/bin/envoy                                                                                                                                    
        volumeMounts:                                                                                                                                             
        - name: istio-certs                                                                                                                                       
          mountPath: /etc/certs                                                                                                                                   
          readOnly: true                                                                                                                                          
        - name: istio-envoy                                                                                                                                       
          mountPath: /usr/local/bin/envoy                                                                                                                         
          subPath: envoy                                                                                                                                          
        - name: istio-config                                                                                                                                      
          mountPath: /etc/istio/proxy                                                                                                                             
          readOnly: true                                                                                                                                          
      volumes:                                                                                                                                                    
      - name: istio-certs                                                                                                                                         
        secret:                                                                                                                                                   
          secretName: istio.default                                                                                                                               
          optional: true                                                                                                                                          
      - name: istio-envoy                                                                                                                                         
        configMap:                                                                                                                                                
          name: istio-envoy                                                                                                                                       
          optional: true                                                                                                                                          
      - name: istio-config                                                                                                                                        
        configMap:                                                                                                                                                
          name: istio                                                                                                                                             


Add bellow lines to the app2/deploy/app2-deployment.yaml container section 
Add the Istio sidecar proxy container

      - name: istio-proxy                                                                                                                                         
        image: docker.io/istio/proxyv2:1.12.0                                                                                                                     
        args:                                                                                                                                                     
        - proxy                                                                                                                                                   
        - sidecar                                                                                                                                                 
        - --configPath                                                                                                                                            
        - /etc/istio/proxy                                                                                                                                        
        - --binaryPath                                                                                                                                            
        - /usr/local/bin/envoy                                                                                                                                    
        volumeMounts:                                                                                                                                             
        - name: istio-certs                                                                                                                                       
          mountPath: /etc/certs                                                                                                                                  
          readOnly: true                                                                                                                                          
        - name: istio-envoy                                                                                                                                      
          mountPath: /usr/local/bin/envoy                                                                                                                        
          subPath: envoy                                                                                                                                  
        - name: istio-config                                                                                                                                      
          mountPath: /etc/istio/proxy                                                                                                                            
          readOnly: true                                                                                                                                          
      volumes:                                                                                                                                                    
      - name: istio-certs                                                                                                                                         
        secret:                                                                                                                                                   
          secretName: istio.default                                                                                                                             
          optional: true
      - name: istio-envoy
        configMap:
          name: istio-envoy
          optional: true                                                                                                                                          
      - name: istio-config                                                                                                                                    
        configMap:                                                                                                                                              
          name: istio                                                                                                                                            

Step 4: Deploy the Updated Applications
Apply the updated deployment files for app1 and app2 in their location:
   kubectl apply -f app1-deployment.yaml -n app1                                                                                                                  
   kubectl apply -f app2-deployment.yaml -n app2                                                                                                                  

The Istio sidecar proxies will now be injected automatically into the Pods of app1 and app2.

Step 5: Configure API Traffic Routing with Istio VirtualService
Create an Istio VirtualService to route the API traffic based on the path 
app1-virtualservice.yaml                                                                                                                                      

apiVersion: networking.istio.io/v1alpha3                                                                                                                         
kind: VirtualService                                                                                                                                              
metadata:                                                                                                                                                       
  name: app1-virtualservice                                                                                                                                       
spec:                                                                                                                                                             
  hosts:                                                                                                                                                          
  - "*"                                                                                                                                                           
  gateways:                                                                                                                                                       
  - istio-system/ingressgateway                                                                                                                                   
  http:                                                                                                                                                           
  - match:                                                                                                                                                        
    - uri:                                                                                                                                                       
        prefix: "/app1"                                                                                                                                           
    route:                                                                                                                                                        
    - destination:                                                                                                                                                
        host: app1-service                                                                                                                                        
        port:                                                                                                                                                     
          number: 80                                                                                                                                            


app2-virtualservice.yaml

apiVersion: networking.istio.io/v1alpha3                                                                                                                          
kind: VirtualService                                                                                                                                              
metadata:                                                                                                                                                         
  name: app2-virtualservice                                                                                                                                       
spec:                                                                                                                                                             
  hosts:                                                                                                                                                          
  - "*"                                                                                                                                                           
  gateways:                                                                                                                                                       
  - istio-system/ingressgateway                                                                                                                                   
  http:                                                                                                                                                           
  - match:                                                                                                                                                        
    - uri:                                                                                                                                                        
        prefix: "/app2"                                                                                                                                           
    route:                                                                                                                                                        
    - destination:                                                                                                                                                
        host: app2-service                                                                                                                                        
        port:                                                                                                                                                     
          number: 80                                                                                                                                              


Step 6: Apply the VirtualService YAML files:
   kubectl apply -f app1-virtualservice.yaml -n app1                                                                                                            
   kubectl apply -f app2-virtualservice.yaml -n app2                                                                                                                                    
Now, the API traffic to http://nodeip:nodeport/app1 will be routed to the app1 service, and http://nodeip:nodeport/app2 will be routed to the app2 service.


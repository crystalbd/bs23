# installation of K3s

Step 1: Install Kubectl
Go to the root path of the master node and run the below commands
# curl -LO https://dl.k8s.io/release/v1.26.0/bin/linux/amd64/kubectl
# chmod +x kubectl
# cp kubectl /usr/bin

check the kubectl version
# kubectl version --short

Update Master and worker nodes host file
# cat /etc/hosts

Step 2:
Install K3s in the Master server
# curl -sfL https://get.k3s.io | sh -

Once successfully installed, check k3s service status
# systemctl status k3s

k3s config file in the below path in Master
# cat /etc/rancher/k3s/k3s.yaml

Next, we need to copy the config file to use in kubectl.
# mkdir ~/.kube
# cp /etc/rancher/k3s/k3s.yaml ~/.kube/config

Check node status
# kubectl get nodes

Step 3 : Install k3s agent in WorkerNode
Go to the worker node and execute the below command,

# curl -sfL https://get.k3s.io | K3S_URL=${k3s_Master_url} K3S_TOKEN=${k3s_master_token} sh -
k3s_Master_url = https://k3smaster.devopsart.com:6443

k3s_master_token= "Get the token from the master by executing the below command"
# cat /var/lib/rancher/k3s/server/node-token 

Once the installation is successful, we can check the k3s agent status
# systemctl status k3s-agent.service

Step 4: K3s Installation validation
# kubectl get nodes

Step 5: Set lebels for worker nodes
# kubectl label nodes <node_name> node=node1
# kubectl label nodes <node_name> node=node2


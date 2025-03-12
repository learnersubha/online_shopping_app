How to deploy the application in k8s??
=> First create an EC2 instance and connect through ssh. 
 Then install the required things:-
    1. Docker (sudo apt-get install docker.io)
    2. install Kind 
On Linux:
# For AMD64 / x86_64
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.27.0/kind-linux-amd64
# For ARM64
[ $(uname -m) = aarch64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.27.0/kind-linux-arm64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind

On macOS:
# For Intel Macs
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.27.0/kind-darwin-amd64
# For M1 / ARM Macs
[ $(uname -m) = arm64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.27.0/kind-darwin-arm64
chmod +x ./kind
mv ./kind /some-dir-in-your-PATH/kind

On Windows in PowerShell:
curl.exe -Lo kind-windows-amd64.exe https://kind.sigs.k8s.io/dl/v0.27.0/kind-windows-amd64
Move-Item .\kind-windows-amd64.exe c:\some-dir-in-your-PATH\kind.exe

curl.exe -Lo kind-windows-amd64.exe https://kind.sigs.k8s.io/dl/v0.27.0/kind-windows-amd64
Move-Item .\kind-windows-amd64.exe c:\some-dir-in-your-PATH\kind.exe
      
  3. Install Kubectl  
    For Linux
  curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
    For MacOS
  curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/darwin/amd64/kubectl"    
    For Windows
  curl.exe -LO "https://dl.k8s.io/release/v1.32.0/bin/windows/amd64/kubectl.exe"  

 Then clone the repo where the application available.

 Now create a cluster.
 (If you want cluster creating .yaml file click:->)
 
 After creating cluster create a namespace, it is a  mechanism to organize and isolate groups of resources within a single cluster
 namespace.yaml:->
 
 Now creayte a pod. Pod is the smallest deployable units of computing that you can create and manage in Kubernetes
 pod.yaml:->
 
After creating pod create deployment for scaling. A Deployment manages a set of Pods to run an application workload,
 deployment.yaml:- https://github.com/learnersubha/online_shopping_app/blob/master/k8s-Deploy/Deploymentfile>
 
Now create a service to connect with the outer world. Kubernetes Services are API objects that enable network exposure for one or more cluster Pods. 
 service.yaml:->

 I created the KinD (Kubernetes in Docker) cluster so after create service our cluster port not mapped with my EC2 instance because my cluster is available in a docker container so If i mapped my cluster port with my EC2 instance the need to do port forwarding.
 kubectl port-forward svc/onlineshop-svc -n onlineshop-ns 5173:5173 --address=0.0.0.0

 Now go to your EC2 instance and go inbound rules and expose port 5173 and now check (ec2 IP:5173)


#Kubernetes Commands

## MINIKUBE

### start minikube 
minikube start

### check minikube status
minikube status

### stop minikube 
minikube stop



## PODS

### show list of pods
kubectl get pods

### show list of pods with more details
kubectl get pods -o wide —> for more details

### Run a pod with specific image
kubctl run nginx --image=nginx

### To delete the running pod
kubectl delete pod nginx --now




## NODES





ping pod_ip
ssh worker-1 ping pod_ip

kubectl port-forward --help
kubectl port-forward pod/nginx 8080:80  —> master node port 8080 is mapped with node’s 80. now requests can go towards pods. check http://localhost:8080/. (kind of tunnelling)

kubectl exec -it nginx -- bash —> to go inside pod



kubectl run nginx --image=nginx --dry-run=client -o yaml | tee nginx.yaml

—dry-run=client —> simulate the execution without actually performing commad.
-o —> format output as YAML
tee —> output to screen and file


————————————

kubectl explain pod.spec.restartPolicy

Imperative commands —> Create Replace Delete
These are the commands to instruct cluster to perform some operation directly. It only performs specific operation but are risky.

Declarative commands —> apply 
we write every operation state in YAML or JSON. it compare current state and new state and only apply changes if required to new state. Rather than performing individual command , we should prefer to represt desired state of resources in YAML.

steps:
create xyz.yaml file 
kubectl create -f nginx.yaml 
kubectl get pods

In real life we need multiple pods deployed so to keep one file create a pods.yaml


create 1 more for demo —> kubectl run ubuntu --image=ubuntu --dry-run=client -o yaml | tee ubuntu.yaml
merge to one file {cat nginx.yaml; echo ---; cat ubuntu.yaml } |  tee pods.yaml

now create pods.yaml for 2 pods & verify 
kubectl create -f pods.yaml
kubectl get pods

——————————————

make pods.yml better as there repeatative stuff.

apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: mypods
    name: mypods
spec:
  containers:
  - image: nginx
    name: nginx
    resources: {}
  - image: ubuntu
    name : ubuntu
    resources: {}
 dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}


kubectl create -f pods.yaml 
kubectl get pods -o wide

Now pod information is present, pod is running 2 containers inside.

Get pod info —> kubectl describe pod/mypods | more

any change in yaml the use —> kubectl apply -f pods.yaml
check logs in pod’s container —> kubectl logs pod/mypods -c nginx
to run command in pod’s container —> kubectl exec -it pod/mypods -c nginx <COMMAND>
to check logs of a container that is crashed and restatring —> kubectl logs pod/mypods -c nginx -p

delete pods —> kubectl delete pod/mypods --now or kubectl delete pod mypods --now

——————————————————

Kubernetes network model allows pods to communicate without NAT.
container restart  valid policy in Kubernetes  —> Never, Always & OnFailure
command allows you to view the YAML declaration specification from the command line —> kubectl explain [object]

In a Kubernetes Pod, what is used to run tasks that must complete successfully before the main application containers start? —> Init Containers


————————————————

NameSpaces

1. get all name spaces:  kubectl get namespaces or   kubectl get ns
NAME              STATUS   AGE
default           Active   4h50m
kube-node-lease   Active   4h50m
kube-public       Active   4h50m
kube-system       Active   4h50m


2. to get all the resources accorss all the namespaces —> kubectl get all -A

kubectl api-resources | more

kubectl create namespace mynamespace —> create a namespace
check —> kubectl get namespace

Create pod in namespace —> kubectl -n mynamespace run nginx --image=nginx

To view pod in namespace —>  kubectl -n mynamespace get pods -o wide

Delete in namespace —> kubectl -n mynamespace delete pod nginx

To view kubectl configuration —> kubectl config view
to change namespace from default to “mynamespace” —> kubectl config set-context  —current --namespace=mynamespace.

To delete namespace —> kubrctl delete namespace/mynamespace --now 

—————————————————
Deployment

create a deployment file for nginx —> kubectl create deployment nginx --image=nginx --dry-run=client -o yaml | tee ngnix-deployment.yaml 
deploy the deployment —> kubectl apply -f ngnix-deployment.yaml 



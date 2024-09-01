# POD
* Pod is the smallest unit of deployable of computing.
* Pod is a group of containers with shared resources and network resource.

# Imperative Approach:
Tell the K8s API what to create, run, replace, delete using CLI

# create a basic pod of nginx
1. **kubectl run nginx --image=nginx**
2. **kubectl create pod nginx-pod --image=nginx** (NOT STANDARD)
3. **kubectl create -f pods.yaml**  (Can also work with YAML)


```
pod/nginx created
```


## Show pod details:

**kubectl get pods**

```
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          7s
```

## Show pod details in details: 
**kubectl get pods -o wide**
```
NAME    READY   STATUS    RESTARTS   AGE    IP            NODE       NOMINATED NODE   READINESS GATES
nginx   1/1     Running   0          3m2s   10.244.0.30   minikube   <none>           <none>
```
## Delete pod: 
**kubectl delete pod nginx**
```
pod "nginx" deleted
```


# Declarative Approach:
Tell K8s by defining in YAML 

create **basic-pod.yaml** 

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```

## Deploy pod: 
**kubectl apply -f basic-pod.yaml**
```
pod/nginx created
```

## Show pod details:

**kubectl get pods**

```
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          7s
```

## Show pod details in details: 
**kubectl get pods -o wide**
```
NAME    READY   STATUS    RESTARTS   AGE    IP            NODE       NOMINATED NODE   READINESS GATES
nginx   1/1     Running   0          3m2s   10.244.0.30   minikube   <none>           <none>
```
## Delete pod: 
**kubectl delete pod nginx**
```
pod "nginx" deleted
```


## Multi-container pod (NGINX & UBUNTU)
**pods.yaml**
```
apiVersion: v1
kind: Pod
metadata:
  name: multi-container-pod
  labels:
    app: multi-container
spec:
  containers:
  - name: nginx-container
    image: nginx:1.19.0
    ports:
    - containerPort: 80
  - name: ubuntu-container
    image: ubuntu:20.04
    command: ["sleep", "infinity"]
```


## Get detailed pod description
**kubectl describe pod/mypods | more**

## Get 1 pod 1 container logs
**kubectl logs pod/mypods**

## Get 1 pod mutiple container logs for a specific container
**kubectl logs pod/mypods -c nginx**




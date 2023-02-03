# Homework 12 - Task2. K8S
## Task:

1. Get information about your worker node and save it in some file

2. Create a new namespace (all resources below will create in this namespace)

3. Prepare deployment.yaml file which will create a Deployment with 3 pods of Nginx or Apache and service for access to these pods via ClusterIP and NodePort. 
> * Show the status of deployment, pods and services. Describe all resources which you will create and logs from pods

4. Prepare two job yaml files:
> * One gets content via curl from an internal port (ClusterIP)
> * Second, get content via curl from an external port (NodePort)

5. Prepare Cronjob.yaml file which will test the connection to Nginx or Apache service every 3 minutes.

## Steps 1: Get information about your worker node and save it in some file
 
* Get information about nodes and save in file:
```
echo "kubectl get nodes -o wide" > info_nodes.txt  && kubectl get nodes -o wide >> info_nodes.txt && echo -e "\n" >> info_nodes.txt
```
<img width="1230" alt="image" src="https://user-images.githubusercontent.com/117667360/216373687-fddd9fb0-6770-4e29-be4b-3d652cc90a16.png">

* Add detail information about kubemaster node in file info_nodes.txt
```
echo "kubectl describe nodes kubemaster" >> info_nodes.txt  && kubectl describe nodes kubemaster >> info_nodes.txt && echo -e "\n" >> info_nodes.txt
```
* Add detail information about kubenode node in file info_nodes.txt
```
echo "kubectl describe nodes kubenode" >> info_nodes.txt  && kubectl describe nodes kubenode >> info_nodes.txt && echo -e "\n" >> info_nodes.txt
```
* File [info_nodes.txt](https://github.com/rlnq/homework12/blob/main/info_nodes.txt) with information about worker node

## Step 2: Create a new namespace (all resources below will create in this namespace)

* Create a new namespace with name 'newkubens':
```
kubectl create namespace newkubens
```
* List all namespaces:
```
kubectl get ns
```
<img width="1232" alt="image" src="https://user-images.githubusercontent.com/117667360/216475311-4cb8131b-e7f7-4912-b6ab-23d5a0d99d89.png">

## Step 3: Prepare deployment.yaml file which will create a Deployment with 3 pods of Nginx or Apache and service for access to these pods via ClusterIP and NodePort. 

* Our yaml file [deployment.yaml]() for creation Deployment with 3 pods of Nginx and service for access to these pods via ClusterIP and NodePort.
```
kubectl apply -f deployment.yaml
```
<img width="690" alt="image" src="https://user-images.githubusercontent.com/117667360/216477549-2deeb3f3-fe8c-4a2e-bf7d-264bae2869b5.png">

* Switch to 'newkubens' namespace:
```
kubectl config set-context --current --namespace=newkubens
```
* Check information about current namespace:
```
kubectl config get-contexts
```
* List all nodes:

<img width="1254" alt="image" src="https://user-images.githubusercontent.com/117667360/216481105-69d8966c-9c2a-4342-a460-9285bded0653.png">

* Make sure that our pods are created in the namespace we need:
```
kubectl get pods -o wide -A | grep newkubens
```
<img width="1259" alt="image" src="https://user-images.githubusercontent.com/117667360/216482162-94cb742a-af96-4281-aaea-70e0a87d23e5.png">

* List all services in the current namespace 'newkubens' with more details:
```
kubectl get service -o wide
```
<img width="1244" alt="image" src="https://user-images.githubusercontent.com/117667360/216482601-916aaf1b-eb73-426f-9c81-c99abd2ff27a.png">



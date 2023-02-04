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

# Steps 1: Get information about your worker node and save it in some file
 
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

# Step 2: Create a new namespace (all resources below will create in this namespace)

* Create a new namespace with name 'newkubens':
```
kubectl create namespace newkubens
```
* List all namespaces:
```
kubectl get ns
```
<img width="1232" alt="image" src="https://user-images.githubusercontent.com/117667360/216475311-4cb8131b-e7f7-4912-b6ab-23d5a0d99d89.png">

# Step 3: Prepare deployment.yaml file which will create a Deployment with 3 pods of Nginx or Apache and service for access to these pods via ClusterIP and NodePort. 
> * Show the status of deployment, pods and services. Describe all resources which you will create and logs from pods

* Our yaml file [deployment.yaml]() for creation Deployment with 3 pods of Nginx and service for access to these pods via ClusterIP and NodePort.
```
kubectl apply -f deployment.yaml -n newkubens
```
<img width="690" alt="image" src="https://user-images.githubusercontent.com/117667360/216477549-2deeb3f3-fe8c-4a2e-bf7d-264bae2869b5.png">

* List of all status of deployment, pods and services in newkubens namespace with more details:
```
kubectl get deployment,pods,services -n newkubens -o wide
```
<img width="1257" alt="image" src="https://user-images.githubusercontent.com/117667360/216547736-37040648-3378-4360-a0d7-f7f0500231f9.png">

* Logs from first pod:
```
kubectl logs nginx-deployment-866c5d4565-cbzkm -n newkubens
```
<img width="1260" alt="image" src="https://user-images.githubusercontent.com/117667360/216548756-a8e4098d-3c0c-45c2-b249-8078acfe5027.png">

* Logs from second pod:
```
kubectl logs pod/nginx-deployment-866c5d4565-mnr9k -n newkubens
```
<img width="1259" alt="image" src="https://user-images.githubusercontent.com/117667360/216549371-d1d7467b-94d0-4a4a-9e98-bf053f0374f0.png">

* Logs from third pods:
```
kubectl logs pod/nginx-deployment-866c5d4565-zlztk -n newkubens
```
<img width="1259" alt="image" src="https://user-images.githubusercontent.com/117667360/216549465-b6cba538-c26e-4ae3-a79a-d36a54f4de5d.png">

# Step 4: Prepare two job yaml files: one - gets content via curl from an internal port (ClusterIP) and second - get content via curl from an external port (NodePort)

* Run Job file [clusterip.yaml]():
```
kubectl apply -f clusterip.yaml
```
<img width="754" alt="image" src="https://user-images.githubusercontent.com/117667360/216553441-98b49a3c-f3e6-4dc2-bd6f-90b181f2d9bf.png">

* Get log from job.batch/curl-clusterip-job:
```
kubectl logs job.batch/curl-clusterip-job
```
<img width="1258" alt="image" src="https://user-images.githubusercontent.com/117667360/216554308-0fbc80f4-47d5-467f-90c6-16f576d3260e.png">

* Run Job file [nodeport.yaml]():
```
kubectl apply -f nodeport.yaml 
```
<img width="640" alt="image" src="https://user-images.githubusercontent.com/117667360/216554665-5619e239-a9bb-4b40-9d41-a559cd1130cc.png">

* Get logs from job.batch/curl-nodeport-job:
```
kubectl logs job.batch/curl-nodeport-job
```
ps: i don't have external ip!
<img width="1250" alt="image" src="https://user-images.githubusercontent.com/117667360/216555365-91ade330-5443-4aa3-898a-b0b30d21d7d7.png">

# Step 5: Prepare Cronjob.yaml file which will test the connection to Nginx service every 3 minutes.

Run CronJob file []():
```
kubectl apply -f cronjob.yaml
```
<img width="635" alt="image" src="https://user-images.githubusercontent.com/117667360/216557041-c68e0d62-11b8-4228-99bb-29870404d91e.png">


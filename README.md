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
kubectl get ns newkubens
```
<img width="510" alt="image" src="https://user-images.githubusercontent.com/117667360/216914471-9f8b32b3-1323-4943-94c8-1c5d824937f0.png">

# Step 3: Prepare deployment.yaml file which will create a Deployment with 3 pods of Nginx or Apache and service for access to these pods via ClusterIP and NodePort. 
> Show the status of deployment, pods and services. Describe all resources which you will create and logs from pods

* Our yaml file [deployment.yaml]() for creation Deployment with 3 pods of Nginx and service for access to these pods via ClusterIP and NodePort.
```
kubectl apply -f deployment.yaml -n newkubens
```
<img width="738" alt="image" src="https://user-images.githubusercontent.com/117667360/216915540-15fd732f-4f67-4d41-8016-541644d2817d.png">

* Verify the deployment in newkubens namespace with more details:
```
kubectl get deployment -n newkubens -o wide
```
<img width="840" alt="image" src="https://user-images.githubusercontent.com/117667360/216916341-1e720f85-42f6-41d3-80cf-1a29e8393c09.png">

* Describe the deployment:
```
kubectl describe deployment nginx-deployment -n newkubens
```
<img width="1259" alt="image" src="https://user-images.githubusercontent.com/117667360/216916723-55912974-5296-493d-944f-dab2c4dc18eb.png">

# Step 4: Prepare two job yaml files: one - gets content via curl from an internal port (ClusterIP) and second - get content via curl from an external port (NodePort)

* Run Job file [clusterip.yaml]():
```
kubectl apply -f clusterip.yaml -n newkubens
```
* Get log from job.batch/curl-clusterip-job:
```
kubectl logs job.batch/curl-clusterip-job
```
<img width="1237" alt="image" src="https://user-images.githubusercontent.com/117667360/216920854-29891806-096d-4eef-b82f-189098421937.png">

* Run Job file [nodeport.yaml]():
```
kubectl apply -f nodeport.yaml -n newkubens
```
* Get logs from job.batch/curl-nodeport-job:
```
kubectl logs job.batch/curl-nodeport-job
```
<img width="1253" alt="image" src="https://user-images.githubusercontent.com/117667360/217037000-5eea64a4-5658-4d6e-9c54-c488560de778.png">

# Step 5: Prepare Cronjob.yaml file which will test the connection to Nginx service every 3 minutes.

* Run CronJob file []():
```
kubectl apply -f cronjob.yaml -n newkubens
```
<img width="1159" alt="image" src="https://user-images.githubusercontent.com/117667360/217039467-37afa950-101b-4b92-8200-805c78c6152f.png">

* Get the Cronjob description:
```
kubectl describe cronjob nginx-cronjob
```
<img width="1259" alt="image" src="https://user-images.githubusercontent.com/117667360/217041266-33441bd4-188d-46c5-89ae-a367db3db885.png">

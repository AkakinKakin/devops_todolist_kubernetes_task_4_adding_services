1. Create a namespace

First, create a separate namespace to isolate the application:

kubectl create namespace todoapp

2. Deploy the application (Pods + Services)

Apply the manifest containing pods and services:

kubectl apply -f todoapp.yml -n todoapp


Check that the pods and services are running:

kubectl get pods -n todoapp
kubectl get svc -n todoapp

3. Test via ClusterIP Service

ClusterIP is only accessible inside the cluster, so use a busybox pod:

kubectl run busybox --rm -it --image=busybox -n todoapp -- sh


Inside the container, run:

wget -qO- http://todoapp-clusterip/api/health
wget -qO- http://todoapp-clusterip/api/ready
exit

4. Test via Port-Forward

To access the ClusterIP service from your local machine:

kubectl port-forward svc/todoapp-service 8080:80 -n todoapp


In another terminal:

curl http://localhost:8080/api/health
curl http://localhost:8080/api/ready

5. Test via NodePort Service

NodePort allows external access to the application via the node’s IP (e.g., port 30080).

First, find the node IP address:

kubectl get nodes -o wide


Then access the application:

curl http://<NODE_IP>:30080/api/health
curl http://<NODE_IP>:30080/api/ready
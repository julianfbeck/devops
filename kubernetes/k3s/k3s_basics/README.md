# k3s basics
Based on _Introduction to Kubernetes on the Edge with K3s_


- [k3s basics](#k3s-basics)
	- [Build and publish App using Docker](#build-and-publish-app-using-docker)
	- [Create Pod](#create-pod)
		- [Forward Port](#forward-port)
		- [Get logs](#get-logs)
		- [Get Ip](#get-ip)
	- [Add Service](#add-service)
		- [Now forward service traffic](#now-forward-service-traffic)
	- [Add Deployment](#add-deployment)
		- [See logs](#see-logs)
		- [Now forward service traffic](#now-forward-service-traffic-1)
	- [Add Ingress](#add-ingress)
		- [Port forward Treafik](#port-forward-treafik)
	- [Add Ingress with TLS](#add-ingress-with-tls)
		- [Port forward Treafik](#port-forward-treafik-1)
	- [Run Cron Job](#run-cron-job)
## Build and publish App using Docker
```
docker build -t flask-app:0.1.1 .
```
```
docker login --username <username>
```
```
docker build -t docker.io/<username>/flask-app:0.1.1 .
```
```
docker push <username>/flask-app:0.1.1
```
## Create Pod 

```
kubectl create -f pod.yaml
```
```
kubectl get pods
```
### Forward Port
```
kubectl port-forward flask-app 5000:5000
```
### Get logs
```
kubectl logs pod/flask-app
```
### Get Ip
```
kubectl get pod/flask-app --output go-template="{{.metadata.name}} {{ .status.podIP }}" ; echo;
```
## Add Service
Send traffic to pod by forwarding its service
```
kubectl apply -f service.yaml
```
### Now forward service traffic
```
kubectl port-forward service/flask-svc 5000:5000
```
## Add Deployment
A Deployment provides declarative updates for Pods and ReplicaSets.
```
kubectl create -f deployment.yaml
```
### See logs
```
kubectl logs deploy/flask-app
```
```
kubectl get pods
```
```
kubectl logs pod/flask-app-6c74fb9c7c-hr4s7
```
### Now forward service traffic
```
kubectl port-forward service/flask-svc 5000:5000
```
## Add Ingress
External Ingress to the load-balance
```
kubectl apply -f ingress.yaml
```
### Port forward Treafik
```
kubectl port-forward -n kube-system service/traefik 8080:80
```
## Add Ingress with TLS
Add a DNS host name to the Ingress record 
```
kubectl apply -f ingress-host.yaml
```
### Port forward Treafik
```
kubectl port-forward -n kube-system service/traefik 8080:80
```
## Run Cron Job
See yaml
```
kubectl apply -f ingress-host.yaml
```
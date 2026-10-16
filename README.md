# K8s Kind Voting App

A comprehensive guide for setting up a Kubernetes cluster using Kind on an AWS EC2 instance, installing and configuring Argo CD, and deploying applications using Argo CD.

## Overview

This guide covers the steps to:
- Launch an AWS EC2 instance.
- Install Docker and Kind.
- Create a Kubernetes cluster using Kind.
- Install and access kubectl.
- Set up the Kubernetes Dashboard.
- Install and configure Argo CD.
- Connect and manage your Kubernetes cluster with Argo CD.


## Architecture

![Architecture diagram](k8s-kind-voting-app.png)

## Observability

![Grafana diagram](grafana.png)
![Prometheus diagram](prometheus.png)

* A front-end web app in [Python](/vote) which lets you vote between two options
* A [Redis](https://hub.docker.com/_/redis/) which collects new votes
* A [.NET](/worker/) worker which consumes votes and stores them in…
* A [Postgres](https://hub.docker.com/_/postgres/) database backed by a Docker volume
* A [Node.js](/result) web app which shows the results of the voting in real time



## Resume Description

### Project Title: 

Automated Deployment of Scalable Applications on AWS EC2 with Kubernetes and Argo CD

### Description: 

Led the deployment of scalable applications on AWS EC2 using Kubernetes and Argo CD for streamlined management and continuous integration. Orchestrated deployments via Kubernetes dashboard, ensuring efficient resource utilization and seamless scaling.

### Key Technologies:

* AWS EC2: Infrastructure hosting for Kubernetes clusters.
* Kubernetes Dashboard: User-friendly interface for managing containerized applications.
* Argo CD: Continuous Delivery tool for automated application deployments.

### Achievements:

Implemented Kubernetes dashboard for visual management of containerized applications on AWS EC2 instances.
Utilized Argo CD for automated deployment pipelines, enhancing deployment efficiency by 60%.
Achieved seamless scaling and high availability, supporting 99.9% uptime for critical applications.
This project description emphasizes your role in leveraging AWS EC2, Kubernetes, and Argo CD to optimize application deployment and management processes effectively.
1. Install Helm via script

curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-4
chmod 700 get_helm.sh
./get_helm.sh

./get_helm.sh

To Verify
helm version

2. Install Prometheus repo via helm:-
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

helm repo list

3. Create monitoring namespace:
   
kubectl create namespace monitoring

4. Install Prometheus-stack and grafana:-

helm install prometheus-stack prometheus-community/kube-prometheus-stack --namespace monitoring --set prometheus.service.nodePort=30000 --set grafana.service.nodePort=31000 --set  grafana.service.type=NodePort --set prometheus.servicetype=NodePort

4. Check Service

kubectl get svc -n monitoring
NAME                                        TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
alertmanager-operated                       ClusterIP   None            <none>        9093/TCP,9094/TCP,9094/UDP   81s
prometheus-operated                         ClusterIP   None            <none>        9090/TCP                     78s
prometheus-stack-grafana                    NodePort    10.96.74.210    <none>        80:31000/TCP                 85s
prometheus-stack-kube-prom-alertmanager     ClusterIP   10.96.214.207   <none>        9093/TCP,8080/TCP            85s
prometheus-stack-kube-prom-operator         ClusterIP   10.96.22.64     <none>        443/TCP                      85s
prometheus-stack-kube-prom-prometheus       ClusterIP   10.96.61.87     <none>        9090/TCP,8080/TCP            85s
prometheus-stack-kube-state-metrics         ClusterIP   10.96.237.39    <none>        8080/TCP                     85s
prometheus-stack-prometheus-node-exporter   ClusterIP   10.96.246.122   <none>        9100/TCP                     85s
root@ip-172-31-38-172:~/monitoring# 

5. Port forwarding for Prometheus:
kubectl port-forward service/prometheus-stack-kube-prom-prometheus 9090:9090 -n monitoring --address=0.0.0.0 &






# Kubernetes Project Setup

## Overview
This project involves deploying a Java Spring Boot application and its associated services on a Kubernetes cluster. The deployment includes the following components:

- **Database Deployment**: Manages the database layer for the application.
- **Memcached Deployment**: Provides caching services to optimize performance.
- **RabbitMQ Deployment**: Handles messaging services between components.
- **Tomcat Deployment**: Hosts the Java Spring Boot application.
- **App Secrets Configuration**: Utilizes `app-secret.yaml` to securely manage sensitive application configuration and credentials.

Two approaches are used for the Kubernetes cluster setup:

1. **Using Vagrant and kubeadm**: A local environment is created to provision the Kubernetes cluster using Vagrant for virtual machine management and kubeadm for cluster initialization.
2. **Using Amazon Elastic Kubernetes Service (EKS)**: A managed Kubernetes service on AWS is configured to deploy the application in a cloud environment.

## Project Components

### 1. **Database Deployment**
The database deployment provides persistent storage for the application. It includes configuration for:
- Persistent volume claims (PVCs) for data storage.
- StatefulSet or Deployment strategies to ensure data reliability.

### 2. **Memcached Deployment**
Memcached acts as an in-memory key-value store for caching data. This deployment ensures reduced latency and improved performance.

### 3. **RabbitMQ Deployment**
RabbitMQ is used for asynchronous communication between the application services. The deployment includes configuration for message queues, exchanges, and routing.

### 4. **Tomcat Deployment**
Tomcat is configured to host the Spring Boot application. This deployment manages the application runtime environment, including:
- Application service definitions.
- Load balancing and auto-scaling configurations.

### 5. **App Secrets Configuration**
`app-secret.yaml` is used to manage sensitive credentials securely, such as:
- Database connection strings.
- Messaging broker credentials.
- Other application-specific secrets.

### 6. **Cluster Setup**
- **Vagrant and kubeadm**: This approach uses Vagrant to create virtual machines that act as Kubernetes nodes. kubeadm initializes and configures the Kubernetes cluster.
- **Amazon EKS**: This method leverages AWS’s managed Kubernetes service to deploy the cluster in a scalable and highly available cloud environment.

## Goals
The main objectives of this project include:
- Setting up a Kubernetes cluster for the Spring Boot application.
- Ensuring all components are correctly deployed and interact seamlessly.
- Using Infrastructure as Code (IaC) principles to automate cluster provisioning and management.
- Supporting both local development and cloud-based deployment.

## Prerequisites
To set up and deploy this project, ensure you have:
- Vagrant installed for local environment setup.
- AWS CLI and eksctl installed for EKS configuration.
- Docker installed for building and managing container images.
- Kubernetes CLI (`kubectl`) for managing the cluster.

## Deployment Workflow

### Local Cluster (Vagrant and kubeadm):
1. Initialize Vagrant to provision virtual machines:
   ```bash
   vagrant up
   ```
2. SSH into the Vagrant VM:
   ```bash
   vagrant ssh
   ```
3. Use kubeadm to initialize the Kubernetes cluster:
   ```bash
   sudo kubeadm init
   ```
4. Configure kubectl for the cluster:
   ```bash
   mkdir -p $HOME/.kube
   sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
   sudo chown $(id -u):$(id -g) $HOME/.kube/config
   ```
5. Apply deployment manifests:
   ```bash
   kubectl apply -f db-deployment.yaml
   kubectl apply -f memcached-deployment.yaml
   kubectl apply -f rabbitmq-deployment.yaml
   kubectl apply -f tomcat-deployment.yaml
   kubectl apply -f app-secret.yaml
   ```

### Cloud Cluster (AWS EKS):
1. Create an EKS cluster using eksctl:
   ```bash
   eksctl create cluster --name my-cluster --region us-west-2 --nodegroup-name standard-workers --node-type t2.medium --nodes 3
   ```
2. Verify the cluster:
   ```bash
   kubectl get nodes
   ```
3. Deploy application components:
   ```bash
   kubectl apply -f db-deployment.yaml
   kubectl apply -f memcached-deployment.yaml
   kubectl apply -f rabbitmq-deployment.yaml
   kubectl apply -f tomcat-deployment.yaml
   kubectl apply -f app-secret.yaml
   ```


## Conclusion
This project provides a flexible solution for deploying a Spring Boot application and its associated services on Kubernetes. By supporting both local and cloud-based cluster setups, it ensures adaptability for different development and production needs.


# **Voting App Project**

This is a **Voting App** built using Kubernetes and Minikube on an EC2 instance. The app allows users to vote on different options and view the results in real-time. The project uses multiple microservices, including a voting service and a result service, running in Kubernetes pods, exposed via `NodePort`.

## **Table of Contents**

- [Project Overview](#project-overview)
- [Technologies Used](#technologies-used)
- [Project Setup](#project-setup)
  - [Prerequisites](#prerequisites)
  - [Steps to Run](#steps-to-run)

## **Project Overview**

This project sets up a **Voting App** on an EC2 instance using **Minikube** and **Kubernetes**. The app consists of two primary services:
- **Voting Service**: Allows users to submit their votes.
![voting-screen](https://github.com/user-attachments/assets/9685a5a8-b20f-4148-a943-737b95855d85)
- **Result Service**: Displays the voting results in real-time.
![zResult-Screen](https://github.com/user-attachments/assets/e84deaf3-0f4b-46b3-953c-6985e886760b)



The services are exposed using Kubernetes **NodePort** to make them accessible through the EC2 instance's public IP.

## **Technologies Used**
- **Minikube**: For running a local Kubernetes cluster on EC2.
- **Kubernetes**: Orchestration platform to manage services and pods.
- **Docker**: For containerizing the voting app and result service.
- **EC2**: AWS virtual machine to run Minikube.
- **NodePort**: Kubernetes service type to expose the app services externally.

## **Project Setup**

### **Prerequisites**
- **EC2 Instance**: An EC2 instance running on AWS.
- **Security Group**: A security group for EC2 instance.
- **Minikube**: Installed and configured to run on EC2.
- **Kubernetes CLI (kubectl)**: Installed on your local machine to interact with the Kubernetes cluster.
- **Docker**: To build and run containers if needed.
- **Git**: To clone the repository.

### Security Group Setup:
   Create 3 inbound rules for following ports:
   1. 22
   2. 443
   3. 30000-32767

### **Steps to Run**

1. **Clone the repository**:
   To begin, SSH into your EC2 instance with your private key:

   ```bash
   ssh -i /path-to-your-key/key.pem ubuntu@<ec2-public-ip>
   
2. **Install Docker**:
    
   ##### 2.1 Update the instance:
   ```bash
    sudo apt update
    sudo apt upgrade -y
   ```

   ##### 2.2 Install Docker's official repository:
   ```bash
    sudo apt-get update
    sudo apt-get install ca-certificates curl
    sudo install -m 0755 -d /etc/apt/keyrings
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
    sudo chmod a+r /etc/apt/keyrings/docker.asc
    ```
   
   ##### 2.3 Add Docker repository to APT sources:
   ```bash
    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
    $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update
   ```
   
   ##### 2.4 Install Docker:
   ```bash
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```
   
3. **Install kubectl**:

   ##### 3.1 Download kubectl
   ```bash
    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
   ```

   ##### 3.2 Validate the kubectl binary
   output of this should be "kubectl: OK"
   ```bash
    echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
   ```

   ##### 3.3 Install kubectl
   ```bash
    sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
    kubectl version --client
   ```

4. **Install Minikube**:

   ##### 4.1 Download and install Minikube
   ```bash
    curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
    sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
   ```

   ##### 4.2 Add your current user to interact with Dcocker daemon
   output of this should be "kubectl: OK"
   ```bash
    sudo usermod -aG docker $USER
    newgrp docker
    groups
   ```

   ##### 4.3 Start Minikube cluster
   output of this should be "kubectl: OK"
   ```bash
    minikube start --driver=docker
   ```

   ##### 4.4 Verify Minikube status
   ```bash
    minikube status
   ```

5. **Clone the repository**:
   To begin, SSH into your EC2 instance with your private key:

   ```bash

   ```
 6. **Apply the deployments and services**:
   To begin, SSH into your EC2 instance with your private key:

     ```bash
      kubectl create -f voting-app-deploy.yaml
      kubectl create -f voting-service-deploy.yaml
      kubectl create -f redis-deploy.yaml
      kubectl create -f redis-service.yaml
      kubectl create -f postgres-deploy.yaml
      kubectl create -f postgres-service.yaml
      kubectl create -f worker-app-deploy.yaml
      kubectl create -f result-app-deploy.yaml
      kubectl create -f result-app-service.yaml
     ```

7. **Check Service and Port**

   ```bash
    kubectl get deployments,svc
   ```

8. **Access the voting-app**

   ```bash
   http://<ec2-public-ipv4>:<voting-service-port-number>
   http://<ec2-public-ipv4>:<result-service-port-number>
  ```

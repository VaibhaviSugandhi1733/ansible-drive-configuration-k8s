# **Ansible-Driven Configuration Management Inside Kubernetes Pods**  

🚀 **Ansible running inside Kubernetes Pods to configure and manage application containers** – fully manual, with no prebuilt images.

---

## **📌 Project Overview**  

✅ Build a **custom Ansible Docker image**  
✅ Deploy it as a **Pod inside Kubernetes**  
✅ Use Ansible to configure an **application container (Nginx Pod)** running in the same cluster  

---

## **🛠️ Tech Stack**  

- **Ansible (Custom image built on Ubuntu 20.04)**  
- **Kubernetes (Minikube for local testing)**  
- **Docker**  

---

## **📂 Project Structure**  
ansible_k8s_project/
├── Dockerfile.ansible # Custom Ansible image
├── ansible-pod.yaml # Pod definition for Ansible
├── nginx-pod.yaml # Target application pod
└── playbook.yml # Ansible playbook to configure app



---

## **⚡ Setup Instructions**

### **1️⃣ Prerequisites**  

Install Minikube & kubectl in Ubuntu:

sudo apt update && sudo apt upgrade -y
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
sudo dpkg -i minikube_latest_amd64.deb
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
minikube start --driver=docker





# 🧰 Ansible-Driven Configuration Management Inside Kubernetes Pods (MicroK8s)

This project demonstrates how to run **Ansible inside a Kubernetes Pod** to configure another application container (Nginx) within the same cluster — **without using any prebuilt Docker images**.

---

## 🚀 Project Objective

- 🔧 Build a **custom Ansible image** from scratch.
- 📦 Run it as a Pod inside **MicroK8s** (Kubernetes).
- 🔄 Use Ansible to configure a running **Nginx pod** by modifying its content.
- ✅ No internet pull — images are manually built and loaded into MicroK8s.

---

## 🗂️ Project Structure

```

ansible\_k8s\_project/
├── Dockerfile.ansible      # Dockerfile for custom Ansible image
├── ansible-pod.yaml        # Pod manifest for Ansible
├── nginx-pod.yaml          # Pod manifest for Nginx app
└── playbook.yml            # Ansible playbook to configure Nginx

````

---

## ✅ Prerequisites

- Ubuntu (WSL or native)
- [MicroK8s installed](https://microk8s.io/)
- Docker installed (`sudo apt install docker.io`)

---

## ⚙️ Step-by-Step Guide

### 1. Enable MicroK8s and Required Addons

```bash
sudo snap install microk8s --classic
sudo usermod -a -G microk8s $USER
newgrp microk8s

microk8s status --wait-ready
microk8s enable dns storage
alias kubectl='microk8s kubectl'
````

---

### 2. Build the Ansible Docker Image

```bash
docker build -t custom-ansible -f Dockerfile.ansible .
docker save custom-ansible:latest -o custom-ansible.tar
microk8s ctr image import custom-ansible.tar
```

---

### 3. Deploy the Ansible Pod

```bash
kubectl apply -f ansible-pod.yaml
kubectl get pods
```

---

### 4. Deploy the Target Nginx Application Pod

```bash
kubectl apply -f nginx-pod.yaml
```

---

### 5. Copy the Ansible Playbook to the Ansible Pod

```bash
kubectl cp playbook.yml ansible-pod:/playbooks/playbook.yml
```

---

### 6. Execute the Ansible Playbook

```bash
kubectl exec -it ansible-pod -- /bin/bash
cd /playbooks
ansible localhost -m copy -a "dest=/usr/share/nginx/html/index.html content='<h1>Configured by Ansible Inside Kubernetes Pod!</h1>'"
```

---

### 7. Verify Nginx Output

```bash
kubectl port-forward pod/nginx-app 8080:80
```

Visit: [http://localhost:8080](http://localhost:8080)

Expected output:

```
<h1>Configured by Ansible Inside Kubernetes Pod!</h1>
```

---

## 🧪 Testing

You can modify `playbook.yml` to perform additional configuration and re-run it using the same approach.

---

## 🧹 Cleanup

```bash
kubectl delete -f ansible-pod.yaml
kubectl delete -f nginx-pod.yaml
```

---

## 📌 Notes

* This project uses **`imagePullPolicy: Never`** to avoid pulling from Docker Hub.
* It works entirely offline once images are imported into MicroK8s using `ctr`.
* You can extend this by setting up SSH-enabled containers for more advanced Ansible playbooks.

---

## ✍️ Author
Vaibhavi Sugandhi
DevOps Engineer — CI/CD | Kubernetes | Ansible | Docker

---

## 📜 License

This project is licensed under the MIT License.



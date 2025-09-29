---
#  Kubernetes Cluster with Minikube

##  Objective
Deploy and manage applications in a **local Kubernetes cluster** using **Minikube, kubectl, and Docker**.

---

## Setup Instructions

```bash
1. Download the Minikube binary package using the wget command:
wget https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

2. Then, use the chmod command to give the file executive permission:
chmod +x minikube-linux-amd64

3. Finally, move the file to the /usr/local/bin directory:
sudo mv minikube-linux-amd64 /usr/local/bin/minikube

4. With that, you have finished setting up Minikube. Verify the installation by checking the version of the software:
minikube version
```
<img width="1427" height="517" alt="Screenshot 2025-09-29 191850" src="https://github.com/user-attachments/assets/ccdabe14-8b30-4fd3-a8a9-4fed0310a1de" />

### Verify Minikube installation.
## Installing Kubectl
```
Apart from installing Minikube, you also need to set up kubectl, the command line tool for working with Kubernetes.
1. Run the following command to download kubectl:
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl

3. Give it executive permission:
chmod +x kubectl
  
3. Move it to the same directory where you previously stored Minikube:
sudo mv kubectl  /usr/local/bin/
````
<img width="1842" height="592" alt="2" src="https://github.com/user-attachments/assets/9911e8ea-e318-403c-a9fc-21f8da71bd81" />

---

### 2️⃣ Start Minikube Cluster

```bash
minikube start --driver=docker
kubectl cluster-info
kubectl get nodes
```
<img width="1588" height="518" alt="3" src="https://github.com/user-attachments/assets/f8e0356f-4ed0-46a7-ba82-2472292aa502" />
<img width="1313" height="178" alt="4" src="https://github.com/user-attachments/assets/7dcb3e31-1a68-4285-9943-71b8b77fa29a" />
<img width="870" height="98" alt="5" src="https://github.com/user-attachments/assets/1811ff51-ac37-433d-a1fc-1e013ebcb330" />
---

### 3️⃣ Create a Deployment

vi **deployment.yaml**:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: nginx:latest
        ports:
        - containerPort: 80
```

## Apply it:

```bash
kubectl apply -f deployment.yaml
kubectl get pods
```
<img width="682" height="102" alt="7" src="https://github.com/user-attachments/assets/4f755d74-b439-4ef2-b005-9e6c36ab8bbc" />
<img width="766" height="131" alt="8" src="https://github.com/user-attachments/assets/d7989af7-c345-401d-9147-08d9d1e340a1" />

---

### 4️⃣ Expose the App as a Service

vi **service.yaml**:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-app-service
spec:
  selector:
    app: my-app
  type: NodePort
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

## Apply it:

```bash
kubectl apply -f service.yaml
kubectl get svc
```
<img width="668" height="90" alt="12" src="https://github.com/user-attachments/assets/4ffde483-e3cb-4ea2-a135-f310b22cce8d" />
<img width="847" height="132" alt="14" src="https://github.com/user-attachments/assets/2145b214-5edf-44c7-a12e-1b60797f3664" />

Access the app:

```bash
minikube service my-app-service
```

---

### 5️⃣ Scale the Deployment

```bash
kubectl scale deployment my-app --replicas=3
kubectl get pods
```
<img width="1180" height="365" alt="15" src="https://github.com/user-attachments/assets/f6404af4-0eb6-4247-8da9-7c7361443a50" />
<img width="1526" height="622" alt="16" src="https://github.com/user-attachments/assets/d2bf25d9-50ff-4b37-8310-deccef970a2f" />

---

### 6️⃣ Debug & Logs

```bash
kubectl describe deployment my-app
kubectl logs <pod-name>
```
<img width="1898" height="483" alt="image" src="https://github.com/user-attachments/assets/5be973ab-0976-4ae5-bcc8-9238e24cb08b" />

The kubectl logs command shows that your nginx pod started successfully, running its entrypoint script and loading configuration files. 
Nginx is configured for both IPv4 and IPv6, and the logs confirm the worker processes are running. 
Incoming requests, like "GET /", are being handled with 200 OK, showing the pod is serving traffic. Overall, nginx pod in Minikube is healthy and reachable.

---

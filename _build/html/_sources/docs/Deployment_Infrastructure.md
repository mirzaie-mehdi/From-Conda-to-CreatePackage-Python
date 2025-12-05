# Working with Virtual Machines, SSH and Remote MLflow Infrastructure


This chapter introduces the practical foundations needed to work with remote computing environments commonly used in Machine Learning and MLOps workflows. You will learn what a **Virtual Machine (VM)** is, how to connect to it using **SSH**, how to securely expose remote services using **port forwarding**, and finally how to connect your local ML code to a **remote MLflow server** for experiment tracking.

---

## Table of Contents
- [What is a Virtual Machine?](#vm)
- [Why Data Scientists Use VMs](#whyvm)
- [How to Build a Virtual Machine](#VM)
- [Setting Up SSH Access](#ssh)
- [Using SSH Port Forwarding](#pf)
- [Connecting to Remote MLflow](#mlflow)
- [Full Workflow Summary](#summary)
- [Troubleshooting](#ts)

---

## 1. What is a Virtual Machine?
<a name="vm"></a>

A **Virtual Machine (VM)** is a fully isolated computer environment running inside a physical machine. It behaves like a real computer—with its own CPU, RAM, storage, operating system, and network interface.

### You can think of a VM as:

#### **1️⃣ A remote computer you log into**
A Virtual Machine behaves just like a physical computer, but it exists *somewhere else*—in a datacenter or in the cloud. It has its own:

- Operating system (often Ubuntu/Linux)
- CPU cores  
- RAM  
- Disk storage  
- Network interface  

You connect to it using **SSH (Secure Shell)**, which means:

- You can work on the VM from anywhere  
- Your laptop becomes only a “window” into the VM  
- Training jobs continue running even if you close your laptop  

A VM is essentially your **remote workstation** for ML.

---

#### **2️⃣ A controlled and reproducible computing environment**
A laptop changes constantly (OS updates, Python conflicts, limited resources). A VM is different: it stays **stable and consistent**.
A VM provides:

- A fixed Python environment  
- Stable versions of CUDA, drivers, and libraries  
- Shared datasets accessible to multiple users  
- No unexpected system updates  
- An isolated environment that does not depend on your laptop  

This makes it ideal for **reproducible experiments**, a key MLOps requirement. If your code runs today on the VM, it will run the same way tomorrow, because nothing changes unless *you* change it.

---

#### **3️⃣ A place to run ML workloads without relying on your laptop**
Laptops have limitations:

- Limited RAM  
- No or limited GPU  
- Limited CPU power  
- Overheating  
- Battery draining  
- Sleep mode interrupts long training runs  

A VM solves these problems:

- Can have large amounts of RAM  
- Can include powerful CPUs or multiple GPUs  
- Runs **24h** without interruption  
- Can handle big datasets and heavy training workloads  
- Supports multiple notebooks/scripts running at once  
- Can host ML services such as MLflow, MinIO, FastAPI, or databases  

A VM becomes your **primary compute engine**, while your laptop becomes simply a **client**.

---

::::{admonition} ⭐ Summary
A Virtual Machine is:

> **A powerful, stable, always-on remote computer used to run machine learning tasks, experiments, and services without depending on your laptop’s hardware.**

VMs are fundamental to modern ML engineering and MLOps.
::::


---

### Why Data Scientists Use VMs
<a name="whyvm"></a>

Machine Learning workloads often require:

- **More compute power** than your laptop can provide  
- **Long-running jobs** that must survive reboot or disconnection  
- **Shared experiment tracking**, such as MLflow  
- **Persistent storage** for datasets and model artifacts  
- **Team-wide access** to the same environment  

A VM provides all of this in a controlled way.

:::{note}
Even if you develop code locally, the *infrastructure* for experiment tracking and deployment often lives on remote VMs.
:::

---


## 2. How to Build a Virtual Machine (VM)

Creating a Virtual Machine is one of the first steps toward building a stable and reproducible ML infrastructure. While the exact steps differ slightly between cloud providers (Azure, AWS, GCP), the process is conceptually the same everywhere. This section explains **how to build a VM in a general, cloud-agnostic way**, and also highlights the key parameters you must choose for ML workloads.

---

### 1️⃣ Choose a Cloud Platform or Host Environment
You can create a VM in:

- **Azure** (Azure Portal → Virtual Machines)
- **AWS EC2**
- **Google Cloud Compute Engine**
- **Local virtualization tools** (VirtualBox, VMware)
- **University/enterprise clusters** (custom portals)
- **Bare-metal servers managed via SSH**

Regardless of the platform, the VM creation workflow follows the same steps.

---

### 2️⃣ Select the Operating System (OS)

For Machine Learning, the recommended OS is:

- **Ubuntu 20.04 LTS**  
- **Ubuntu 22.04 LTS** (very common)  
- (Optional) Ubuntu + NVIDIA GPU drivers (if using GPUs)

Why Ubuntu?

- Compatibility with ML frameworks  
- Easy package management (`apt`, `pip`, `conda`)  
- Good community support  
- Most MLOps tools assume Linux environments  

---

### 3️⃣ Choose VM Size (CPU, RAM, GPU)

Depending on your ML workload:

#### **For lightweight models (e.g., scikit-learn):**
- 2–4 vCPUs  
- 8–16 GB RAM  
- No GPU needed  

#### **For deep learning (PyTorch/TensorFlow):**
- 8+ vCPUs  
- 32+ GB RAM  
- One or more GPUs (NVIDIA Tesla series)  
- Dedicated disk for datasets  

#### **For MLOps infrastructure (MLflow, MinIO, FastAPI services):**
- 2–4 vCPUs  
- 8 GB RAM  
- Standard SSD storage  

:::{note}
You do *not* need GPU for MLflow, databases, or MinIO. GPUs are needed only for model training or inference workloads.
:::

---

### 4️⃣ Configure Networking & Access

Every VM needs:

- A **public IP address** (for SSH access)
- A security rule (firewall) allowing **SSH on port 22**
- (Optional) private networking if multiple VMs talk to each other

For security:

- Disable password login  
- Use SSH keys only  
- Do not expose MLflow or MinIO to the internet  
- Use port forwarding instead  

---

### 5️⃣ Add Your SSH Key

During VM creation, the portal will ask for an SSH key. Paste the content of your local public key:

```sh
cat ~/.ssh/id_rsa.pub
```
---

### 6️⃣ Create the VM and Connect to It

Once the VM is created, connect from your local machine:

```sh
ssh <username>@<VM_PUBLIC_IP>
```

For example:

```sh
ssh student@20.73.41.122
```

If you have an SSH config entry:

```sh
ssh mlops-vm
```
---

### 7️⃣ Install Essential ML Infrastructure Components

After connecting to the VM, you typically install:

System updates:
```sh
sudo apt update && sudo apt upgrade -y
```

Python tools:
```sh
sudo apt install python3-pip python3-venv -y
```

Git:
```sh
sudo apt install git -y
```

MLflow + libraries (inside a venv):

```sh
python3 -m venv mlflow_env
source mlflow_env/bin/activate
pip install mlflow boto3 psycopg2-binary
```

Optional — GPUs:

Install NVIDIA drivers + CUDA toolkit depending on cloud platform.

---

### 8️⃣ Enable the VM for MLOps Work

A fully configured ML VM typically includes:

MLflow tracking server

Object storage (MinIO or cloud buckets)

PostgreSQL / MySQL (MLflow metadata)

SSH port forwarding for UI access

Docker (for deployment workloads)

Kubernetes tooling (kubectl + Azure CLI if using AKS)

This transforms your VM from a plain Linux box into a full ML platform.

::::{admonition} ⭐ Summary
A Virtual Machine (VM) provides a reproducible, stable, and always-on environment for Machine Learning and MLOps.  
To build a practical ML-ready VM:

1. **Choose a cloud or host platform** (Azure, AWS, GCP, on-premise)
2. **Select Ubuntu** as the operating system (20.04 or 22.04 LTS recommended)
3. **Allocate compute resources**  
   - CPUs: 2–8  
   - RAM: 8–32 GB  
   - GPU: optional (NVIDIA)
4. **Enable secure SSH access** using key-based authentication
5. **Create the VM** and connect via `ssh`
6. **Install essential tools**  
   - Python, pip/conda  
   - MLflow  
   - Git  
   - Storage clients  
7. **Configure networking** and SSH port forwarding for MLflow/MinIO
8. **Add Docker & Kubernetes** tooling for production deployments

These steps give you a powerful, reusable infrastructure to run ML experiments, track them, store artifacts, and deploy production ML services.
::::


---

## 3. SSH Basics
<a name="ssh"></a>

**SSH (Secure Shell)** is a protocol used to securely log in to a remote machine (like a VM) and run commands.

### 3.1 Generate SSH key pair (locally)

On your laptop:

```sh
ssh-keygen -t rsa -b 4096
```

This creates:

A private key (e.g., ~/.ssh/id_rsa) → keep this secret

A public key (e.g., ~/.ssh/id_rsa.pub) → you share this with servers you trust


### 3.2 Why are there two keys?

SSH uses **asymmetric cryptography**, which requires a matched pair of keys:

#### 🔐 Private Key (your secret identity)

- Stored only on your laptop

- Must never be shared

- Used to prove your identity when connecting

- If someone obtains this file, they can log in as you

:::{warning}

Think of it **like your passport** it uniquely identifies you.
:::

#### 🔓 Public Key (safe to share)

- You upload this to the server (VM)

- It is not sensitive

- The server uses it to verify that your private key is real

- Useless without the private key

:::{warning}

Think of it like the **lock to your VM**. Your private key is the only key that can open it.
:::

### 3.3 How SSH authentication works (simple explanation)

You run:
```sh
ssh <user>@<vm-ip>
```

The VM checks whether your public key is in:

```sh
~/.ssh/authorized_keys
```

If found, the server sends a cryptographic challenge. Your laptop solves it using your private key. If the solution matches the public key, access is granted. No passwords are sent over the network. Your private key never leaves your machine.

### 3.4 Why SSH keys are more secure than passwords

- Impossible to guess or brute-force (4096-bit RSA is extremely strong)

- No secrets are transmitted

- No password typing → avoids keyloggers

- Works automatically once set up

Required by most cloud providers (Azure, AWS, GCP)

::::{admonition} ⭐ Summary

| File                          | Location    | Purpose                | Share?  |
| ----------------------------- | ----------- | ---------------------- | ------- |
| **Private Key** (`id_rsa`)    | Laptop only | Proves your identity   | ❌ Never |
| **Public Key** (`id_rsa.pub`) | Given to VM | Verifies your identity | ✔ Safe  |

::::

SSH keys are the foundation of secure access to remote ML environments and production infrastructure.



### 3.5 Add your public key to the VM
On the VM (or via your cloud provider’s UI), add the content of your `id_rsa.pub` into:

```sh
~/.ssh/authorized_keys
```
Make sure permissions are correct:

```sh
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

### 3.6 Using an SSH config
Instead of typing long commands, create/edit:

```sh
~/.ssh/config
```

Example:

```sh
Host mlops-vm
    HostName <VM_PUBLIC_IP_OR_HOSTNAME>
    User <your-username>
    IdentityFile ~/.ssh/id_rsa
```

Now you can simply connect with:

```sh
ssh mlops-vm
```

:::{important}
If you get a `permissions are too open` warning, run:

```sh
chmod 600 ~/.ssh/id_rsa
```

:::

---

## 4. Port Forwarding for MLflow and MinIO
<a name="pf"></a>

Many ML tools run as **web applications** on the VM, for example:

| Service   | Typical Port | Description                           |
| --------- | ------------ | ------------------------------------- |
| MLflow    | 5000         | UI and tracking API                   |
| MinIO     | 9000         | Web UI for artifact storage           |
| MinIO API | 9001         | S3-compatible API endpoint (optional) |


These ports usually **are not reachable directly from your laptop** (for security reasons).
Instead, we use **SSH port forwarding** (also called tunneling).

### 4.1 Configure port forwarding in SSH config
Extend your `~/.ssh/config` entry:

```sh
Host mlops-vm
    HostName <VM_PUBLIC_IP_OR_HOSTNAME>
    User <your-username>
    IdentityFile ~/.ssh/id_rsa
    LocalForward 5000 127.0.0.1:5000
    LocalForward 9000 127.0.0.1:9000
    LocalForward 9001 127.0.0.1:9001
```

### 4.2 Connect with forwarding
Now connect:

```sh
ssh mlops-vm
```

As long as this SSH session is open, you can open on your laptop:

- MLflow → `http://127.0.0.1:5000`

- MinIO → `http://127.0.0.1:9000`

Even though these services are actually running on the VM.

:::{note}
This approach is much safer than exposing MLflow and MinIO directly to the internet. The ports stay private to your tunnel.
:::

---

## 5. Connecting Code to a Remote MLflow Server
<a name="mlflow"></a>

Once port forwarding is set up, you can connect your ML code **(running on your laptop or in the VM)** to the MLflow tracking server.

### 5.1 Basic configuration in Python

```sh
import mlflow

# Tracking server appears local because of SSH port forwarding
mlflow.set_tracking_uri("http://127.0.0.1:5000")

# Choose a meaningful experiment name
mlflow.set_experiment("wine_quality_experiment")
```

### 5.2 Logging runs, params, metrics, and models

```sh
import mlflow
import mlflow.sklearn
from sklearn.linear_model import ElasticNet
from sklearn.metrics import mean_squared_error
import numpy as np

# X_train, X_test, y_train, y_test assumed to be prepared already

with mlflow.start_run(run_name="elasticnet_default"):
    alpha = 0.5
    l1_ratio = 0.5

    model = ElasticNet(alpha=alpha, l1_ratio=l1_ratio, random_state=42)
    model.fit(X_train, y_train)

    preds = model.predict(X_test)
    rmse = np.sqrt(mean_squared_error(y_test, preds))

    mlflow.log_param("alpha", alpha)
    mlflow.log_param("l1_ratio", l1_ratio)
    mlflow.log_metric("rmse", float(rmse))

    mlflow.sklearn.log_model(model, artifact_path="model")

print("Run finished, check MLflow UI at http://127.0.0.1:5000")
```

After running this:

- Navigate to `http://127.0.0.1:5000` in your browser

- Open your experiment

- Inspect runs, parameters, metrics, and artifacts

:::{note}
When MLflow is configured with an object storage (e.g., MinIO or Azure Blob Storage), your logged models and artifacts are stored there, while metadata goes into the MLflow backend database.
:::

---

## 6. Building Production ML Services (Docker, AKS/Kubernetes)
<a name="deploy"></a>

So far, we used a VM and MLflow to **track experiments**.
In real projects, you often want to expose a **trained model as a service** (e.g., REST API) that other applications can call.

A common production stack:

- **MLflow** for model tracking and registry

- **Docker** to package your model and API

- **Kubernetes** (e.g., **Azure Kubernetes Service – AKS**) to run and scale containers

This section gives a high-level overview and a minimal end-to-end example.

### 6.1 High-level architecture

Conceptually:

```sh
MLflow (on VM or Azure ML)
   ▲
   │ (load model by URI or from registry)
   ▼
Docker container (FastAPI / Flask serving code)
   ▲
   │ (container image)
   ▼
Kubernetes / AKS (runs N replicas, exposes a Service)
```

The steps:

- Train and log model to MLflow

- Write a small **serving app** (e.g., FastAPI) that loads the model

- Create a **Docker image** for that app

- Push the image to a container registry

- Deploy the image on **Kubernetes/AKS** with a Deployment + Service

### 6.2 Example: Simple FastAPI inference service

Assume you have a model registered in MLflow, e.g.:

Registered name: `wine-quality-model`

Stage: `Production`

We will write an API that receives JSON and returns predictions.

```sh
# src/app.py
import mlflow.pyfunc
from fastapi import FastAPI
import pandas as pd

# MLflow model URI (from registry or direct run artifact)
MODEL_URI = "models:/wine-quality-model/Production"

app = FastAPI(title="Wine Quality Prediction API")

# Load the model once at startup
model = mlflow.pyfunc.load_model(MODEL_URI)

@app.get("/health")
def health():
    return {"status": "ok"}

@app.post("/predict")
def predict(features: dict):
    """
    Example request body:
    {
      "fixed acidity": 7.4,
      "volatile acidity": 0.70,
      "citric acid": 0.00,
      "residual sugar": 1.9,
      "chlorides": 0.076,
      "free sulfur dioxide": 11.0,
      "total sulfur dioxide": 34.0,
      "density": 0.9978,
      "pH": 3.51,
      "sulphates": 0.56,
      "alcohol": 9.4
    }
    """
    df = pd.DataFrame([features])
    preds = model.predict(df)
    return {"prediction": float(preds[0])}

```

You can test this locally (before Dockerizing) using `uvicorn`:

```sh
uvicorn src.app:app --host 0.0.0.0 --port 8000
```

Then open: `http://127.0.0.1:8000/docs` to test the API.

### 6.3 Dockerizing the service
Create a requirements.txt:

```sh
fastapi
uvicorn[standard]
mlflow
pandas
```

Create a `Dockerfile`:

```sh
# Dockerfile
FROM python:3.10-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY src/ ./src/

# Environment variables for MLflow tracking / artifacts (adapt for your setup)
ENV MLFLOW_TRACKING_URI=http://mlflow:5000

EXPOSE 8000

CMD ["uvicorn", "src.app:app", "--host", "0.0.0.0", "--port", "8000"]
```

Build the Docker image:

```sh
docker build -t wine-quality-api:latest .
```

Run locally:

```sh
docker run -p 8000:8000 wine-quality-api:latest
```

Check `http://127.0.0.1:8000/docs` again.

:::{warning}
In a real setup with MinIO, Azure Blob, or S3, you need to configure environment variables or credentials inside the container so that mlflow.pyfunc.load_model can access the model artifacts.
:::

### 6.4 Deploying the Docker image to Kubernetes / AKS
On a Kubernetes cluster (e.g., AKS), you:

1. Push the image to a registry (e.g., Azure Container Registry):

```sh
docker tag wine-quality-api <your-registry>/wine-quality-api:latest
docker push <your-registry>/wine-quality-api:latest
```

Create a `Deployment` and `Service manifest`.

Example `deployment-wine-api.yaml`:

```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: wine-quality-api
  labels:
    app: wine-quality-api
spec:
  replicas: 2
  selector:
    matchLabels:
      app: wine-quality-api
  template:
    metadata:
      labels:
        app: wine-quality-api
    spec:
      containers:
        - name: wine-quality-api
          image: <your-registry>/wine-quality-api:latest
          ports:
            - containerPort: 8000
          env:
            - name: MLFLOW_TRACKING_URI
              value: "http://mlflow:5000"
            # add any storage credentials here if needed
---
apiVersion: v1
kind: Service
metadata:
  name: wine-quality-api-service
spec:
  type: LoadBalancer
  selector:
    app: wine-quality-api
  ports:
    - port: 80
      targetPort: 8000
```

Apply it:

```sh
kubectl apply -f deployment-wine-api.yaml
```

On AKS or another managed Kubernetes:

- The `Service` of type `LoadBalancer` will be given an external IP

- You can then call:

```sh
http://<EXTERNAL-IP>/health
http://<EXTERNAL-IP>/predict
```

from any client that can access the cluster.

:::{note}
In real production setups, you will usually:

- use HTTPS (TLS),

- add authentication/authorization,

- configure autoscaling and monitoring,

- integrate with secrets managers (for credentials).
:::

## 7. Troubleshooting
<a name="ts"></a>

**Q1: I cannot access MLflow at** `http://127.0.0.1:5000`.

- Check that your SSH session is active and uses `LocalForward 5000` `127.0.0.1:5000`.

- Ensure MLflow is actually running on the VM (check with `ps aux | grep mlflow` or `netstat -tulpn`).

**Q2: My Docker container cannot load the MLflow model.**

Confirm that `MLFLOW_TRACKING_URI` inside the container points to a reachable MLflow server.

If using remote artifact storage (MinIO, S3, Azure Blob), ensure the correct environment variables and credentials are configured inside the container.

**Q3: Kubernetes Service has no external IP.**

- On some local clusters (e.g., kind, minikube), `LoadBalancer` is emulated or not supported by default.

- On AKS, wait a bit for the LoadBalancer to be provisioned and check with:

```sh

kubectl get svc wine-quality-api-service
```

**Q4: My model works locally but not in Kubernetes.**

- Check logs:

```sh
kubectl logs deployment/wine-quality-api
```

- Common issues: wrong model URI, missing environment variables, missing Python dependencies.





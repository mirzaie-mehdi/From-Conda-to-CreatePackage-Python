# SSH Basics


Secure Shell (SSH) is a foundational tool for working with remote servers, cloud virtual machines, version control systems, deployment pipelines, and MLOps workflows.
This chapter provides a practical introduction to SSH that you can reuse across all environments: local, remote, cloud, development, and production.

---

## Table of Contents

- [What Is SSH and Why Do We Use It?](#ssh)
- [How SSH Authentication Works](#howssh)
- [Generating SSH Keys](#genssh)
- [Connecting to Remote Machines](#connssh)
- [Configuring ~/.ssh/config](#confssh)
- [Copying Files via SSH](#copssh)
- [SSH Port Forwarding](#sshport)
- [SSH Agent Usage](#sshagent)
- [Cloud-Specific SSH Examples](#sshcloud)
- [Troubleshooting SSH](#troubssh)
- [Summary](#summary)
---

## 1. What Is SSH and Why Do We Use It?
<a name="ssh"></a>

SSH (Secure Shell) provides:

- 🔐 Secure, encrypted communication

- 👤 Strong authentication using key pairs

- 💻 Remote command execution

- 📁 Secure file transfer (scp, rsync)

- 🔌 Port forwarding for tunneling services 

SSH is used in:

- Connecting to cloud VMs (Azure, AWS, GCP) 
- Accessing remote Jupyter or MLflow 
- Managing Linux servers
- Deploying applications, DevOps workflows, MLOps tracking servers (e.g., MLflow)
- Using Git with SSH keys 
- Transfer files onto the server 


### How SSH Authentication Works
<a name="howssh"></a>

SSH relies on a **public/private key pair.**


```sh
Local Machine                                     Remote Server
--------------                               -------------------------
[ Private Key ] -- authentication check --> [ Public Key in ~/.ssh/authorized_keys ]
```

```sh
              ┌─────────────────────┐
              │     Local Machine   │
              │  (Your Laptop/PC)   │
              └──────────┬──────────┘
                         │
                         │  Uses private key for authentication
                         │
                 ┌───────▼────────┐
                 │  Private Key    │
                 │  id_ed25519     │
                 └───────┬────────┘
                         │
                         │  SSH Connection Request
                         ▼
        =================================================================
                         ▲
                         │
                         │  Server checks if private key matches public key
                         │
                 ┌───────┴────────┐
                 │ Public Key      │
                 │ authorized_keys │
                 └───────┬────────┘
                         │
              ┌──────────▼──────────┐
              │    Remote Server     │
              │ (Cloud VM / Linux)   │
              └──────────────────────┘
```

### Generate SSH key pair (locally)

On your laptop:

```sh
ssh-keygen -t rsa -b 4096
```

This creates:

A private key (e.g., ~/.ssh/id_rsa) → keep this secret

A public key (e.g., ~/.ssh/id_rsa.pub) → you share this with servers you trust



:::{note}

Recommended modern approach: **Ed25519**

```sh
ssh-keygen -t ed25519 -C "your_email@example.com"
```

You will get:
- `~/.ssh/id_ed25519` (private key)
- `~/.ssh/id_ed25519.pub` (public key)

:::



###  Why are there two keys?

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

###  How SSH authentication works (simple explanation)

You run:
```sh
ssh <user>@<vm-ip>
```

The VM checks whether your public key is in:

```sh
~/.ssh/authorized_keys
```

If found, the server sends a cryptographic challenge. Your laptop solves it using your private key. If the solution matches the public key, access is granted. No passwords are sent over the network. Your private key never leaves your machine.

### Why SSH keys are more secure than passwords

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

### Add your public key to a server

- **Option A:** Using `ssh-copy-id` **(Simple)**

```sh
ssh-copy-id user@server

```
-  **Option B:** Manual method

```sh
cat ~/.ssh/id_ed25519.pub | ssh user@server "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"

OR
cat ~/.ssh/id_rsa.pub | ssh user@server "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"


```

### Connect to Remote Machines

Basic syntax:

```sh
ssh user@hostname

```

Examples:


```sh
ssh ubuntu@51.120.14.33
ssh ec2-user@ec2-3-90-1-20.compute-1.amazonaws.com
ssh myuser@35.199.200.10

```
**First connection fingerprint prompt**

```sh
The authenticity of host '51.120.14.33' can't be established.
Are you sure you want to continue connecting (yes/no)?

```

Type **yes** to store the fingerprint and continue.


### Configuring `~/.ssh/config`

Create shortcuts for frequently used servers.

`~/.ssh/config`: the config file includes:


```sh
Host myvm
    HostName 20.55.130.12
    User ubuntu
    IdentityFile ~/.ssh/id_ed25519

```

Now connect with:

```sh
ssh myvm

```

#### **Diagram: SSH config expansion**

```sh
ssh myvm
   │
   ├── expands to
   │       ssh ubuntu@20.55.130.12 -i ~/.ssh/id_ed25519
   ▼
Connected


```

### Copying Files via SSH

Using **scp** a simple way to copy (upload) a file to a server:

```sh
scp file.txt user@host:/path/

```
You can also **download**:

```sh
scp user@host:/path/file.txt .

```

Using `rsync` ***(efficient for folders)*

```sh
rsync -avz -e ssh myfolder/ user@host:/remote/myfolder/

```

### SSH Port Forwarding

Port forwarding allows you to securely access remote services (MLflow, Jupyter, FastAPI) through your local browser. In the other word, SSH port forwarding allows you to securely “tunnel” a network port from one machine to another. This is very common in MLOps work because many tools expose **web interfaces** on ports:

- MLflow → 5000

- Jupyter Notebook → 8888

- FastAPI → 8000

- TensorBoard → 6006

These services run on a **remote VM**, but you want to open them in **your local browser**.
SSH forwarding makes this possible **without exposing the service to the internet**.

#### SSH Local Port Forwarding

Local port forwarding maps:

```sh
Your Laptop (localhost:PORT_A = 5000)
        ↓
Remote Server (localhost:PORT_B = 5000)

```

It makes a remote service appear as if it is running on your laptop.


**Command**


```sh
ssh -L 5000:localhost:5000 user@server
```


```sh
┌──────────────────┐         SSH Tunnel        ┌────────────────────┐
│  Your Laptop      │  5000 → localhost:5000  │   Remote Server     │
│ http://localhost:5000  ───────────────────▶ │ MLflow at 5000      │
└──────────────────┘                          └────────────────────┘

```


Use case examples:

- Access **MLflow UI** running on a VM on port 5000
- View and access remote **Jupyter Notebook** running on a VM
- Use **FastAPI** backend running remotely
- Connect to remote APIs without exposing them online

**Practical example**

If a workshop server runs MLflow at:

```sh
127.0.0.1:5000

```
You can open your browser locally at:

```sh
You can open your browser locally at:

```

and it shows the MLflow UI from the server.


#### SSH Remote port forwarding

Remote port forwarding is the opposite of local port forwarding:

```sh
Remote Server (localhost:PORT_A)
        ↓
Your Laptop (localhost:PORT_B)

```

It allows the **remote machine** to access a service running on **your laptop**.

**Command**


```sh
ssh -R 6000:localhost:6000 user@server
```

**Use Cases**

Remote port forwarding is useful when you want the **remote server** to access:

- a local development server

- a Jupyter instance running on your laptop

- a FastAPI app you are debugging locally

Example:

A training machine needs access to a local app you are developing.
Remote forwarding lets the server reach your laptop.


### SSH Agent Usage

To avoid repeated passphrase prompts:


Start agent:


```sh
eval "$(ssh-agent -s)"
```

Add your key:


```sh
ssh-add ~/.ssh/id_ed25519
```

List loaded keys:


```sh
ssh-add -l

```

### Cloud-Specific SSH Examples

This section gives examples for Azure, AWS EC2, and Google Cloud.

#### Azure VM

Public IP example: `20.75.44.12`

##### SSH into Azure VM

```sh
ssh azureuser@20.75.44.12

```
##### Using a config entry

```sh
Host azure-ml-vm
    HostName 20.75.44.12
    User azureuser
    IdentityFile ~/.ssh/id_ed25519

```

#### AWS EC2

AWS often uses the username `ec2-user` and `.pem` keys.

```sh
ssh -i ~/.ssh/mykey.pem ec2-user@ec2-3-80-155-100.compute-1.amazonaws.com

```

Recommended to convert `.pem` to OpenSSH format:


```sh
ssh-keygen -p -m PEM -f mykey.pem

```

#### GCP Compute Engine

Typical username: your local machine username.

```sh
ssh myname@34.122.110.10

```

Or use the `gcloud` command (auto-manages keys):


```sh
gcloud compute ssh myvm --zone=us-central1-a

```

### Troubleshooting SSH

#### **Fix permissions (most common issue)**

```sh
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519

```

#### Remove old host fingerprint


```sh
ssh-keygen -R IP_or_hostname

```

#### Specify key manually


```sh
ssh -i ~/.ssh/id_ed25519 user@server

```

#### Connection refused / timeout

Common causes:

- Firewall blocks port 22

- VM not running

- Wrong username

- Public key not installed on server

### Summary

SSH is a core tool for cloud computing, MLOps, DevOps, and deployment workflows.
By mastering SSH:

You can access any remote machine safely

- Securely transfer files and datasets

- Run remote services (MLflow, Jupyter, APIs)

- Use tunneling to avoid exposing ports to the internet

- Integrate with Azure, AWS, and GCP environments

- Maintain strong security practices


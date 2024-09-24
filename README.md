
Ansible is a free and open-source automation tool used to configuration management, application deployment and task automation. Ansible has a simple yet powerful architecture primarily consisting of **two types of nodes**:

- **Control Node:** This is the machine where Ansible is installed, and from which commands, modules, and playbooks are executed. 
- **Managed Nodes:** These are the target systems (servers, VMs, etc.) on which Ansible performs tasks. They do not require Ansible to be installed; they are managed through SSH (Linux) or WinRM (Windows).

Ansible operates in an **agentless fashion**, which means it does not need agents to be running on managed nodes. **It uses SSH for Linux and WinRM for Windows to communicate with managed nodes.**


### Pre-requisites | Environment


|      Role       |         FQDN                  |       IP       |     OS       |
|-----------------|-------------------------------|----------------|--------------|
| Control Node    | ansible-controller.saiful.com | 192.168.3.10   | Ubuntu 24.04 |
| Managed Node1   | web1.saiful.com               | 192.168.3.11   | Ubuntu 24.04 |
| Managed Node1   | web2.saiful.com               | 192.168.3.12   | Ubuntu 22.04 |
| Managed Node+n  | xyz                           | xyz            | xyz          |


### Step:1: Update hostfile `/etc/hosts`(Only control node)

 - Downlaod the hostfile and hostname updating script 
```sh
curl -O https://github.com/saifulislam88/ubuntu-essentials-package-installing-manager/blob/main/hostconfig.sh
chmod +x hostconfig.sh
```
 - **Run** the `./hostconfig.sh` script or **add manually** `vim /etc/hosts`

```sh
192.168.3.10   ansible-controller.saiful.com       ansible-controller
192.168.3.11   web1.saiful.com                     web1
192.168.3.12   web1.saiful.com                     web1
```

### **Step:2:** **Ansible Tasks Operation User**
- Create a user **'msi'** or who will execute to **perform Ansible tasks** on **both** the `control` and `managed` node and give the **`sudo permissions`**.

```sh
adduser --disabled-password --gecos "" msi && echo "msi:nopassword" | chpasswd && usermod -aG sudo msi
```

- **Set up SSH keys** | Configure `password less authentication` for the created user`saiful` from **`control node`** to **`managed nodes`**. For that, generate an SSH key pair and copy it to the managed nodes using the following commands.

```sh
su msi
ssh-keygen -t rsa -b 4096
```
```sh
ssh-copy-id msi@web1.saiful.com
ssh-copy-id msi@web2.saiful.com 
```
- Test the managed server access
```sh
ssh msi@web1.saiful.com
```
```sh
ssh msi@web2.saiful.com 
```



Step 1: Verify sudo privileges and update system packages.
Step 2: Add Ansible PPA (Personal Package Archive)
Step 3: Install Ansible and verify the installation.
Step 4: Host machine setup.
Step 5: Set up SSH keys in host machines.
Step 6: Test the host machine access.

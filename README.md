
Ansible is a free and open-source automation tool used to configuration management, application deployment and task automation. Ansible has a simple yet powerful architecture primarily consisting of **two types of nodes**:

**Control Node:** This is the machine where Ansible is installed, and from which commands, modules, and playbooks are executed. 
**Managed Nodes:** These are the target systems (servers, VMs, etc.) on which Ansible performs tasks. They do not require Ansible to be installed; they are managed through SSH (Linux) or WinRM (Windows).

Ansible operates in an **agentless fashion**, which means it does not need agents to be running on managed nodes. **It uses SSH for Linux and WinRM for Windows to communicate with managed nodes.**



### Pre-requisites | Environment

- Standard

|      Role       |         FQDN                  |       IP       |     OS       |
|-----------------|-------------------------------|----------------|--------------|
| Control Node    | ansible-controller.saiful.com | 192.168.3.10   | Ubuntu 24.04 |
| Managed Node1   | web1.saiful.com               | 192.168.3.11   | Ubuntu 24.04 |
| Managed Node1   | web2.saiful.com               | 192.168.3.12   | Ubuntu 22.04 |
| Managed Node+n  | xyz                           | xyz            | xyz          |

- **Ansible Control Node** – `ansible-controller.saiful.com`
- **Ansible Managed Nodes** – `node1.saiful.com` & `node2.saiful.com`
- **Ansible Tasks User** - Create a user **'saiful'** to **perform Ansible tasks** on both the `control` and `managed` node and give the **`sudo permissions`**.





Step 1: Verify sudo privileges and update system packages.
Step 2: Add Ansible PPA (Personal Package Archive)
Step 3: Install Ansible and verify the installation.
Step 4: Host machine setup.
Step 5: Set up SSH keys in host machines.
Step 6: Test the host machine access.

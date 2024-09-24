### Ansible Architecture Overview:

Ansible is a free and open-source automation tool used to configuration management, application deployment and task automation. Ansible has a simple yet powerful architecture primarily consisting of **two types of nodes**:

- **Control Node:** This is the machine where Ansible is installed, and from which commands, modules, and playbooks are executed. 
- **Managed Nodes:** These are the target systems (servers, VMs, etc.) on which Ansible performs tasks. They do not require Ansible to be installed; they are managed through SSH (Linux) or WinRM (Windows).

Ansible operates in an **agentless fashion**, which means it does not need agents to be running on managed nodes. **It uses SSH for Linux and WinRM for Windows to communicate with managed nodes.**


### Ansible Core Components:
- **Inventory:** A file listing the hosts or nodes to manage, typically `/etc/ansible/hosts` or a `custom path with **any name** like `/home/msi/hostdb`.
- **Modules:** Predefined code to perform tasks (e.g., file management, service restart, package installation).
- **Playbooks:** YAML files containing a set of tasks to automate processes.
- **Roles:** A structure for organizing playbooks and other configuration files for reusability.
- **Plugins:** Extend the core functionality of Ansible (e.g., inventory plugins, connection plugins).


### Pre-requisites | Environment | Installation


|      Role       |         FQDN                  |       IP       |     OS       |
|-----------------|-------------------------------|----------------|--------------|
| Control Node    | ansible-controller.saiful.com | 192.168.3.10   | Ubuntu 24.04 |
| Managed Node1   | web1.saiful.com               | 192.168.3.11   | Ubuntu 24.04 |
| Managed Node2   | web2.saiful.com               | 192.168.3.12   | Ubuntu 22.04 |
| Managed Node+n  | xyz                           | xyz            | xyz          |


### Step:1: Update hostfile `/etc/hosts`(Only control node)

🔴 Downlaod the hostfile and hostname updating script 
```sh
curl -O https://github.com/saifulislam88/ubuntu-essentials-package-installing-manager/blob/main/hostconfig.sh
chmod +x hostconfig.sh
```
🔴 **Run** the `./hostconfig.sh` script or **add manually** `vim /etc/hosts`

```sh
192.168.3.10   ansible-controller.saiful.com       ansible-controller
192.168.3.11   web1.saiful.com                     web1
192.168.3.12   web1.saiful.com                     web1
```

### **Step:2:** **Ansible Tasks Operation User**
🟡 Create a user **'msi'** or who will execute to **perform Ansible tasks** on **both** the `control` and `managed` node and give the **`sudo permissions`**.
```sh
adduser --disabled-password --gecos "" msi && echo "msi:nopassword" | chpasswd && usermod -aG sudo msi
```

🟡 **Set up SSH keys** | Configure `password less authentication` for the created user`saiful` from **`control node`** to **`managed nodes`**. For that, generate an SSH key pair and copy it to the managed nodes using the following commands.
```sh
su msi
ssh-keygen -t rsa -b 4096
```
```sh
ssh-copy-id msi@web1.saiful.com
ssh-copy-id msi@web2.saiful.com 
```
- Test the managed server access

`ssh msi@web1.saiful.com `
`ssh msi@web2.saiful.com `


### **Step:3:** To install **Ansible** on your server, run the following command on your **control node**:

🟢 **Ansible Installation**
```sh
sudo apt update -y
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible -y
```
```sh
sudo ansible --version
```



After installing Ansible, you can manage remote nodes using the default configuration provided in /etc/ansible, as well as create custom configurations in other locations such as a user's home directory. By default, `Ansible` is configured to use the `/etc/ansible` directory for its configuration, `inventory`, and `playbook files`. When running `Ansible` as the **`root`** user, `Ansible` will look for the following default files:

🟢 Default Configuration in /etc/ansible


- **Ansible Configuration File:** `/etc/ansible/ansible.cfg`
- **Inventory File:** `/etc/ansible/hosts`
- **Playbooks and Roles:** Can be created and stored in any directory, but the default inventory and configuration are tied to `/etc/ansible/adduser.yaml`.

This setup allows centralized management of nodes by the root user or Keep as it is after ansible installation\

**`Ansible Default Configuration File`**\
`vim /etc/ansible/ansible.cfg`
```sh
[defaults]
inventory = /etc/ansible/hosts
remote_user = root
host_key_checking = False

[privilege_escalation]
become=True
become_method=sudo
become_user=root
become_ask_pass=False
```

**`Inventory File`**\
`vim /etc/ansible/hosts`
```sh
[dev]
192.168.3.11

[prod]
web1.saiful
```

**`Playbooks and Roles`**\
`vim /etc/ansible/adduser.yaml`

```sh
---
- hosts: all
  become: yes
  tasks:
    - name: Add the user 'arham' with a bash shell and append to the appropriate group based on OS
      ansible.builtin.user:
        name: arham
        shell: /bin/bash
        groups: "{{ 'sudo' if ansible_facts['os_family'] == 'Debian' else 'wheel' }}"
        append: yes
        password: "$6$b83Y/9DUqDUcOUWa$2/Ck6fbUZzf/IcycdRvR6grGkKTg0Xr9D9RReFWK7kKctK3mva5r6a7CImZ3VMLdaJ2TS7fkBeYnduKge8O55/"   ## mkpasswd --method=SHA-512 | Where password is "nopassword"
```


Step 1: Verify sudo privileges and update system packages.
Step 2: Add Ansible PPA (Personal Package Archive)
Step 3: Install Ansible and verify the installation.
Step 4: Host machine setup.
Step 5: Set up SSH keys in host machines.
Step 6: Test the host machine access.

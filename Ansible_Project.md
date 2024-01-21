# Ansible Project

![Ansible Architecture](/Diagram.png)

## What is Ansible?

- **Ansible** is a software tool that provides simple but `powerful automation` for **cross-platform computer support**. It is primarily intended for **IT professionals**, who use it for **application deployment**, **updates on workstations and servers**, **cloud provisioning**, **configuration management**, **intra-service orchestration**, and nearly anything a systems administrator does on a weekly or daily basis.
- **Ansible** doesn't depend on agent software and has `no additional security infrastructure`, so it's **easy to deploy**.

## How Ansible Works?

- **Ansible** works by `connecting to nodes` (**clients**, **servers**, or whatever you're `configuring`) on a **network**, and then sending a small program called an `Ansible module` to that node. **Ansible** executes these modules over SSH and removes them when finished.
- **SSH keys** are the most common way to provide access, but other forms of `authentication` are also supported.

## Ansible Architecture: Nodes and Modules

- **Ansible's Architecture** is based on the concept of a **control node** and a **managed node**. The platform is executed from the control node where a user runs the `ansible-playbook` command. There must be at least one `control node`; a backup control node can also exist. The devices being automated and managed by the control node are known as **managed nodes**.
- **Ansible** automates `Linux` and `Windows` by connecting to managed **nodes** and pushing out small programs called `Ansible modules`. **Ansible** executes these `modules`, which are the resource models of the desired system state, over **Secure Socket Shell (SSH)** by default and `removes` them when finished.
- `Ansible` modules are written in **Python** and can be written in any language. `Ansible` modules are reusable, standalone scripts that can be used by the **Ansible API**, **Ansible Playbooks**, or **Ansible Galaxy**.

---

### For a more comprehensive version of this blog post, please refer to the previous entry where you can find a thorough and hands-on rephrasing of the content:

- https://devops-rohit.hashnode.dev/day-55-grasping-configuration-management-with-ansible

---

## Create **3 Instances** on `AWS EC2` with following names:

- _Ansible_Master_Server_
- _Node_1_
- _Node_2_

![Screenshot from 2023-08-26 22-37-15](https://github.com/Rohit312001/GitDemo/assets/76991475/8e3c8903-9605-472a-a7a3-d0234a594382)

### Installation of Ansible on AWS EC2 (Master Node)

```
#!/bin/bash
sudo apt-add-repository ppa:ansible/ansible
sudo apt update
sudo apt install ansible
```

```
sudo chmod 777 install.sh
sudo ./install.sh
```

![Screenshot from 2023-08-26 21-07-38](https://github.com/Rohit312001/GitDemo/assets/76991475/031154bb-821c-4422-a749-8891acc96192)

```
ansible --version
```

![Screenshot from 2023-08-26 22-03-06](https://github.com/Rohit312001/GitDemo/assets/76991475/c8328674-8620-4610-bb0e-ee0db712c3bd)

### Create ssh key on Master Node

```
cd ~/.ssh
ssh-keygen
```

![Screenshot from 2023-08-26 22-05-56](https://github.com/Rohit312001/GitDemo/assets/76991475/c8f07db7-3985-4239-b7d7-36d8ca4d229b)

### What is Hosts file?

- In the context of **Ansible**, a `host file` (also known as an **inventory file**) is a `configuration file` used to define and organize the list of **target hosts** that **Ansible should manage**.

### Where is Hosts file located?

- **Ansible** uses this file to map **target hosts** to **managed nodes**. The **hosts file** is usually located in `/etc/ansible/hosts`.

- So, open the **hosts** file and add the **IP addresses** of the **Nodes**:

```
sudo vim /etc/ansible/hosts
```

```
[servers]
Node_1 ansible_host= <Public IP-Adddress of Node-1>
Node_2 ansible_host= <Public IP-Adddress of Node-2>

[all:vars]
ansible_python_interpreter=/usr/bin/python3
ansible_ssh_private_key_file=/home/ubuntu/.ssh/id_rsa
```

![imageedit_5_3816895337](https://github.com/Rohit312001/GitDemo/assets/76991475/b55d1e80-a26b-4df8-8a5a-cb0ea83dd126)

---

### Now we will ping Node server using Master Node by pasting the public ssh key of Master Node to the Node server.

```
# Open Node_1
cd /home/ubuntu/.ssh
vim authorized_keys
```

![imageedit_9_3321018291](https://github.com/Rohit312001/GitDemo/assets/76991475/7a28a394-ad4a-46c1-9e95-a2369a42fec0)

```
# Open Node_2
cd /home/ubuntu/.ssh
vim authorized_keys
```

![imageedit_13_8868297131](https://github.com/Rohit312001/GitDemo/assets/76991475/49a5927f-0f51-4b76-9006-ba3d2431d19d)

### What is Ansible Playbook?

- **Ansible playbooks** run `multiple tasks`, `assign roles`, and `define configurations`, `deployment steps`, and `variables`. If you’re using **multiple servers**, Ansible playbooks organize the steps between the `assembled machines` or `servers` and get them organized and running in the way the users need them to. Consider playbooks as the equivalent of instruction manuals.

- It allows you to describe the desired state of your `systems` and `applications` in a **declarative manner**, and Ansible takes care of executing the `necessary tasks` to achieve that state.

- Ansible playbooks are written in **YAML**. YAML is a `human-readable data serialization language`. It is commonly used for configuration files and in applications where data is being `stored` or `transmitted`.

- **YAML** stands for **YAML Ain't Markup Language**. It is a human-readable data serialization language and commonly used for configuration files, but it can be used in many applications where data is being stored or transmitted.

### How to create an Ansible Playbook?

- An **Ansible playbook** is a `structured` and `human-readable document` written in **YAML** (YAML Ain't Markup Language) that defines a `set of tasks` and configurations to be performed on remote servers. **Playbooks are the heart of Ansible automation** and allow you to define the desired state of your systems and applications.

- **YAML Syntax:** `Playbooks` are written in YAML format, which is a `human-readable markup language`. YAML uses indentation and colons to structure `data hierarchically`. Indentation is crucial in YAML to define the `nesting levels of data`.

- **Play:** A `playbook` starts with a `list of plays`. Each play targets a **specific group of hosts** and defines a **set of tasks** to be executed on those **hosts**.

- **name:** A user-friendly name for the play.

- **hosts:** The group(s) of hosts this play will target. Host groups are defined in the inventory file.

- **become:** Indicates whether privilege escalation is required to perform tasks (using sudo).

- **tasks:** List of tasks to be executed in this play.

For Example;

```
---
- name: My First Playbook
  hosts: web_servers
  become: yes
  tasks:
    # Tasks will be defined here
    - name: Ensure Nginx is installed
    apt:
      name: nginx
      state: present
```

- **name:** A description of the task.

- **module:** The Ansible module to use for the task (e.g., apt, yum, file, copy, etc.).

- **module_arguments:** Arguments specific to the module being used.

### Create a playbook to install Nginx

- **Installing Nginx using Playbook:**

```
-
    name: Installing Nginx using Playbook
    hosts: all
    become: yes
    tasks:
      - name: Install Nginx
        apt:
          command: nginx -y
      - name: Start and enable Nginx
        service:
          name: nginx
          state: started
          enabled: yes
```

![Screenshot from 2023-09-01 12-39-03](https://github.com/Rohit312001/GitDemo/assets/76991475/fd3385fc-80ab-448f-8ae1-107f66d99331)

- **Output of the above playbook:**

![Screenshot from 2023-09-01 12-39-14](https://github.com/Rohit312001/GitDemo/assets/76991475/e7904b4d-000a-4c81-8e60-a89c11cca5db)
![Screenshot from 2023-09-01 12-39-35](https://github.com/Rohit312001/GitDemo/assets/76991475/4b3901e8-8697-4f7e-ba2a-5a54583964e6)

- **Checking the status of Nginx on one of the Node_server**

```
service nginx status
```

![Screenshot from 2023-09-01 12-40-24](https://github.com/Rohit312001/GitDemo/assets/76991475/270773dd-e33a-48da-8dcc-28e79f76d781)

```
nginx -v
```

![Screenshot from 2023-09-01 12-41-17](https://github.com/Rohit312001/GitDemo/assets/76991475/12406334-d354-4d85-a1e9-a72103e36ab2)

---

### Deploy a sample webpage using the ansible playbook.

```
cd playbook
# Put any index.html file in the playbook folder
sudo vim index.html
```

![Screenshot from 2023-09-01 13-43-08](https://github.com/Rohit312001/GitDemo/assets/76991475/a15c4597-b2a8-4667-94d4-dfa4d5fb71d7)

- Modify the playbook as follows:

```
-
    name: Installing Nginx using Playbook
    hosts: all
    become: yes
    tasks:
      - name: Install Nginx
        apt:
          command: nginx -y
      - name: Start and enable Nginx
        service:
          name: nginx
          state: started
          enabled: yes
      - name: Creating Webpage
        copy:
          src: index.html
          dest: /var/www/html/index.html
```

![Screenshot from 2023-09-01 13-15-19](https://github.com/Rohit312001/GitDemo/assets/76991475/1ee1c3fa-570a-426d-926d-e8af69c9fcbc)

- **Output of the above playbook:**

![Screenshot from 2023-09-01 13-15-56](https://github.com/Rohit312001/GitDemo/assets/76991475/d420d9c7-6ec3-47dc-b501-850937dd2c42)

- **Now we will use the public IP address of the Node server to access the webpage.**

### Node_1 Output:

![Screenshot from 2023-09-01 13-17-26](https://github.com/Rohit312001/GitDemo/assets/76991475/534336a9-1977-411e-b765-238aec019d69)

### Node_2 Output:

![Screenshot from 2023-09-01 13-17-31](https://github.com/Rohit312001/GitDemo/assets/76991475/3c07ea0d-2beb-491b-a53e-07a7781d9d52)

### Node_3 Output:

![Screenshot from 2023-09-01 13-17-37](https://github.com/Rohit312001/GitDemo/assets/76991475/e6db52b1-ea65-4e5b-8749-1510cec3643f)

---

**Thus we have successfully deployed a sample webpage using the ansible playbook.**

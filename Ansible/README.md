# Ansible
This is my own personal documentation of the common Ansible commands. \
As most OS are Linux in DevOps, this in the context of Linux (Ubuntu).

</br>

## Table of Contents:
> - [Setting Up Ansible](https://github.com/leeyawnz/DevSecOps/tree/main/Ansible#setting-up-ansible)
> - [Accessing a Remote Server with a Controller Server](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#accessing-a-remote-server-with-a-controller-server)
>   - [Using SSH-Copy-ID](https://github.com/leeyawnz/DevSecOps/tree/main/Ansible#using-ssh-copy-id)
>   - [Manually](https://github.com/leeyawnz/DevSecOps/tree/main/Ansible#manually)
> - [Ansible Inventory](https://github.com/leeyawnz/DevSecOps/tree/main/Ansible#ansible-inventory)
>   - [Inventory Group](https://github.com/leeyawnz/DevSecOps/tree/main/Ansible#inventory-groups)
>   - [Inventory Variables](https://github.com/leeyawnz/DevSecOps/tree/main/Ansible#inventory-variables)
> - [Ansible Module/Task/Play/Playbook](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#ansible-moduletaskplayplaybook)
>   - [Modules](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#modules)
>   - [Tasks](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#tasks)
>   - [Handlers](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#handlers)
>   - [Plays](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#plays)
>   - [Playbooks](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#playbooks)

</br>

## Setting Up Ansible
To set up Ansible, we can run the following commands
```
sudo apt update
```
```
sudo apt install software-properties-common
```
```
sudo add-apt-repository --yes --update ppa:ansible/ansible
```
```
sudo apt install ansible
```
Checking if Ansible has been installed
```
python
```
```
ansible --version
```
[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#table-of-contents)

</br>

## Accessing a Remote Server with a Controller Server
Taking advantage of SSH, we can access different servers remotely from a controller server. This is done manually or by using ssh-copy-id to make sure that all servers have the public key that was given by the controller server. This is key for Ansible as we can now configure remote servers using a single controller server.

</br>

### Using SSH-Copy-ID
Need to find out on how to do this

[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#table-of-contents)

</br>

### Manually
#### 1. Setting up our instances
First create 2 instances in AWS. After creating the 2 instances,run all 2 instances on 2 separate terminals.

Inside the first instance, rename it to controller
```
sudo hostnamectl set-hostname controller
```
Inside the second instance, rename it to server1
```
sudo hostnamectl set-hostname server1
```

#### 2. Checking for existing ssh key
Inside our controller instance, check if an ssh key is present already.
```
ls .ssh/
```
> If id_rsa and id_rsa.pub is present, there is an ssh key already present in the controller instance.
> 
> Else, you should only see a file named authorized_keys.

#### 3. Generating an ssh key
Since the controller is a new instance, it should not have any existing ssh key created so we need to generate an ssh key.

Inside the controller instance, we will generate our ssh key.
```
ssh-keygen
```
> For all the required entries, just leave them blank

Now, check if the id_rsa and id_rsa.pub files have been created
```
ls .ssh/
```
> Output: \
> authorized_keys id_rsa id_rsa.pub

#### 4. Copying ssh Public Key into server1
From the controller instance, copy the ssh key in the .ssh/id_rsa.pub file
```
cat .ssh/id_rsa.pub
```
Enter into your server1 instance and edit the authorized_keys file:
```
vi .ssh/authorized_keys
```
In this authorized_keys file, paste the public key and save it.

#### 5. Done!
We can now double check our access to server1 using the controller instance
```
ssh username@ip-address
```
> Do note that if the authorized_keys file belongs to another user, you can only access the remote server through that user. Generally, we will want to link root to root so that we can do configurations in the remote server. \
> \
> We can escape access from the remote server by exiting it
> ```
> exit
> ```

[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#table-of-contents)

</br>

## Ansible Inventory
In Ansible, there is a file called inventory. Inside the inventory file, we can add all the remote servers (including localhost) so that we can target them when we perform Ansible configuration management tasks.

We can create an inventory file easily.
```
vi inventory
```
> If there is no inventory file created and specified, Ansible will go to this file /etc/ansible/hosts and use it as a default inventory file.
### Inventory Groups
Inside this inventory file, we can group servers together.
```
ipaddress1
 
[web]
webserver1
webserver2

[db]
dbserver1
```
> From here, we can see that we have 3 server groups. By default, every group belong to the 'all' group

We can also have nested groups using ":childen" in the [ ].
```
[atlanta]
host1
host2

[raleigh]
host2
host3

[southeast:children]
atlanta
raleigh

[usa:children]
southeast
northeast
southwest
northwest
```

### Inventory Variables
We can assign variables to a server as well.
```
[atlanta]
host1 http_port=80 maxRequestsPerChild=808
host2 http_port=303 maxRequestsPerChild=909
```
If a group shares the same variables, we can group the variable together and attach them to the server group.
```
[atlanta]
host1
host2

[atlanta:vars]
ntp_server=ntp.atlanta.example.com
proxy=proxy.atlanta.example.com
```

### Inventory Types
#### 1. Static
Static inventory file contains ip addresses of hosts which are not changing. The addresses are explicitly written in this file.

#### 2. Dynamic
Dynamic inventory is the opposite of static, where the hosts addresses may change. For example, in AWS, whenever we restart an instance, the ip address changes.

[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#table-of-contents)

</br>

## Ansible Module/Task/Play/Playbook
### Modules
A module is a specifc action that you want to execute. Basic modules are:
- ping
- apt
- service
> A module will be injected into our remote servers and it will execute all the necessary tasks. After execution, this module will be deleted.

### Tasks
A task consists of all the parameters, including the module that you want to run that does one thing.

Below is an example of a task:
```
- name: Installing Apache2
  apt:
    name: apache2
    state: present
```
> We can give a task a name 'Download Apache2' as well to identify what each individual task does.

### Handlers
A handler is a special set of tasks where it will be called if a task successfully caused a change in the system.

Below is an example of a handler:
```
- name: Installing Apache2
  apt:
    name: apache2
    state: present
  notify:
    - Start and Enable Apache2

handlers:
- name: Start and Enable Apache2
  service:
    name: apache2
    state: started
    enabled: yes
```
> When successfully caused a change, using notify, the handler task will be invoked.

### Plays
A play consists of a set of tasks that will be run together.

Below is an example of a play with 2 tasks:
```
- name: Checking Connection
  ping: ~
- name: Installing Apache2
  apt:
    name: apache2
    state: present
```

### Playbooks
A playbook contains several plays. This playbook is a YAML file.

Below is an example of a playbook:
```
- hosts: all
  tasks:
    - name: Checking Connection
      ping: ~

- hosts: all
  tasks:
    - name: Installing Apache2
      apt:
        name: apache2
        state: present
      notify:
        - Start and Enable Apache2
        
handlers:
- name: Start and Enable Apache2
  service:
    name: apache2
    state: started
    enabled: yes
```
> So as we can see, Playbook > Plays > Tasks > Modules, where '>' means consists of.

To run a playbook, we just need to use the command
```
ansible-playbook playbook.yml
```
> As mentioned before, Ansible uses the default /etc/ansible/hosts file as inventory. We can specify the specific inventory file with a '-i' parameter
> ```
> ansible-playbook -i inventory playbook.yml
> ```

[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#table-of-contents)

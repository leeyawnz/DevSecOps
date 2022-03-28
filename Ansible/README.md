# Ansible
This is my own personal documentation of the common Ansible commands. \
As most OS are Linux in DevOps, this in the context of Linux (Ubuntu).

</br>

## Table of Contents:
> - [Setting Up Ansible](https://github.com/leeyawnz/DevSecOps/tree/main/Ansible#setting-up-ansible)
> - [Accessing a Remote Server with a Controller Server](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#accessing-a-remote-server-with-a-controller-server)
>   - [AWS Instance Creation](https://github.com/leeyawnz/DevSecOps/tree/main/Ansible#aws-instance-creation)
>   - [AWS Instance Creation from an AWS Instance Snapshot](https://github.com/leeyawnz/DevSecOps/tree/main/Ansible#aws-instance-creation-from-an-aws-instance-snapshot)
>   - [Manually](https://github.com/leeyawnz/DevSecOps/tree/main/Ansible#manually)
> - [Ansible Inventory](https://github.com/leeyawnz/DevSecOps/tree/main/Ansible#ansible-inventory)
>   - [Inventory Group](https://github.com/leeyawnz/DevSecOps/tree/main/Ansible#inventory-groups)
>   - [Inventory Variables](https://github.com/leeyawnz/DevSecOps/tree/main/Ansible#inventory-variables)
>   - [Inventory Types](https://github.com/leeyawnz/DevSecOps/tree/main/Ansible#inventory-types)
>   - [Additional Inventory-related Commands](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#additional-inventory-related-commands)
> - [Ansible Module/Task/Play/Playbook](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#ansible-moduletaskplayplaybook)
>   - [Modules](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#modules)
>   - [Tasks](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#tasks)
>   - [Handlers](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#handlers)
>   - [Plays](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#plays)
>   - [Playbooks](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#playbooks)
> - [Ansible Configuration](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#ansible-configuration)
> - [Ad-hoc Commands](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#ad-hoc-commands)
> - [Managing Variables](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#managing-variables)
>   - [Creating Variables](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#creating-variables)
>   - [Using Variables](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#using-variables)
>     - [1. Specifying on Command Line](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#1-specifying-on-command-line)
>     - [2. Specifying Inside Playbook](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#2-specifying-on-playbook)
>     - [3. Creating a Variable File](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#3-creating-a-variable-file)
>     - [4. From a Registered Variable](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#4-from-a-registered-variable)
>     - [5. Gathering Facts](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#5-gathering-facts)

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

### AWS Instance Creation
When creating an instance, we can go to Configure Instance Details (Step 3) and under the user data we can copy the id_rsa.pub key and paste it here in this section.
```
echo "<id_rsa.pub ssh key>" >> <specific path>/.ssh/authorized_keys
```
> Need to test this out as there seems to be some error. The authorized_keys file is created after the instance is created and therefore, we are unable to push this ssh-key into the file. Would need to double-check this again.
Once we launch the instances, we will have password-less access for those instances.

[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#table-of-contents)

</br>

### AWS Instance Creation from an AWS Instance Snapshot
We can take a snapshot, from AWS Snapshot tab, of an instance that has password-less ssh authentication in AWS and we can create multiple instances from the snapshot from that particular instance.

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

Have yet to try, can refer to this article [here](https://devopscube.com/setup-ansible-aws-dynamic-inventory/).

[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#table-of-contents)

### Additional Inventory-related Commands
Display all inventory details
```
ansible-inventory -i inventory --list
```
> Display specific group inventory 
> ```
> ansible-inventory -i inventory group --list
> ```


Display inventory hierarchy
```
ansible-inventory -i inventory --graph
```
> Display group hierarchy
> ```
> ansible-inventory -i inventory group --graph
> ```

Display inventory variables
```
ansible-inventory -i inventory --vars
```
> Display group variables
> ```
> ansible-inventory -i inventory group --vars
> ```

[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#table-of-contents)

</br>

## Ansible Module/Task/Play/Playbook
### Modules
A module is a specifc action that you want to execute. Basic modules are:
- debug
- ping
- apt
- service
- command
> A module will be injected into our remote servers and it will execute all the necessary tasks. After execution, this module will be deleted.

### Tasks
A task consists of all the parameters, including the module that you want to run that does one thing.

Below is an example of a task:
```
---
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
---
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
---
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
---
- hosts: all
  become: yes
  tasks:
    - name: Checking Connection
      ping: ~

- hosts: all
  become: yes
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
> Or we can specify the specific group as well
> ```
> ansible-playbook -i inventory web playbook.yml
> ```

For a simple playbook set-up and execution, we can create a folder where it contains the following files:
> ansible.cfg \
> inventory \
> playbook.yml

[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#table-of-contents)

</br>

## Ansible Configuration
Ansible searches for the first ansible.cfg file it can find and uses that configuration file. We can find more details in the ansible.cfg file.
```
vi /etc/ansible/ansible.cfg
```

Configuration file precedence (first to last):
- ANSIBLE_CONFIG (environment variable if set) 
- ansible.cfg (in current directory) 
- ../ansible.cfg (in home directory) 
- /etc/ansible/ansible.cfg

[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#table-of-contents)

</br>

## Ad-hoc Commands
Ad-hoc commands are useful when it comes to performing one-off tasks that makes small changes to configurations. If big configurations are required, we can always use playbooks or roles.

Example of an ad-hoc command:
```
ansible -i inventory all -m apt -a "name:httpd state:present"
```
> ```
> ansible -i <inventory> <inventory-group> -m <module> -a "<attributes>"
> ```
> Search for the specific module documentation as required. May require to become root with the "-b" argument.

[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#table-of-contents)

</br>

## Managing Variables
In Ansible, we can create or use existing variables to create a more dynamic playbook.
 
### Creating Variables
We can create variables separating multiple words with an '_'.

Examples of valid variable names:
> foo \
> foo_env \
> foo_port \
> foo5 \
> _foo

[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#table-of-contents)
 
</br>

### Using Variables
We can manipulate variable output in two ways:
- {{ ansible_facts["eth0"]["ipv4"]["address"] }}
- {{ ansible_facts.eth0.ipv4.address }}
> Both methods will give the same output: "value" \
> ansible_facts { \
> &nbsp;&nbsp;&nbsp;&nbsp;eth0: { \
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ipv4: { \
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;address: "value" \
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;} \
> &nbsp;&nbsp;&nbsp;&nbsp;} \
> }

### 1. Specifying on Command Line
Using the '-e' or '--extra-vars' attribute, we can specify variables in the command line:
```
ansible-playbook playbook.yml --extra-vars "version=1.23.45 other_variable=foo"
```
> Inside the playbook.yml, \
> src: "{{ version }}"
> dest: "{{ other_variable }}"

[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#table-of-contents)

### 2. Specifying Inside Playbook
We can specify variables and use them inside of our playbooks in this manner:
```
---
- hosts: all
  become: yes
  vars:
    - apache2_src: <path>
    - apache2_dest: <path>
  tasks:
    - name: Installing Apache2
      apt:
        name: apache2
        state: present
    - name: Adding File Apache2 Server
      copy:
        src: "{{ apache2_src }}"
        dest: "{{ apache2_dest }}"
```
> Depending on where the variable is mentioned, there may or may not be a need to specify it with " ". \
> \
> E.g \
> url: /usr/share/{{ package_name }}/html
 
[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#table-of-contents)

### 3. Creating a Variable File
We can create a file for our variables as well.
```
vi apache2_vars.yml
```
Inside the apache2_vars file, we can add the variables:
```
apache2_src: <path>
apache2_dest: <path>
```

To use it in our playbooks, we have to add this file in the playbook in this manner:
```
---
- hosts: all
  become: yes
  vars_files:
    - apache2_vars.yml
[...]
```
[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#table-of-contents)

### 4. From a Registered Variable
After running a task, that task will output some metadata. We can save that metadata in a variable and call a specific metadata.
```
- name: Replace index.html
  copy:
    dest: /var/www/html/index.html
    content: "Hello World"
  register: index_file
- name: Output index_file Variable Contents
  debug:
    msg: "{{ index_file }}"
```
Using the debug module, we can print out all the metadata that the task will output and use it as a reference to use as variables. \
In this case, the debug module will only print out the specific checksum variable output:
```
[...]
- name: Output index_file Variable Contents
  debug:
    msg: "{{ index_file.checksum }}"
```
[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#table-of-contents)

### 5. Gathering Facts
This specific module is used to gather information about the inventory. The command below displays facts from all hosts and stores them indexed by hostname at /tmp/facts.
```
ansible -i inventory -m gather_facts --tree /tmp/facts
```
The metadata output from gathering facts can be used as a variable in a similar fashion like a registered variable.
 
[Back to Top](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#table-of-contents)

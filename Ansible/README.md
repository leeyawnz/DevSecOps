# Ansible
This is my own personal documentation of the common Ansible commands. \
As most OS are Linux in DevOps, this in the context of Linux (Ubuntu).

</br>

## Table of Contents:
> - [Setting Up Ansible](https://github.com/leeyawnz/DevSecOps/tree/main/Ansible#setting-up-ansible)
> - [Accessing a Remote Server with a Controller Server](https://github.com/leeyawnz/DevSecOps/blob/main/Ansible/README.md#accessing-a-remote-server-with-a-controller-server)
>   - [Using SSH-Copy-ID](https://github.com/leeyawnz/DevSecOps/tree/main/Ansible#using-ssh-copy-id)
>   - [Manually](https://github.com/leeyawnz/DevSecOps/tree/main/Ansible#manually)

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

## Ansible Terminologies
### Inventory
In Ansible, there is a file called inventory. Inside the inventory file, we can add all the remote servers (including localhost) so that we can target them when we perform Ansible configuration management tasks.

We can create an inventory file easily.
```
vi inventory
```
Inside this inventory file, we can group servers together.
> ```
> ipaddress1
> \
> [web]
> webserver1
> webserver2
> \
> [db]
> dbserver1

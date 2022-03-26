# Ansible
This is my own personal documentation of the common Ansible commands. \
As most OS are Linux in DevOps, this in the context of Linux (Ubuntu).

## Table of Contents:
> - [Setting Up Ansible]()
> - [Introduction to SSH-Keygen]()
>   - [ssh Demo]()

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

</br>

## Introduction to SSH-Keygen
Taking advantage of SSH, we can access different servers remotely to configure them from a controller server. This is done by making sure that all servers have the public key that was given by the controller server. 

### Accessing a Remote Server with a Controller Server
#### Using ssh-copy-id
Need to find out on how to do this

#### Manually
#### 1. Setting up our instances
To see this in action, we can first create 3 instances in AWS. After creating the 3 instances,run all 3 instances on 3 separate terminals.

Inside the first instance, rename it to controller
```
sudo hostnamectl set-hostname controller
```
Inside the second instance, rename it to server1
```
sudo hostnamectl set-hostname server1
```
Inside the third instance, rename it to server2
```
sudo hostnamectl set-hostname server2
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

4. Copying ssh Public Key into server1 and server2
From the controller instance, copy the ssh key from the 

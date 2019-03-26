# Ansible Vagrant profile for Developement

## Background

Vagrant and VirtualBox for developements  (Tested with only Mac but should work for most of the Linux variations)

## Getting Started

This README file is inside a folder that contains a `Vagrantfile`  which tells Vagrant how to set up your virtual machine in VirtualBox.

To use the vagrant file, you will need to have done the following:

  1. Download and Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
  2. Download and Install [Vagrant](https://www.vagrantup.com/downloads.html)
  3. Install [Ansible](https://www.ansible.com/) ([guide for installing Ansible](http://docs.ansible.com/ansible/latest/intro_installation.html))
  4. Open a shell prompt (Terminal app on a Mac) and cd into the folder containing the `Vagrantfile`
  5. Run the following command to install the necessary Ansible roles for this profile: `$ ansible-galaxy install -r requirements.yml`
  6. ssh-keygen -t rsa -b 4096 -C "your_email@example.com"  and name the key file as devbox

Once all of that is done, you can simply type in `vagrant up`, and Vagrant will create a new VM, install the base box, and configure it.

Once the new VM is up and running (after `vagrant up` is complete and you're back at the command prompt), you can log into it via SSH if you'd like by typing in `vagrant ssh`. Otherwise, the next steps are below.
 
##### What is installed for your developement box?
 
 - Python 3.6 
 - Java openjdk-8-jdk
 - Node 10.15.3 and NPM 6.9
 - Docker
 - Kafka and Zookeeper
 - TMux
 - Python Pip3  
      - invoke
      - ansible-vault
      - awscli  
      - boto3
      - requests  
      
##### How to connect?     

If you have configured correctly with your devbox ssh-keys as specified above then you should be able connect via SSH as follows

ssh -A vagrant@192.168.57.57

    * Firewalls disabled and all ports are open for your developement purpose. so 192.168.57.57:<port> is open to access from host machine
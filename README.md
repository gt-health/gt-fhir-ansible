This is the readme to the gt-fhir-ansible project

This project is an ansible project built with ansible version 2.1.1.0

The purpose of this project is to provide a way to quickly and easily build the gt-fhir-infrastructure for developing.

The gt-fhir infrastructure consist of 3 roles
	* A gt-fhir datastore and webapp front.
	* A SMRTonFHIR Authentication Server
	* A SMRTonFHIR app/app client server

The datastore handles the EHR/model part of the process. The Authentication server handles user management. And the SMRTonFHIR app client provides the real web applications that access the database

Each of these components require special network information about the other 2. We are using ansible to quickly create this networked infrastructure.

The following is the steps needed to config yourself perfectly

0) Before step 1, we need to install and configure ansible

sudo apt-get install ansible --version=2.1.1

in /etc/ansible/ansible.cfg
Uncomment and change the pipelining entry to:

pipelining=True

in /etc/ansible/hosts
Add these entries at the bottom of the file:
[GT-FHIR]
localhost

[SMRTonFHIRAuth]
localhost

[SMRTonFHIRApps]
localhost

Note that you should replace "localhost" with the IP address of the Virtual Machine you have configured for each GT-FHIR component.

Add an ansible user to ssh to.

sudo adduser ansible

in /etc/sudoers add this line:
ansible ALL=(ALL:ALL) ALL

1) Find the VM machine

This project is really managed within a virtual box machine. Pulling the hosted snapshot machine from virtualbox would be the best way to start setting up your environment. ***VBox Not Hosted yet***

If not you will have to do installation of all the software yourself. This includes
	* Python
	* Ansible
	* An SSH client of somesort

2) Setup your host machine configuration

By default, Ansible will configure the machines locally, on the provided VM. The way to manage this is to change the /etc/ansible/hosts file. If you want to stay with the localhost defaults, please use the local_config/hosts file. Ansible is expecting a structured hosts file like the one we provide.

3) Run ansible playbook

ansible-playbook ${THIS_DIRECTORY}/roles/GT-FHIR.yaml -b --ask-pass --ask-become-pass

ansible will prompt you for the ansible user's password that  you  set eariler. The sudo password is the same password.

NOTE: If ansible cannot connect, you have no setup your ansible user correctly

NOTE: Sometimes ansible will not start the tomcat server correctly. In which case, run

sudo /opt/tomcat/apache-tomcat-8.5.6/bin/startup.sh


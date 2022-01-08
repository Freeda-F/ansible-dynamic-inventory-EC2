# Ansible-EC2-httpd

Deploy a sample HTML Website on AWS EC2 through ANSIBLE using dynamic inventory.

## Overview

In this demo, we will be provisioning the following :

- Provision an SSH keypair, security groups and EC2 instance through Ansible.
- Retrieve the IP Address of instance using dynamic inventory concept.
- Configuring the web server through Ansible.

## Prerequisite

- Access key and secret key for AWS IAM user with administrator access.
- Install Ansible on your machine (ansible master).

## Variables Used

The files used for variable declaration are below :
 - ec2.vars
```
keypair_name: "ansible" #-----keypair name-----#
region: "ap-south-1"  #-----region name-----#
sg1: "ansible-remote" #-----security group-1 name-----#
sg2: "ansible-webserver"  #-----securitygroup-2 name-----#
```
- access-key.vars
```
access_key: "AKIAXXXXXXXXXXXXAAH" #----access key-----#
secret_key: "IL/XXXXXXXXXXXXXXXXXXXXXeY9nzTFs" #-----secret-key-----#
```
- webserver.vars
```
apache_user: "apache" #-----httpd-user-----#
apache_group: "apache" #-----httpd-group-----#
domain_name: "sample.freeda-francis.tech" #-----domain name-----#
apache_port: 80 #-----httpd-port-----#
```

## Provisioning

##### Step 1: Create a task in AWS for generating a new SSH keypair using the ansible module - [ec2_key](https://docs.ansible.com/ansible/latest/collections/amazon/aws/ec2_key_module.html) and save .pem file to the local machine.

![image](https://user-images.githubusercontent.com/93197553/148648004-98de5005-eb11-4f96-ab9a-9e6ccb5d1ab2.png)

##### Step 2: Create the required AWS security groups for the EC2 instances. Here, we will be creating 2 security groups - sg1(Allow all connections from port 22) & sg2(Allow all connections from port 80,443).

![image](https://user-images.githubusercontent.com/93197553/148648097-d6fcce4f-db99-4388-85d2-2aae338e79ef.png)

##### Step 3: Create a task to provision the AWS EC2 instance, for this will use [ec2](https://docs.ansible.com/ansible/latest/collections/amazon/aws/ec2_module.html) module available which can launch the instance.

![image](https://user-images.githubusercontent.com/93197553/148648140-c1c9f847-edcc-4a15-a935-5f43eba61837.png)

##### Step 4: Create a dynamic inventory using the [add_host](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/add_host_module.html) module in ansible.

![image](https://user-images.githubusercontent.com/93197553/148648159-f1760258-bc65-4dc9-89d1-e0b97c9f3920.png)

##### Step 5: Install and configure apache webserver along with its required dependencies through another ansible playbook.

![image](https://user-images.githubusercontent.com/93197553/148648298-d06039da-7eeb-4bbe-a41b-c87f9dceaa5a.png)

##### Step 6: In addition to this, we will be displaying a sample HTML website contents from github using [git](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/git_module.html) module in ansible.

![image](https://user-images.githubusercontent.com/93197553/148648357-1a86249a-5af1-4282-b04b-7e56fec8d83a.png)

## Usage

1. Clone the repository using the below command
```
git clone https://github.com/Freeda-F/ansible-ec2-httpd.git
```
2. Then, make the required changes in above mentioned files for variables - ec2.vars,webserver.vars and access-key.vars.
3. Finally, you can run the ansible playbook using the command
```
ansible-playbook aws-ec2.yml
```

## Result

Now our web server has been configured and we can visit our website using the public IP or domain name.

![image](https://user-images.githubusercontent.com/93197553/148648779-d68643be-e0a6-486b-978a-3e071db3a426.png)



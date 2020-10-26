# Deployment of Load Balancer and Web Servers On AWS Using ANSIBLE

TASK-
Statement- Deploy a Load Balancer and Multiple Web Servers on AWS instances through ANSIBLE!!
♦️ Provision EC2 instances through ansible.

♦️ Retrieve the IP Address of instances using the dynamic inventory concept.

♦️ Configure the Web Servers through the ansible.

♦️ Configure the Load Balancer through the ansible .

♦️ The target nodes of the load balancer should auto-update as per the status of web servers.


IMPORTANT TO LEARN:-

*Ansible is an open source IT Configuration Management, Deployment & Orchestration tool. It aims to provide large productivity gains to a wide variety of automation challenges. This tool is very simple to use yet powerful enough to automate complex multi-tier IT application environments. 

* Web Server is a computer that runs websites. It's a computer program that distributes web pages as they are requisitioned. The basic objective of the web server is to store, process and deliver web pages to the users.

* Load Balancer performs the following functions: Distributes client requests or network load efficiently across multiple servers. Ensures high availability and reliability by sending requests only to servers that are online. Provides the flexibility to add or subtract servers as demand dictates.


PRACTICAL WORK -
Step1- First of all, we have to install boto before launching the instance.

pip3 install boto

pip3 install boto3

Step2- Create a role for launching the instance on the top of AWS.

mkdir /etc/myroles
mkdir /etc/myroles

cd /etc/myroles


ansible-galaxy init ec2_role

![image](https://user-images.githubusercontent.com/61896468/97143385-9b188a80-1788-11eb-84ca-49af16e6d335.png)
![image](https://user-images.githubusercontent.com/61896468/97143392-a075d500-1788-11eb-8f64-ad6786804655.png)
![image](https://user-images.githubusercontent.com/61896468/97143413-a966a680-1788-11eb-84cc-346cb4f13bff.png)
Step3- Inside the vars/main.yml file, we have to pass AWS Access Key and Secret Key. We have to provide Access key inside "myuser" variable and Secret key inside "mypass" variable -

vim vars/main.yml
   
   myuser: XXXXXXXXX
   mypass: XXXXXXXXXXX

Step4- Inside ansible configuration file.

vim /etc/ansible/ansible.cfg

![image](https://user-images.githubusercontent.com/61896468/97143467-c0a59400-1788-11eb-9dc2-43675ab9d8f6.png) 
cd 

cd /projects

vim launch.yml

![image](https://user-images.githubusercontent.com/61896468/97143512-d4e99100-1788-11eb-8c90-cf39d70bf66b.png) 
Step5- Now, we have to run the ansible playbook using below command.

ansible-playbook launch.yml

![image](https://user-images.githubusercontent.com/61896468/97143549-e763ca80-1788-11eb-9b65-4c720b4e1c04.png) 
OUTPUT OF Step5 -

![image](https://user-images.githubusercontent.com/61896468/97143586-f9de0400-1788-11eb-8ea2-2a888965e56a.png) 
Step6- To get the Instance IP dynamically in the inventory, we have to download ec2.py and ec2.ini files

mkdir /mydb

cd /mydb

yum install wget 

wget https://raw.githubusercontent.com/ansible/ansible/stable-2.9/contrib/inventory/ec2.py


![image](https://user-images.githubusercontent.com/61896468/97143687-272ab200-1789-11eb-991d-8960f37b591d.png)
![image](https://user-images.githubusercontent.com/61896468/97143705-2d209300-1789-11eb-952f-1c5446d8d4f6.png)
Inside ec2.py, just change one line => #!/usr/bin/python3

![image](https://user-images.githubusercontent.com/61896468/97143741-40cbf980-1789-11eb-8b8a-2eaa16d12f87.png) 
# wget https://raw.githubusercontent.com/ansible/ansible/stable-2.9/contrib/inventory/ec2.ini

![image](https://user-images.githubusercontent.com/61896468/97143778-56d9ba00-1789-11eb-92da-d135320c9723.png) 
export AWS_REGION='ap-south-1'

export AWS_ACCESS_KEY_ID='XXXXXXXXXXX'


export AWS_SECRET_ACCESS_KEY='XXXXXXXXXXX'

Step7- Now inside the ansible configuration file, change the inventory to /mydb -

![image](https://user-images.githubusercontent.com/61896468/97143829-68bb5d00-1789-11eb-8c1b-d25a90372557.png) 
Step8- Use command given below which will show all the host IP's.

ansible all --list-hosts

![image](https://user-images.githubusercontent.com/61896468/97143871-7cff5a00-1789-11eb-9310-9f6ee083f44e.png) 
 Step9 - First we have to create roles for configuring the webserver and haproxy software inside the EC2 instances.

cd /etc/myroles

ansible-galaxy init Webserver

ansible-galaxy init lbserver

![image](https://user-images.githubusercontent.com/61896468/97143908-8ee0fd00-1789-11eb-9830-ebd239339577.png)
![image](https://user-images.githubusercontent.com/61896468/97143921-956f7480-1789-11eb-811c-2187b2304d46.png)
For lbserver -
![image](https://user-images.githubusercontent.com/61896468/97143992-b1731600-1789-11eb-86c2-b00df0767bda.png)
*Make some changes inside the haproxy.cfg file.

vim haproxy.cfg

![image](https://user-images.githubusercontent.com/61896468/97144038-c51e7c80-1789-11eb-96fa-d34550fa2b94.png) 
vim tasks/main.yml

![image](https://user-images.githubusercontent.com/61896468/97144085-d8c9e300-1789-11eb-97e5-bd6b46ec77fb.png) 
vim handlers/main.yml

![image](https://user-images.githubusercontent.com/61896468/97144134-ec754980-1789-11eb-85c5-de1596bd6a4b.png) 
Step10- Now we have to create a file inside the projects folder for the webserver and lbserver roles .

cd 

cd /pojects

vim setup.yml

![image](https://user-images.githubusercontent.com/61896468/97144177-fdbe5600-1789-11eb-9cae-f620297c3b4a.png) 
Step11- Make some changes inside the inventory file and the ansible.cfg file.

vim /etc/myhosts.txt

![image](https://user-images.githubusercontent.com/61896468/97144208-0e6ecc00-178a-11eb-8b76-9255e9d64d66.png) 
vim /etc/ansible/ansible.cfg

![image](https://user-images.githubusercontent.com/61896468/97144248-1e86ab80-178a-11eb-8543-2e7eebcdb58d.png) 
Step12- Now, for running the setup.yml playbook we have to use command given below.

![image](https://user-images.githubusercontent.com/61896468/97144285-2f372180-178a-11eb-9546-7b1049c59bd1.png)
![image](https://user-images.githubusercontent.com/61896468/97144300-352d0280-178a-11eb-8f2f-69a5d94e0b96.png)
![image](https://user-images.githubusercontent.com/61896468/97144324-3eb66a80-178a-11eb-86c6-9833122ee242.png)
FINAL OUTPUT :- Now, on the Browser give the Public IP with the Port of the Load Balancer as IP:Port.



![image](https://user-images.githubusercontent.com/61896468/97143237-5260d180-1788-11eb-881a-833d1f38bcf3.png)

![image](https://user-images.githubusercontent.com/61896468/97143270-63114780-1788-11eb-8588-0562082c3575.png)

![image](https://user-images.githubusercontent.com/61896468/97143281-686e9200-1788-11eb-9b91-245817211979.png)

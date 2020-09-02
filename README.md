## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.



These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the yaml file may be used to install only certain pieces of it, such as Filebeat.

  ---
- name: installing and launching filebeat
  hosts: webservers
  become: yes
  tasks:

  - name: download filebeat deb
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb


  - name: install filebeat deb
    command: dpkg -i filebeat-7.4.0-amd64.deb

  - name: drop in filebeat.yml
    copy:
      src: /etc/ansible/files/filebeat-configuration.yml
      dest: /etc/filebeat/filebeat.yml

  - name: enable and configure system module
    command: filebeat modules enable system

  - name: setup filebeat
    command: filebeat setup

  - name: start filebeat service
    command: systemctl start filebeat



This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly Available, in addition to restricting Permissions to the network.
-  What aspect of security do load balancers protect? If Web-1 goes down, traffic will go to web-2, and so on. Load balancer redirect traffic to the next available zone.

 What is the advantage of a jump box? JumpBox restrict IP Addresses access. Manage who log in and out.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the Virtual Machine and system  logs.
- What does Filebeat watch for? Its Manages, collect log files
-  What does Metricbeat record? It records system and CPU memory using elasticsearch.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux OS         |
| DVWA-1   |          | 10.0.0.5   |                  |                              
| DVWA-2   |          | 10.0.0.6   |                  |
| Web-3    |          | 10.0.0.7   |                  |                             
| ELK      |Server    | 10.1.0.4   | Linux OS         |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the local Workstation machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 159.250.76.226

Machines within the network can only be accessed by JumpBox.
- Machines allow to access your ELK VM are JumpBox and two DVWA 
What was its IP address? 10.0.0.4, 10.0.0.5, 10.0.0.6

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | No                  | 10.0.0.5 10.0.0.6    |
|          |                     |                      |
|          |                     |                      |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Ansible playbook 
The playbook implements the following tasks:
- apt install docker.io
-docker pull sepb/elk
- docker run into your ansible
- change the ansible hosts to your elkserver
-change the ansible remote user to your host name.
-create a yaml script to make connection to the elk application
-make sure your ports are connected to 5601,9200 and 5044
The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.
- sudo docker ps

https://github.com/yberregu/ELK_Project1/blob/master/Diagrams/2.PNG

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.4, 10.0.0.5, and 10.0.0.6
We have installed the following Beats on these machines:
- filebeat.yml  -metricbeat.yml

These Beats allow us to collect the following information from each machine:
- Filebeat collects data about the file system
- Metricbeat collects machine metrics, such as uptime.
CPU usage: The heavier the load on a machine's CPU, the more likely it is to fail.
### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the ssh-keygen file to ssh public key.
- Update the security rules file to include IP address
- Run the playbook, and navigate to terminal to check that the installation worked as expected.

  Answer the following questions to fill in the blanks:
- Which file is the playbook? Where do you copy it? the filebeat 
- Which file do you update to make Ansible run the playbook on a specific machine? We update The Hosts file Private IP ADDRESS and ansible.cfg to [remote user:sysadmin].How do I specify which machine to install the ELK server on versus which to install Filebeat on?
- Which URL do you navigate to in order to check that the ELK server is running? http://<(YourIpAddress)>:5601/app/kibana#/home

As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc.
ssh jump@(IpAddress)
connect to the Ansible container in the box
sudo docker container list -a
Sudo docker container start [container name]
sudo docker container attach [container name]
change dir. /etc/ansible
nano hosts (update IP on[webservers][elkservers]
nano ansible.cfg (remote_user to which server you want to use)
run the playbook command: root@0e52a6c7f577:/etc/ansible# ansible-playbook roles/filebeat-playbook.yml
see Ansible Dir. Ansible.cfg and Ansible-Hosts for configurations.

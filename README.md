## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/yberregu/ELK_Project1/blob/master/Diagrams/NetWork-Topology%20(2).png

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
      src: /etc/ansible/filebeat-configuration.yml
      dest: /etc/filebeat/filebeat.yml

  - name: enable and configure system module
    command: filebeat modules enable system

  - name: setup filebeat
    command: filebeat setup

  - name: start filebeat service
    command: systemctl start filebeat



This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly Available, in addition to restricting Permissions to the network.
-  What aspect of security do load balancers protect? If Web-1 goes down, traffic will go to web-2, and so on. Load balancer redirects traffic to the next available zone.

 What is the advantage of a jump box? JumpBox restricts IP Addresses access. Manage who log in and out.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the Virtual Machine and system  logs.
- What does Filebeat watch for? Its Manages, collect log files
-  What does Metricbeat record? It records system and CPU memory using elasticsearch.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux OS         |
| Web-1    | webserver| 10.0.0.5   |                  |                              
| Web-2    | webserver| 10.0.0.6   |                  |
|          |          |            |                  |                             
| ELK      | ElkServer| 10.1.0.4   | Linux OS         |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the elk sever can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 159.250.76.226 tcp 5601.

Machines within the network can only be accessed by JumpBox and work station.
- Machines allow to access your ELK VM are JumpBox and workstation port 5601.
What was its IP address? 10.0.0.4, 159.250.76.226

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | No                  |10.0.0.4 ON SSH 22    |
|   web-1  | no                  |10.0.0.4 on SSH 22    |
|   WEB-2  | NO                  |10.0.0.4 on SSH 22    |
| ELK      | NO                  |Workstation PIP ON 5601|

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Ansible playbook 
The playbook implements the following tasks automatically in seconds:
- apt install docker.io
-python3-pip
 -Specify a different group of machines as well as a different remote user
  - name: Config elk VM with Docker
    hosts: elk
    remote_user: sysadmin
    become: true
    tasks:
 -Launching and Exposing the container with these published ports:
 `5601:5601` 
 `9200:9200`
 `5044:5044`

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.
- sudo docker ps

https://github.com/yberregu/ELK_Project1/blob/master/Diagrams/2.PNG

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5, 10.0.0.6
We have installed the following Beats on these machines:
- filebeat.yml  -metricbeat.yml

These Beats allow us to collect the following information from each machine:
- Filebeat collects data about the file system, log events.
- Metricbeat collects machine metrics, such as uptime.
CPU usage: The heavier the load on a machine's CPU, the more likely it is to fail.
### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the /etc/ansible/files/filebeat-config.yml file to '/etc/filebeat/filebeat-playbook.yml'.
- Update the security rules file to include IP address
- Run the playbook, and navigate to terminal to check that the installation worked as expected.

  Answer the following questions to fill in the blanks:
- Which file is the playbook? Where do you copy it? the filebeat{filebeat-configuration.yml}, to webservers filebeat dir.
- Which file do you update to make Ansible run the playbook on a specific machine? We update nano filebeat-config.yml, hosts: ["10.1.0.4:9200"]
  username: "elastic"
  password: "changeme” 

 setup.kibana:
  host: "10.1.0.4:5601". 
  -How do I specify which machine to install the ELK server on versus which to install Filebeat on? change host installation hosts to either webservers or elk.
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
run the playbook command: root@0e52a6c7f577:/etc/ansible/roles# ansible-playbook filebeat-playbook.yml
see Ansible Dir. Ansible.cfg and Ansible-Hosts for configurations.
change the ansible hosts to your elkserver and run sudo docker ps in the command line to see if sebp/elk: 761 is installed.

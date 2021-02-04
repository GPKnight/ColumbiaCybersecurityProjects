# ColumbiaCybersecurityProjects
Repository for Project Work completed during 2020/2021 class
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/GPKnight/ColumbiaCyberSecurityProjects/blob/main/NetworkTopology.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, individual playbooks may be used to install only certain pieces of the network structure, such as Filebeat and Metricbeat.

Docker Setup

-Configure Docker on Jump Box VM: https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/DockerVMConfig.yml

-Ansible config file (change line #106 to the appropriate username): https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/ansible.cfg

-Ansible hosts file (change the IPs / Group names i.e. Webservers / ELK, etc): https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/hosts

---------------------------------------------------------------------------------------------------------------------------------------------------------

Installation of ELK Stack on ELK VM

-Installation of ELK stack on VM: https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/install-elk.yml

---------------------------------------------------------------------------------------------------------------------------------------------------------

Installation of Filebeat log service and Metricbeat metric service

-Installation of Filebeat service: https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/filebeat-playbook.%2Cyml

-Filebeat config file (make changes to line 1105: elasticsearch IP/port, line 1805: kibana host IP/port): https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/filebeat-config.yml 

-Installation of Metricbeat service: https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/metricbeat-playbook.yml

-Metricbeat config file (change line 61: kibana host IP/port, line 95 host IP/port): 
  
  
  
 ###Contents of README.md

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly _____, in addition to restricting _____ to the network.
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box?_

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the _____ and system _____.
- _TODO: What does Filebeat watch for?_
- _TODO: What does Metricbeat record?_

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.1   | Linux            |
| TODO     |          |            |                  |
| TODO     |          |            |                  |
| TODO     |          |            |                  |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the _____ machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- _TODO: Add whitelisted IP addresses_

Machines within the network can only be accessed by _____.
- _TODO: Which machine did you allow to access your ELK VM? What was its IP address?_

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes/No              | 10.0.0.1 10.0.0.2    |
|          |                     |                      |
|          |                     |                      |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible?_

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- ...
- ...

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: List the IP addresses of the machines you are monitoring_

We have installed the following Beats on these machines:
- _TODO: Specify which Beats you successfully installed_

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the _____ file to _____.
- Update the _____ file to include...
- Run the playbook, and navigate to ____ to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

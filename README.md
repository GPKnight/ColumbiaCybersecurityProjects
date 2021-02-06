# ColumbiaCybersecurityProjects
Repository for Project Work completed during 2020/2021 class
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/GPKnight/ColumbiaCyberSecurityProjects/blob/main/NetworkTopology.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, individual playbooks may be used to install only certain pieces of the network structure, such as Filebeat and Metricbeat.

## Docker Setup

-Configure Docker on Jump Box VM: https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/DockerVMConfig.yml

-Ansible config file (change line #106 to the appropriate username): https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/ansible.cfg

-Ansible hosts file (change the IPs / Group names i.e. Webservers / ELK, etc): https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/hosts

---------------------------------------------------------------------------------------------------------------------------------------------------------

## Installation of ELK Stack on ELK VM

-Installation of ELK stack on VM: https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/install-elk.yml

---------------------------------------------------------------------------------------------------------------------------------------------------------

## Installation of Filebeat log service and Metricbeat metric service

-Installation of Filebeat service: https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/filebeat-playbook.%2Cyml

-Filebeat config file (make changes to line 1105: elasticsearch IP/port, line 1805: kibana host IP/port): https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/filebeat-config.yml 

-Installation of Metricbeat service: https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/metricbeat-playbook.yml

-Metricbeat config file (change line 61: kibana host IP/port, line 95 host IP/port): https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/metricbeat-config.yml
  
  
  
 ### Contents of README.md

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly redundant ensuring availability, in addition to restricting any overload within the network.

- A load balancer, with respect to the  CIA Triad, deals with availability. It allows distribution of incoming web traffic between hot servers, allowing for the alleviation of traffic stress on the server.  And, in the case of server failure, maintains the availability of information to remain unimpeded. A load balancer can also call for user authentication and can include other protections such as Web Application Firewall (WAF). A load balancer may also mitigate distributed denial-of-service (DDoS) attacks by dropping traffic indicative of such attacks before it reaches your web servers. [Load Balancer Source Info](https://lumecloud.com/what-does-a-load-balancer-do/)

- The purpose of a Jump Box is to provide a secure and isolated server to allow sys admins to conduct administrative tasks over the network. By using a Jump Box, it isolates the instances from direct outside connection. However, it also runs the risk of catastrophic failure because once a threat authenticates into the jump box/server there are fewer roadblocks between it and sensitive data. For instance, in the case of the 2015 Office of Personnel Management (OPM) attack, "By controling the jumpbox, the attackers had gained access to every nook and cranny of OPM's digital terrain." [Wired Postmortem](https://www.wired.com/2016/10/inside-cyberattack-shocked-us-government/)

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.

- Filebeat generates logs and forwards them through a specified port to a single server allowing admin to review/visualize and index multiple machines in single location without the need to SSH into multiple machines. Per the Filebeat Overview page: "When you start Filebeat, it starts one or more inputs that look in the locations you’ve specified for log data. For each log that Filebeat locates, Filebeat starts a harvester. Each harvester reads a single log for new content and sends the new log data to libbeat, which aggregates the events and sends the aggregated data to the output that you’ve configured for Filebeat." [Filebeat Overview](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-overview.html#:~:text=Filebeat%20is%20a%20lightweight%20shipper,Elasticsearch%20or%20Logstash%20for%20indexing)

- Metricbeat is a service deployed on VMs throughout your network to monitor metrics such as CPU usage, memory, file system, disk Input/Output (IO), and network IO. It also records statistics from services deployed on your network VMs including but not limited to Apache, MySQL, NGINX [Full List Here](https://www.elastic.co/guide/en/beats/metricbeat/current/metricbeat-modules.html)

- Both Metricbeat and Filebeat are members of the Elastic Beats family, which are "Beats are open source data shippers that you install as agents on your servers to send operational data to [Elasticsearch](https://www.elastic.co/products/elasticsearch)." A full list of the Beat agents can be found [here](https://www.elastic.co/guide/en/beats/libbeat/7.10/beats-reference.html).

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function                               | IP Address                                                                             | Operating System                               |
|----------|----------------------------------------|----------------------------------------------------------------------------------------|------------------------------------------------|
| Jump Box | Access Point to Virtual Networks       | Dynamic (Public) 10.0.0.4 (Private)                                                    | Linux 5.4.0-1039-azure x86_64 (Ubuntu 20.04.2) |
| Web-1    | Web Traffic Machine 1                  | 52.136.123.97 (Public - Load Balancer & VM) 10.0.0.7 (Private)                         | Linux 5.4.0-1039-azure x86_64 (Ubuntu 20.04.2) |
| Web-2    | Web Traffic Machine 2                  | 52.136.123.97 (Public - Load Balancer) 52.168.176.167 (Public - VM) 10.0.0.8 (Private) | Linux 5.4.0-1039-azure x86_64 (Ubuntu 20.04.2) |
| Elk VM   | ELK Stack Machine to hold logs/metrics | 52.184.150.17 (Public) 10.1.0.4 (Private)                                              | Linux 5.4.0-1039-azure x86_64 (Ubuntu 20.04.2) |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 71.235.45.209 (Personal Laptop)

Machines within the network can only be accessed by jumpbox.
- Jumpbox, private IP of 10.0.0.4

A summary of the access policies in place can be found in the table below.

| Name          |  Publicly Accessible  | Allowed IP Addresses         |
|---------------|:---------------------:|------------------------------|
| Jumpbox       | Yes, from specific IP | 71.235.45.209                |
| Web-1         | No                    | 10.0.0.4                     |
| Web-2         | No                    | 10.0.0.4                     |
| ELK VM        | Yes - Port 5601       | 10.0.0.4, 71.235.45.209:5601 |
| Load Balancer | Yes - Port 80         | 10.0.0.4, 71.235.45.209:80   |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because:

- There is no agent required to run Ansible.  All you need to do to initiate it is ssh into a machine and install python.
- The ease of use in getting Ansible to work makes the learning curve small.  The syntax of writiing modules within a playbook is not difficult to learn.
- It is a simple stable environment which allows sys admins to configure many machines at once, allowing for more efficient network management.
- It is "push" based, versus "pull" based, which Ansible alternatives Chef and Puppet are.

- [See Comparison of Chef/Puppet/Ansible](https://www.veritis.com/wp-content/uploads/2020/03/infographic-chef-vs-puppet-vs-ansible-what-are-the-differences.png)

The playbook implements the following tasks:

- The first module within the [Install Elk](https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/install-elk.yml) playbook installs Docker engine. (Note that according to the [Docker Documentation](https://docs.docker.com/engine/install/debian/) docker.io is an outdated version of Docker)

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

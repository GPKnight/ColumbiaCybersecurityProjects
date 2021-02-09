# ColumbiaCybersecurityProjects
Repository for Project Work completed during 2020/2021 class
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Diagrams/NetworkTopology.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, individual playbooks may be used to install only certain pieces of the network structure, such as Filebeat and Metricbeat.

## Pulling a Docker container onto Jumpbox

- Once your virtual network has been setup to include a jumpbox and webservers, it is time to configure the jumpbox before we can proceed with any further steps.

- SSH into the jumpbox using the username and ip configured during setup in the Microsoft Azure account (for these files all usernames were kept as azureuser)

- Once in the jumpbox, run "apt update" then "sudo apt install docker.io"

- Once Docker is installed on the jumpbox, it must be initiated, "sudo systemctl start docker"

- Following the initializing of Docker, you will want to pull the specific Ansible container by running "sudo docker pull cyberxsecurity/ansible"

- To then start the Ansible container, run "sudo docker run -ti cyberxsecurity/ansible:latest bash". This command will only need to be run once. After the container is created, you will run "sudo docker container start [Container Name Here] and "sudo docker container attach [Container Name Here] for every subsequent login.


## Docker  DVWA Container on Webserver VM Setup

-Configure Docker and install DVWA containers on web VMs: https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Ansible/DockerVMConfig.yml

-Ansible config file (change line #106 to the appropriate username): https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Ansible/ansible.cfg

-Ansible hosts file (change the IPs / Group names i.e. Webservers / ELK, etc): https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Ansible/hosts

---------------------------------------------------------------------------------------------------------------------------------------------------------

## Installation of ELK Stack on ELK VM

-Installation of ELK stack on VM: https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Ansible/install-elk.yml

---------------------------------------------------------------------------------------------------------------------------------------------------------

## Installation of Filebeat log service and Metricbeat metric service

-Installation of Filebeat service: https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Ansible/filebeat-playbook.yml

-Filebeat config file (make changes to line 1105: elasticsearch IP/port, line 1805: kibana host IP/port): https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Ansible/filebeat-config.yml 

-Installation of Metricbeat service: https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Ansible/metricbeat-playbook.yml

-Metricbeat config file (change line 61: kibana host IP/port, line 95 host IP/port): https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Ansible/metricbeat-config.yml
  
  
  
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
- 71.235.45.209 (Host Laptop)

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

- The first module within the [Install Elk](https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Ansible/install-elk.yml) playbook pulls the Docker mod. (Note that according to the [Docker Documentation](https://docs.docker.com/engine/install/debian/) docker.io is an outdated version of Docker)

- The second module in the playbook installs Python3, as Docker utilizes the scripting language.

- The third module installs the Docker module allowing the engine to take docker-specific commands within the agent environment.

- The third and fourth modules are redundant, one or the other may be hashed out. Both modules are methods of increasing the virtual memory to an amount suitable to run the elk container.

- The fifth module downloads and installs the ELK stack container and specifies which ports to activate. In this case, 5601, 9200, 5044. All three ports are referenced in either the [Filebeat-config](https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Ansible/filebeat-config.yml) or the [Metricbeat-config](https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Ansible/metricbeat-config.yml) files.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

 [Docker ps screenshot](https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Graphics/dockerps.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- The ELK server is configured to monitor the 2 webserver VMs: Web-1 10.0.0.7 and Web-2 10.0.0.8

We have installed the following Beats on these machines:
- Filebeat and Metricbeat are installed on Web-1 and Web-2, 10.0.0.7 and 10.0.0.8, respectively.

These Beats allow us to collect the following information from each machine:
- Filebeat consists of two main components: inputs and harvesters. A harvester reads contents of a file, line by line, then sends content to an output. An input is responsible for managing the harvesters and finding all the sources for them to read. Filebeat stores the delivery state of each event it logs in the registry file, ensureing no data loss. [Filebeat Documentation Source](https://www.elastic.co/guide/en/beats/filebeat/current/how-filebeat-works.html)

- Metricbeat is an Elastic beat that can be installed on servers to periodically collect metrics from the OS and services running on the server. Metricbeat can collect mretrics from the following services running on a server [Metricbeat Modules](https://www.elastic.co/guide/en/beats/metricbeat/current/metricbeat-modules.html). Metricbeat can insert any metrics into Elasticsearch, or send to Logstash, Redis, or Kafka. [Metricbeat Source Info](https://www.elastic.co/guide/en/beats/metricbeat/current/metricbeat-overview.html#:~:text=Metricbeat%20is%20a%20lightweight%20shipper,such%20as%20Elasticsearch%20or%20Logstash).

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

### Setting up ELK Stack Server

- Copy [Install ELK Playbook](https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Ansible/install-elk.yml) to /etc/ansible/roles

- Verify that the /etc/ansible/hosts file has a group [elk]
[Private IP of ELK Server] ansible_python_interpreter=/usr/bin/python3

- Run the "Install Elk.yml" playbook.

- Once the playbook has run successfully, verify the ELK server has been configured and is functioning correctly via two methods:

  - From the jumpbox, enter your docker container.  From that container, ssh into the ELK VM and run "sudo docker ps". The resulting output should be something similar to [This](https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Graphics/Elk.png)
  
  - This would also be a good time to double-check the docker.service is running with the correctly specified ports. To do this, run "systemctl status docker.service" and you should see an output similar to [This](https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Graphics/dockerservice.png)
  
### Setting up Filebeat and Metricbeat services on your Webservers

- Copy the [Filebeat Playbook](https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Ansible/filebeat-playbook.yml) and [Metricbeat Playbook](https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Ansible/metricbeat-playbook.yml) file to /etc/ansible/roles

- Copy the [Filebeat Config](https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Ansible/filebeat-config.yml) and [Metricbeat Config](https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Ansible/metricbeat-config.yml) yo /etc/ansible/files

- Update the Filebeat Config file at line #1105 to include: "hosts: ["IPv4 of ELK Server:9200"]" and line #1805 to include: "host: "IPv4 of ELK Server:5601""

- Update the Metricbeat Config file at line #61 to include: "hosts: "IPv4 of ELK Server:5601" and line #96 to include: "host: IPv4 of Elk Server:9200""

- Verify that /ansible/[host](https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Ansible/hosts) has the IPv4 addresses of the webservers in it (in this example, the group is identified as [webservers] 10.0.0.7 10.0.0.8

- Run the playbook by entering ansible-playbook "PlaybookName.yml"

- To verify that the Beat services were installed and running, you can confirm it two ways. The first, to verify the service was installed and running on the webserver. The second verification will confirm that the ports specified within the playbook are correct and pipe logs and metrics to the web, visualized by Kibana through the ELK Stack server.

  - Verify 1: ssh username@webserver1/2IPv4 , enter the following on the Command Line Interface (CLI): "systemctl status filebeat" ; "systemctl status metricbeat" you should see  that both servicess are running.
  
  - Verify 2: On your host machine, go to "ELK VM IPv4:5601/app/kibana", click on Logs and you should see something similar to [this](https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Graphics/FilebeatLog.png) go back to the main Kibana page, click Metrics and you should see something similar to [this](https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Graphics/metricbeatlog.png)
  
  - If both Verify 1 and Verify 2 are confirmed, your Filebeat and Metricbeat installs were succesful, and the return of logs and metrics are succesful.

### As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc. 

Once your virtual network has been set up with the appropriate configuration, ssh into the jumpbox and start and attach your container.  Once in the container navigate to /etc/ansible folder and use the wget command to pull the playbook, host, and config files using the link addresses to this repository:

wget https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Ansible/DockerVMConfig.yml

wget https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Ansible/ansible.cfg 
  ##Change line #106 to applicable username##
  
wget https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Ansible/hosts
  ##Change the IPs / Group Names i.e. Webservers, Elk, etc##
  
wget https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Ansible/install-elk.yml
  ##Make sure the hosts: refers to the correct IPv4 address/IP group name in the Hosts file
  
wget https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Ansible/filebeat-playbook.yml
  ##Make sure the hosts: option in the header indicates the correct webserver IP group/IPv4 addresses

wget https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Ansible/filebeat-config.yml
  ##Make sure that line # 1105 has the correct private IPv4 and port for the ELK VM and line # 1805 the host: has the ELK IPv4 and port are specified

wget https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Ansible/metricbeat-playbook.yml
  ##Make sure the hosts: line in the header has the webserver group name/IPv4 addresses listed

wget https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Ansible/metricbeat-config.yml
  ##Make sure line # 61 has the ELK IP/port specififed and line # 95 has the ELK IP/port specified
  
## When the edits indicated above are made, proceed to the following:

- From the /etc/ansible run the [Install ELK](https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Ansible/install-elk.yml) Playbook by running "ansible-playbook [Install Elk Playbook Name]
 
- Move the filebeat and metricbeat playbook files into /etc/ansible/roles and move the filebeat and metricbeat config files into /etc/ansible/files

- Move into the /etc/ansible/roles folder and run "ansible-playbook [filebeat playbook name]" and "ansible-playbook [metricbeat playbook name]"

- Your virtual network is now equipped to monitor logs and metrics! 

#### [CONGRATS](https://github.com/GPKnight/ColumbiaCybersecurityProjects/blob/main/Graphics/hello-world.jpg)

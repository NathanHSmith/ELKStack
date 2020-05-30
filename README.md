# Gibson93
BlackCherry

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

[Images>Project 1 - Network Diagram.pdf]

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

  - Enter the playbook file.
	install-elk.yml

---
- name: ELK Stack
  hosts: elkservers
  become: true
  tasks:

  - name: Change memory on host machine
	shell: sysctl -w vm.max_map_count=262144

  - name: docker.io
	apt:
  	force_apt_get: yes
  	name: docker.io
  	state: present

- name: Install pip
	apt:
  	force_apt_get: yes
  	name: python-pip
  	state: present

  - name: Install Docker python module
	pip:
  	name: docker
  	state: present

- name: download and launch docker ELK Stack
	docker_container:
  	name: elkstack
  	image: sebp/elk
  	state: started
  	published_ports: 5601:5601
  	published_ports: 9200:9200
  	published_ports: 5044:5044


This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly __resilient_and available__, in addition to restricting _attacks_and access___ to the network.
What aspect of security do load balancers protect? 
They protect application servers against DoS attacks, where the attacker will attempt to flood a network with access requests in order to take down a network.
What is the advantage of a jump box?
A jump box acts a a gateway router for the network, allowing connections from specific IP addresses and ports to forward to the virtual machines behind the jump box.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the  network traffic_____ and system  logs_____.
- What does Filebeat watch for?
Filebeat watches for log data, that then is forwarded (shipped) to Logstash for processing or Elasticsearch for indexing.
- What does Metricbeat record?
Metricbeat records metrics and statistics from an operating system and services running on the server.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function   | IP Address | Operating System |
|----------|----------  |------------|------------------|
| Jump Box | Gateway    | 10.0.1.9   | Linux            |
| ELKStack | ELKstack   | 10.0.1.20  | Linux            |
| DVWA-VM1 | Internal   | 10.0.1.17  | Linux            |
| DVWA-VM2 | Internal   | 10.0.1.16  | Linux            |
| DVWA-VM3 | Internal   | 10.0.1.18  | Linux            |
| DVWA-VM4 | Internal   | 10.0.1.19  | Linux            |
### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Load Balancer_____ machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 51.143.114.40

Machines within the network can only be accessed by _SSH____.
- Which machine did you allow to access your ELK VM? What was its IP address? - JumpBox, 10.0.1.9

A summary of the access policies in place can be found in the table below.

| Name                  | Publicly Accessible | Allowed IP Addresses |
|----------             |---------------------|----------------------|
| Jump-Box-Provisioner 	| No                  | 10.0.1.20   	       |
| ELKStack		          | No		              | 10.0.1.17, 10.0.1.16 |
|			                  |	                    | 10.0.1.18, 10.0.1.19 |
| RedTeamLoadBalancer2  | Yes                 | 10.0.1.17, 10.0.1.16 |
|			                  |	                    | 10.0.1.18, 10.0.1.19 |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- What is the main advantage of automating configuration with Ansible?_
Allows a user to configure multiple VMs with one configuration file.
The playbook implements the following tasks:
- In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
  . install docker io
  . install pip (python pip)
  . install docker python module
  . download and lauch docker ELK Stack

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

[Images>Project1-Docker ps.pdf]

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.1.9
  10.0.1.17
  10.0.1.16
  10.0.1.18
  10.0.1.19

We have installed the following Beats on these machines:
- installed Filebeat and Metricbeat on ELK Stack VM (10.0.1.20)

These Beats allow us to collect the following information from each machine:
- Filebeat collects log data, that then is forwarded (shipped) to Logstash for processing or Elasticsearch for indexing. Filebeat monitors the Elasticsearch log files, collects log events, and ship them to the monitoring cluster. Recent logs are visible on the Monitoring page in Kibana.
- Metricbeat collects metrics and statistics from an operating system and services running on the server. It can be used to monitor and analyze system CPU, memory and load.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the install-elk.yml_____ file to _/etc/ansible (???)____.
- Update the configuration_ file to include.the VMs on which to install or run the stated program.
- Run the playbook, and navigate to Kibana (port 5601)____ to check that the installation worked as expected.

_ Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?
	install-elk.yml. Copy to /etc/ansible directory within Ansible container? (confused by question)
- _Which file do you update to make Ansible run the playbook on a specific machine? the appropriate configuration file
	How do I specify which machine to install the ELK server on versus which to install Filebeat on? To specify the machine to install the ELK server, one will list the machine under the [webservers] heading within the Ansible hosts file. To install Filebeat, one would list the IP address of the specific VM within the filebeat-configuration.yml file.
- _Which URL do you navigate to in order to check that the ELK server is running? One would navigate to the Kibana URL (public IP address of VM, followed by :5601, which is the Kibana port). If Kibana pulls up, the ELK server is running.

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

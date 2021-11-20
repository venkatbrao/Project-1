# DVWA and ELK deployment through Azure VM Based Project
This specific vault will tell you the best way to make a protected and simple arrangement of DVWA and ELK that can be utilized by Cybersecurity understudies and experts

**DVWA & ELK Deployment**
![Network Diagram for Project](https://user-images.githubusercontent.com/94091566/141694829-115a209e-02c1-41de-b8da-4daee0ba5131.jpg)

These documents have been tried and used to create a live DVWA and ELK arrangement on Azure. They can be utilized to either reproduce the whole arrangement presented previously. Then again, select segments of the filebeat and metricbeat playbook record might be utilized to introduce just specific bits of it, like Filebeat. 

Rundown of playbook files and configuration files expected to make it work:

[Ansible configuration]()

[Ansible Hosts]()

[DVWA Playbook Install]()

[ELK Install Playbook]()

[Filebeat Configuration]()

[Metricbeat Configuration]()

[Filebeat playbook]()

[Metricbeat Playbook]()

This document contains the following details:

- Description 
- Access Policies
- Ansible installation to a Jump Host
= DVWA Configuration
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
  - How to Use the Ansible Build


## Description


The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load Balancing guarantees theat the application will be profoundly accessible, as well as confining traffic to the network.

 - Advantage of a load balancer is that it acts as a reverse proxy and distributes network or application traffic across a number of servers. Load balancers are used to increase capacity (concurrent users) and reliability of applications.
 - The motivation behind a SSH jump host server is to be the main entryway for access to your infrastructure reducing the size of any potential attack surface.  Having a dedicated SSH access point also makes it straightforward to have a collected review log of all SSH associations.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.

 - Filebeat is a lightweight shipper for forwarding and centralizing log data. Installed as an agent on your servers, Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.
 - Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash. Metricbeat helps you monitor your servers by collecting metrics from the system and services running on the server, such as: Apache.

The configuration details of each machine may be found below. 

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| Web 1     | Web server         | 10.0.0.5           |    Linux                |
| Web 2    |   Web server       |   10.0.0.6         |     Linux               |
| Web 3    |   Web server       |   10.0.0.7         |       Linux             |
| ELK Server     |    Elastic, Logstash and Kibana Server      |    40.118.229.124/10.0.0.4        |                  |
| RedTeamLB  |  Load Balancer  |  104.211.61.223  | 

## Access Policies

The machines on the internal network are not exposed to the public Internet.

- Only the ELK virtual machine can accept connections from the Internet just to use it as an application server via this IP addresses:
- Machines within the network can only be accessed internally via by Ansible container through SSH port 22 hosted by the Jump Host Server.
- The ELK server can be access via the Ansible container hosted by the Jump Host server using IP address of 10.2.0.4 via SSH port 22.

Ansible was used to automate configuration of the DVWA web servers and ELK machine. No configuration was performed manually, which is advantageous because...
- Ansible Automation increased IT and DevOps agility
- Improved standardization and compliance
- Better control over cost of infrastructure and cloud resources.

A summary of the access policies in place can be found in the table below. All VM access are internal network via SSH port 22.

| Name	| Publicly Accessible |	Allowed IP Addresses |
|------- | ------------- | ---------|
| Jump Box |	Yes |	13.92.7.84 |
| ELK  Server |	No	| 10.2.0.4 |
| Web-1 |	No	| 10.1.0.5 |
|  Web-2 |	No |	10.1.0.6 |
| Web-3	 | No |	10.1.0.7 |

# Ansible installation to a Jump Host
| Generate a public key using your Terminal or Gitbash (SSH-keygen) |
| SSH to your Jump Host VM (SSH username@13.92.7.84) |
| Then you can start installing Ansible. |

 -Install docker.io to your Jump Box

 - Run first this command: sudo apt update

 - Then sudo apt install docker.io

 - Verify that the Docker service is running with this command: sudo systemctl status docker

 - If not running run this command: sudo systemctl status docker

- Once the Docker is installed, pull the container cybersecurity/asnible with this command: sudo docker pull cybersecurity/ansible

- Lunch the ansible container and connect it using the appropriate Docker commands: docker run -ti cyberxsecurity/ansible:latest bash

- You can check if the docker is running by this command: sudo docker container ps -a

- You can also rename the container by this command: sudo docker rename orig_name new_name

- You can attach to ansible container via this command: sudo docker attach container_name

- Once you are in the container you need to run again SSH-keygen and this will be the public key of the internal servers.

- Next, it is important to edit the files host and ansible.cfg in the /etc/ansible directory

First is the hosts file and as you can see I added my VMs.
![Hosts script](https://user-images.githubusercontent.com/94091566/142706967-083b5e48-5f72-4447-8f13-95fdaf018636.jpg)


And the second one is the ansible.cfg file. As you can see here you need to put your VM username

![ansible](https://drive.google.com/file/d/1bJNszM1MoprJOQLBh0yY1SLQK8p6XFGM/view?usp=sharing)

Then it is done. You can now proceed to the DVWA & ELK configuration.

## DVWA Configuration

The DVWA installation playbook implements the following tasks:

As you can see in the below screenshot, the first four lines are important. This will make Ansible know where and what group of servers you want to install and execute the commands for this playbook. As depicted below the first thing that needs to be installed is the docker.io to the DVWA web servers VM.

![](https://drive.google.com/file/d/1fFVUGMrS1QXBb_pKXGhyVNsZv5eBu9Vh/view?usp=sharing)


















 



 




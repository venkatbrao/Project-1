
# DVWA and ELK deployment through Azure VM Based Project

This specific vault will tell you the best way to make a protected and simple arrangement of DVWA and ELK that can be utilized by Cybersecurity understudies and experts

**DVWA & ELK Deployment**
![Network Diagram for Project](https://user-images.githubusercontent.com/94091566/141694829-115a209e-02c1-41de-b8da-4daee0ba5131.jpg)

These documents have been tried and used to create a live DVWA and ELK arrangement on Azure. They can be utilized to either reproduce the whole arrangement presented previously. Then again, select segments of the filebeat and metricbeat playbook record might be utilized to introduce just specific bits of it, like Filebeat. 

Rundown of playbook files and configuration files expected to make it work:

 - Ansible configuration

- Ansible Hosts]

- DVWA Playbook Install

- ELK Install Playbook]

- Filebeat Configuration

- Metricbeat Configuration

- Filebeat playbook

- Metricbeat Playbook

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
![Ansiblecfg](https://user-images.githubusercontent.com/94091566/142734605-374b449b-ba44-4316-a43e-de28a6593f65.jpg)


Then it is done. You can now proceed to the DVWA & ELK configuration.

## DVWA Configuration

The DVWA installation playbook implements the following tasks:

As you can see in the below screenshot, the first four lines are important. This will make Ansible know where and what group of servers you want to install and execute the commands for this playbook. As depicted below the first thing that needs to be installed is the docker.io to the DVWA web servers VM.

![](https://drive.google.com/file/d/1fFVUGMrS1QXBb_pKXGhyVNsZv5eBu9Vh/view?usp=sharing)

- The 2nd part of the of the installation is to install the phython3-pip. Pip is the standard package manager for Python. It allows you to install and manage additional packages that are not part of the Python standard library.

- The 3rd part of the installation is to install the docker module
- After executing the previous task then you can execute below command to download and lunch the DVWA docker container.
- This last part of the playbook is optional but nice to have. This particular task is to automatically start the Ansible docker service if you reboot your VM.
- Below screenshot is the command to use to run the DVWA playbook and what it looks like when executed. If you see red notes it means something is not right and need to correct it. "Ok" message is a good indicator that it is successful.

## ELK Configuration
The ELK installation playbook implements the following tasks:

- As you can see in the below screenshot, it has the same format of commands in the DVWA playbook. You need the difference in the hosts name. Same thing Ansible need to know where and what group of servers you want to install and execute the commands for this playbook. As depicted below the first thing that needs to be installed is the docker.io to the ELK VM.
- Same as the DVWA, the 2nd part of the of the installation is to install the phython3-pip.
- The 3rd part of the installation is same as DVWA installing the docker module
- The 4th part is also very important. ELK needs an ample amount of memory. When you setup your VM it requires minimum of 4GB system memory or else ELK will not run due to lack of system resource. Below screenshot are commands that will assign enough memory for the ELK to function properly
- After executing the previous task you can now download and lunch the ELK docker container. You have to specify the ports that ELK use and the port you will be using to access the ELK in the web. In my case here I use the port 5601
- Same thing as the DVWA, last part of the playbook is optional but nice to have. This command will automatically start the Ansible docker service if you reboot your VM

## Target Machines & Beats
This ELK server is configured to monitor the following machines:

- Web 1 - 10.1.0.5
- Web 2 - 10.1.0.6
- Web 3 - 10.1.0.7
After successfully running this playbook you now have installed the Filebeats and Metricbeats on your machines.

- Filebeats and Metricbeats are now installed to all web servers using the ELK Install playbook. As you can see it seamlessly installed the beats in the three web servers in just one shot.
- What are these Beats for? This will allow us to collect information from each machine that we want to try to monitor and record logs. Below are the screenshot and explain what are the main usage and what is used for.

- Filebeat is a lightweight shipper for forwarding and centralizing log data. Installed as an agent on your servers, Filebeat monitors the log files or locations that you specify, collects log events such as apache logs, website logs, system logs etc. This forwards them either to Elasticsearch or Logstash for indexing. Below is a sample screenshot of Filebeat.
- Metricbeat is a lightweight shipper that you can install on your servers to periodically collect metrics from the operating system and from services running on the server. Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash. It periodically collects metric data from your target servers, this could be operating system metrics such as CPU or memory or data related to services running on the server. It can also be used to monitor other beats and ELK stack itself. Below is a sample screenshot of Metricbea

- Note: In case DVWA ask you for a username and password just type admin as the user and password as the credentials

Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:

SSH into the control node and follow the steps below:

Copy the Filebeat (Filebeat-Playbook.yml) and Metricbeat (Metricbeat-Playbook.yml) playbook files to your /etc/ansible directory. To make it manageable I created a subdirectory "roles" and made it as my repository of playbooks.

Make sure to update the Filebeat (filebeat-config.yml) and Metricbeat (metricbeat-config.yml) configuration files. You need to edit to indicate what specific machine you want to host the Filebeat and Metricbeat app. This configuration files will be copied to the proper Filebeat and Metricbeat folders once you execute their respective playbooks. In my case I created a subdirectory folder named "files" as a repository for my configuration files. Below are the entries that you need to modify on both configuration files before you execute their respective playbook.




















 



 





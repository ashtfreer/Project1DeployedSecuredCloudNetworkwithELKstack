# Project1DeployedSecuredCloudNetworkwithELKstack
The files in this repository were used to configure the network depicted below.
![Network Diagram](https://user-images.githubusercontent.com/59063895/148176035-ca686bf2-d6fb-49e0-a63c-70fd040b265e.jpg)
These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

This document contains the following details:
 - Description of the Topology
 - Acess Policies
 - ELK Configuration
  - Beats in Use
  - Machines being Monitored
 - How to Use the Ansible Build
 
### Description of the Topology
The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load-Balancer
Load balancing ensures that the application will be highly available, in addition to restricting access to the network. A Load Balancer is designed to ensure that the        environment remains available by distributing incoming data to the web servers.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.

Filebeat
Filebeat contains modules that provide observability and secure data sources to monitor the collection, parsing, and visualization of common log formats and translate them to a single command. Automatic default paths are set up based on the operating system, Elasticsearch Ingest Node pipeline definitions, and the Kibana dashboard (Filebeat: Lightweight Log Analysis & Elasticsearch 2021).

Metricbeat
Metricbeat modules collect data from services such as Apache, Jolokia, NGINX, Prometheus, MySQL, and others. These modules can be deployed using docker in a separate container and, once installed, reads information from cgroups and the proc file system without requiring privileged access. Metricbeat is also part of the Elastic Stack and can be shipped to Elasticsearch, Logstash, or Kibana (Metricbeat: Lightweight Shipper for Metrics 2021).

Configuration Details
The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.1.0.4   | Linux            |
| Web-1    | Webserver/DVWA/Docker         | 10.1.0.5   | Linux            |
| Web-2    | Webserver/DVWA/Docker         | 10.1.0.6   | Linux            |
| Web-3    | Webserver/DVWA/Docker         | 10.1.0.7   | Linux            |
| ELKStack | Elk Stack| 10.2.0.4   | LInux            |

### Access Policies
The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
 - Personal Public IP Address

Machines within the network can only be accessed by SSH.
 - The ELK server is accessibly using SSH from JUMP-Box-Provisioner and web acces from the Personal Public IP address.

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | No                  | Personal Public IP Address    |
| Web-1    | Through Load Balancer   | Load Balancer Public IP: 52.237.165.117 & Jump Box: 10.1.0.5     |
| Web-2    | Through Load Balancer   | Load Balancer Public IP: 52.237.165.117 & Jump Box: 10.1.0.6     |
| Web-3    | Through Load Balancer   | Load Balancer Public IP: 52.237.165.117 & Jump Box: 10.1.0.7     |
| ElkStack | No                  | Personal Public IP Address:5601 & Jump Box SSH: 10.2.0.4  |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
 - utomating the installation process allows for deployment to multiple servers simultaneously.

The playbook implements the following tasks:
 1. Installation of docker.io and python3-pip
 2. Virtual Memory increase to 262144
 3. nload and Launches Elk Docker Container
 4. Sets Public Ports

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![ELKdocker](https://user-images.githubusercontent.com/59063895/148257758-04583334-99a5-47be-b4f0-3a276c736158.JPG)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
 - Web-1: 10.1.0.5
 - Web-2: 10.1.0.6
 - Web-3: 10.1.0.7

We have installed the following Beats on these machines:
 - Filebeat
 - Metricbeat

These Beats allow us to collect the following information from each machine:
 - Filebeat allows monitoring of log files and locations and collects log events (Filebeat: Lightweight Log Analysis, Elasticsearch 2021).
 - Metricbeat allows monitoring of statistical data from services running on the server (Metricbeat: Lightweight Shipper for Metrics 2021).

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
1. Copy the filebeat-config.yml file and metricbeat-config.yml to /etc/ansible
2. Update the configuration files to include the Private IP addresses of the ELK server in the ElasticSearch and Kibana Sections of the configuration files.
3. Run the playbook, and navigate to Elk-Server-Public-IP:5601/app/kibana to check that the installation worked as expected.

Which files is the playbook?
 - ELK.yml
 - filebeat-playbook.yml
 - metricbeat-playbook.yml

Where do you copy it?
 - /etc/ansible/

Which file do you update to make Ansible run the playbook on a specific machine?
 - /etc/ansible/hosts

How do I specify which machine to install the ELK servier on versus which to install Filebeat on?
 - Modify /etc/ansible/hosts to differentiate between webserver IP addresses and ELK IP addresses.

Which URL do you navigate to in order to check that the ELK server is running?
13.77.202.108:5601/app/kibana

Specific commands the user will need to run to download the playbook, update the files, etc:

ssh admin_account_name@jumpboxpublicIP
sudo docker ps -a
sudo docker start "dockername"
sudo docker attach "dockername"
cd /etc/ansible
ansible-playbook ./elk.yml (This will install and launch the Elk Server)
ansible-playbook ./filebeat-playbook.yml (This will install Filebeat)
ansible-playbook ./metricbeat-playbook.yml (This will install Metricbeat)
Open a new browser window and navigate to 13.77.202.108:5601/app/kibana (This opens the Kibana web portal)

References Filebeat: Lightweight Log Analysis & Elasticsearch. Elastic. (2021). Retrieved December 18, 2021, from https://www.elastic.co/beats/filebeat

Implementing Secure Administrative Hosts. Microsoft. (2021, July 29). Retrieved December 18, 2021, from https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/implementing-secure-administrative-hosts

Metricbeat: Lightweight Shipper for Metrics. Elastic. (2021). Retrieved December 18, 2021, from https://www.elastic.co/beats/metricbeat

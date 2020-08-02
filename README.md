# Automated Elk Stack Deployment

The files in this repository were used to configure the network depicted below.

![](https://github.com/crflem4518/VU-Bootcamp/blob/master/diagrams/Cloud_Network_Diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Readme file may be used to install only certain pieces of it, such as Filebeat.

* [Elk](../blob/master/ansible/Elk_yml.txt)
* [Filebeat](../blob/master/ansible/Filebeat_install_yml.txt)
* [Metricbeat](../blob/master/ansible/Metricbeat_install_yml.txt)

This document contains the following details:

* Description of the Topology
* Access Policies
* ELK Configuration
* Beats in Use
* Machines Being Monitored
* How to Use the Ansible Build


## Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.
Load balancing ensures that the application will be highly available, in addition to restricting access to the network.

* Load balancers help distribute incoming traffic evenly across the available servers, this functionality would help in the event of a DoS attack. It also helps security by restirciting access to the servers.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the files and system metrics.

Filebeat watches for the changes in the files in the locations that we specify or the log files and then collects and send the data to logstash/elasticsearch.
Metricbeat collects the metric data from the services and the operating system and sends it to logstash/elasticsearch

The configuration details of each machine may be found below.

| Name       | Function                                                       | IP Address    | Operating System        |
|------------|----------------------------------------------------------------|---------------|-------------------------|
| Jump Box   | Gateway w/Ansible Container                                    | 57.137.80.202 | Ubuntu 18.04 Server LTS |
| Web1 VM    | Hosts DVWA container w/Filebeat and Metricbeat                 | 10.1.0.5      | Ubuntu 18.04 Server LTS |
| Web2 VM    | Backup to Web1. Hosts DVWA container w/Filebeat and Metricbeat | 10.1.0.6      | Ubuntu 18.04 Server LTS |
| Elk Server | Hosts the Elk stack                                            | 10.0.0.4      | Ubuntu 18.04 Server LTS |

## Access Policies

The machines on the internal network are not exposed to the public Internet.
Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

* IP Address: 108.88.16.39

Machines within the network can only be accessed by Jumpbox.

The Elk VM is accessible by the Jumpbox. The Jumpbox IP is 57.137.80.202.

A summary of the access policies in place can be found in the table below.


| Name       | Publicly Accessible  | Allowed IP Address |
|------------|:--------------------:|--------------------|
| Jump Box   |          Yes         | 57.137.80.202      |
| Web1 VM    |          No          |         N/A        |
| Web2 VM    |          No          |         N/A        |
| Elk Server |          No          |         N/A        |


## Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

* You can create and configure more machinces more efficiently, you can run the ansible playbook instead of going to every machine and configuring them individually.

The playbook implements the following tasks:

* Install docker.io
* Install PIP
* Install the docker python module
* Download and Install the Elk container
* Run the command to increase memory

## Target Machines & Beats

This ELK server is configured to monitor the following machines:

* Web1 VM (10.1.0.5)
* Web2 VM (10.1.0.6)

We have installed the following Beats on these machines:

* Filebeat and Metricbeat

These Beats allow us to collect the following information from each machine:

* Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing. You could configure Filebeat to capture the logs from wifi.log to monitor who is logging onto the network.
* Metricbeat helps you monitor your servers by collecting metrics from the system and services running on the server. While using Metricbeat, you could monitor CPU usage.

## Using the Playbook

In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:
SSH into the control node and follow the steps below:

* Copy the filebeat-playbook.yml and metricbeat-playbook.yml files to /etc/ansible/roles.
* Update the /etc/ansible/hosts file to include the ip address of the machine under webservers
* Run the playbook, and navigate to http://[your Elk public IP]:5601/app/kibanato check that the installation worked as expected.

As a Bonus, here are the specific commands you will need to run to download the playbook, update the files, etc.

Before running the playbook, make sure that the private IP of the Elk server is added to the ansible hosts file under elkservers group.
* nano /etc/ansible/hosts command is used to edit the hosts file

The below command is used to increase the memory
* sysctl -w vm.max_map_count=262144 . This improves the performance of the Elk server.

For Filebeat configure your playbook with the following: 
* Download the .deb file: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.2-amd64.deb
* Install the .deb file using the dpkg command: sudo dpkg -i filebeat-7.6.2-amd64.deb
* Copy the Filebeat configuration file from your Ansible container to your WebVM: /etc/ansible/files/filebeat.yml /etc/metricbeat/filebeat.yml
* filebeat modules enable system
* filebeat setup
* service filebeat start

For Metricbeat configure your playbook with the following: 
* Download the .deb file: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.6.2-amd64.deb
* Install the .deb file using the dpkg command: sudo dpkg -i metricbeat-7.6.2-amd64.deb
* Copy the Metricbeat configuration file from your Ansible container to your WebVM: /etc/ansible/files/metricbeat.yml /etc/metricbeat/metricbeat.yml
* metricbeat modules enable system
* metricbeat setup
* service metricbeat start

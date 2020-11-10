# Tays-ELK
The files in this repository were used to configure the network depicted below.


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook files may be used to install only certain pieces of it, such as Filebeat.

Web VMs Configuration Playbook: https://github.com/tjjordan214/Tays-ELK/blob/main/Ansible/WebVMs-Playbook.yml
ELK Server Configuration Playbook: https://github.com/tjjordan214/Tays-ELK/blob/main/Ansible/ELKServer-Playbook.yml
Filebeat and Metricbeat Installation Playbook: https://github.com/tjjordan214/Tays-ELK/blob/main/Ansible/Filebeat_Metricbeat-Playbook.yml
This document contains the following details:

Description of the Topology
Access Policies
ELK Configuration
Beats in Use
Machines Being Monitored
How to Use the Ansible Build
Description of the Topology
The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.

What aspect of security do load balancers protect? Load balancers protect the availability aspect of the CIA Triad.
What is the advantage of a jump box? A Jump Box can be used in front of other machines in a network that are not exposed to the public internet. It controls access to these machines and eliminates having to interact with all of them. Updates for the other machines can be made all at once through the jump box.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the files and system logs.

What does Filebeat watch for? File system changes
What does Metricbeat record? Machine metrics
The configuration details of each machine may be found below.

Name	Function	IP Address	Operating System
Jump Box	Gateway	10.0.0.4	Linux
Web-1	Web Server	10.0.0.8	Linux
Web-2	Web Server	10.0.0.9	Linux
Web-3	Web Server	10.0.0.10	Linux
Load Balancer	Traffic Distributor	52.188.8.199	
ELK Server	Network Monitor	10.1.0.4	Linux
Access Policies
The machines on the internal network are not exposed to the public Internet. Only the Jump Box and Load Balancer can accept connections from the Internet. Access to these “devices” are only allowed from the following IP addresses:

108.4.112.242 can access the Jump Box
HTTP web traffic can access the Load Balancer
Machines within the network can only be accessed by:

SSH connection from the jump box via Port 22
HTTP traffic from the load balancer via Port 80
Which machine did you allow to access your ELK VM? Local Machine

What was its IP address? 108.4.112.242
A summary of the access policies in place can be found in the table below.

Name	Publicly Accessible	Allowed IP Addresses
Jump Box	Yes	108.4.112.242
Web-1	No	10.0.0.4 52.188.8.199
Web-2	No	10.0.0.4 52.188.8.199
Web-3	No	10.0.0.4 52.188.8.199
Load Balancer	Yes	Any
ELK Server	Yes	108.4.112.242
Elk Configuration
Ansible was used to automate the configuration of the ELK machine. No configuration was performed manually, which is advantageous because it makes the configuration of a new machine much easier and faster.

The playbook implements the following tasks:

Install Docker
Install Python Pip3
Install Docker Python Module
Update vm.max_map_count to 262144
Download and launch the ELK docker container
The following screenshot displays the result of running docker ps after successfully configuring the ELK instance.
‘docker ps’ Results alt text

Target Machines & Beats
This ELK server is configured to monitor the following machines:

10.0.0.8
10.0.0.9
10.0.0.10
We have installed the following Beats on these machines:

Filebeat
Metricbeat
These Beats allow us to collect the following information from each machine:

“Filebeat helps generate and organize log files to send to Logstash and Elasticsearch. It logs information about the file system, including which files have changed and when they changed.”

Filebeat Image alt text
Metricbeat collects metrics and statistics from the operating system and sends them to Logstash and Elasticsearch. It records data such as memory usage.

Metricbeat Image alt text
Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: SSH into the control node and follow the steps below:

Copy the Filebeat Configuration file to /etc/ansible/files/filebeat-configuration.yml.
Update the Filebeat Configuration file to include the private IP address of the virtual machine that has been configured to run Elk.
Run the playbook, and navigate to http://[ELK_SERVER_PUBLIC_IP_ADDRESS]:5601/app/kibana to check that the installation worked as expected.
Q&A:

Which file is the playbook? filebeat-playbook.yml

Where should it be located? /etc/ansible/roles directory

Which file is the configuration file?

Filebeat Configuration file: https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat
Metricbeat Configuration file: https://gist.githubusercontent.com/slape/58541585cc1886d2e26cd8be557ce04c/raw/0ce2c7e744c54513616966affb5e9d96f5e12f73/metricbeat
Where do you copy it?

Copy the Filebeat config file to /etc/ansible/files/filebeat-configuration.yml
Copy the Metricbeat config file to /etc/ansible/files/metricbeat-configuration.yml
Which file do you update to make Ansible run the playbook on a specific machine? /etc/ansible/hosts file

How do I specify which machine to install the ELK server on versus which to install Filebeat on?

The machine that is being configured can be specified in the hosts file. The private IP address of the machine should be added to the ‘elk’ group to install Elk, or to the ‘webservers’ group to install Filebeat.

[webservers]

10.0.0.8 ansible_python_interpreter=/usr/bin/python3
10.0.0.9 ansible_python_interpreter=/usr/bin/python3
10.0.0.10 ansible_python_interpreter=/usr/bin/python3
[elk]

10.1.0.4 ansible_python_interpreter=/usr/bin/python3
Which URL do you navigate to in order to check that the ELK server is running? http://[ELK_SERVER_PUBLIC_IP_ADDRESS]:5601/app/kibana
Commands and Instructions:
Command to copy Filebeat Configuration file to Ansible container:

curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/files/filebeat-configuration.yml
Command to edit Filebeat Configuration file:

vi /etc/ansible/files/filebeat-configuration.yml
Edit configuration file by scrolling to line #1106 and replacing the IP address with the private IP of the Elk Machine.

output.elasticsearch:
hosts: ["10.1.0.4:9200"]
username: "elastic"
password: "changeme"
Scroll to line #1806 and replace the IP address with the private IP of the Elk Machine.
Setup.kibana:
host: "10.1.0.4:5601"
Save and exit

Command to copy the Metricbeat Configuration file to Ansible container:

curl https://gist.githubusercontent.com/slape/58541585cc1886d2e26cd8be557ce04c/raw/0ce2c7e744c54513616966affb5e9d96f5e12f73/metricbeat > /etc/ansible/files/metricbeat-configuration.yml
Command to edit Metricbeat Configuration file:

vi /etc/ansible/files/metricbeat-configuration.yml
Make the same edits to this file that was made to the Filebeat configuration file.
Command to run the playbook:

ansible-playbook /etc/ansible/roles/filebeat-playbook.yml

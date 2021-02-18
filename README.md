# My-Cyber-Projects This is my initial attempt at creating a project repository
The files in this repository were used to configure the network diagram depicted below.

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the [install-elk.yml] file may be used to install only certain pieces of it, such as Filebeat.
-filebeat-playbook.yml, metricbeat-playbook.yml
name: Installing and launching filebeat hosts: elk become: yes tasks:
Use command module
name: Download filebeat deb command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb
Use command module
name: Install filebeat deb command: dpkg -i filebeat-7.6.1-amd64.deb
Use copy module
name: Drop in filebeat.yml copy: src: /etc/ansible/files/filebeat-config.yml dest: /etc/filebeat/filebeat.yml
Use command module
name: Enable and configure system module command: filebeat modules enable logstash
Use command module
name: Setup filebeat command: filebeat setup
Use command module
name: Start filebeat service command: sudo service filebeat start
This document contains the following details:
Description of the Topology
Access Policies
ELK Configuration
Beats in Use
Machines Being Monitored
How to Use the Ansible Build
Description of the Topology
The objective of this network is to display a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.
Load balancing makes certain the application will be extremely available, while also restricting traffic to the network. The aspect of security that load balancers protect is availability.
The advantage of a jump box is that you can have a virtual machine on your host.
Integrating an ELK server allows users to monitor the vulnerable VMs for changes to the logs and system traffic in an easier way. Filebeat watches for forwarding and centralizing log data. Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing when it’s installed as an agent on your servers.
Metricbeat is a lightweight shipper that you can install on your servers to periodically record metrics from the operating system and from services running on the server. Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.
The following table includes the configuration details of each machine.
 Note: Use the Markdown Table Generator to add/remove values from the table.
Name
Function
IP Address
Operating System
Jump Box
Gateway server
10.1.1.4
Linux
Web 1
Ubuntu server
10.1.1.5
Linux
Web 2
Ubuntu server
10.1.1.6
Linux
ElkVM
Gatway server
10.2.0.4
Linux
Access Policies
The machines on the internal network are secured from the public Internet.
Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
108.36.152.86
Machines within the network can only be accessed by Jump Box via SSH. Which machine did you allow to access your ELK VM? Jump Box What was its IP address? 10.1.1.4
A summary of the access policies in place can be found in the table below.
Name
Publicly Accessible
Allowed IP Addresses
Jump Box
Yes
52.191.160.230
Web 1
No
10.1.1.4
Web 2
No
10.1.1.5
Elk Configuration
Ansible was utilized in order to automate configuration of the ELK machine. The configuration was not performed manually. The main advantage of automating configuration with Ansible is that you can put commands from/to multiple servers in a single playbook.
The playbook implements the following tasks:
ssh into the ELK machine, install docker, create docker container, start docker container, attach, create yaml config, create playbook, run playbook
The following screenshot shows the result of running docker ps after successfully configuring the ELK instance. 
root@ELK-vNet2:/home/azadmin# docker ps CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES f450012e52ce sebp/elk:761 "/usr/local/bin/star…" 3 days ago Up 3 hours 0.0.0.0:5044->5044/tcp, 0.0.0.0:5601->5601/tcp, 0.0.0.0:9200->9200/tcp, 9300/tcp elk
Target Machines & Beats
This ELK server is configured to monitor the following machines: List the IP addresses of the machines you are monitoring_ Web 1 10.1.1.5 Web 2 10.1.1.6 ELK 10.2.0.4
We have installed the following Beats on these machines: Specify which Beats you successfully installed_metric and filebeat
These Beats allow us to collect the following information from each machine: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., Winlogbeat collects Windows logs, which we use to track user logon events, etc._ filebeat collects changes done and collects metrics and statistics
Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:
SSH into the control node and follow the steps below:
Copy the curl file to the playbook.
Update the config file to include the ip address
Run the playbook, and navigate to kibana to check that the installation worked as expected.
Answer the following questions to fill in the blanks:_
Which file is the playbook? Where do you copy it? root@12988e292a61:/etc/ansible/roles# cat metricbeat-playbook.yml
_Which file do you update to make Ansible run the playbook on a specific machine? hosts file How do I specify which machine to install the ELK server on versus which to install Filebeat on?_update IP address on host file
_Which URL do you navigate to in order to check that the ELK server is running? http://20.186.38.101:5601/app/kibana

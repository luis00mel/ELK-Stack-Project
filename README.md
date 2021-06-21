## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

Inline-style:
![alt text](https://github.com/luis00mel/ELK-Stack-Project/tree/main/Images/ElkStack.PNG "Elk-Stack-Typology")

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the install-elk.yml file may be used to install only certain pieces of it, such as Filebeat.


This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly accessable, in addition to restricting the flow to the network. A jumpbox ensures security, automation, and network segmentation. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
What does Filebeat watch for? Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.
What does Metricbeat record? Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name                 | Function       | IP Address                | Operating System |
|----------------------|----------------|---------------------------|------------------|
| Jump-Box-Provisioner | Gateway        | 10.0.0.4/23.101.137.50    | Linux            |
| Web-1                | Web Server     | 10.0.0.5                  | Linux            |
| Web-2                | Web Server     | 10.0.0.6                  | Linux            |
| DVWA-VM1             | Web Server     | 10.0.0.7                  | Linux            |
| Elk-1                | ELK Server     | 10.1.0.4/137.135.48.201   | Linux            |
| Load Balancer        | Load Balance   | 52.152.221.243            | Linux            |
| Workstation          | Access Control | XXX.194.33.25             | Windows          |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Elk Server machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: [XXX.194.33.25] thought TCP 5601

Machines within the network can only be accessed by my Workstation and Jump-Box-Provisioner [23.101.137.50]. 
Which machine has access to ElkVM?
-Jump-Box-Provisioner IP : 10.0.0.4 via SSH port 22
-Workstation Public IP : port TCP 5601

A summary of the access policies in place can be found in the table below.

| Name                 | Publicly Accessible | Allowed IP Addresses                 |
|----------------------|---------------------|--------------------------------------|
| Jump-Box-Provisioner | No                  | Workstation Public IP on SSH 22      |
| Web-1                | No                  | 10.0.0.4 on SSH 22                   |
| Web-2                | No                  | 10.0.0.4 on SSH 22                   |
| DVWA-VM1             | No                  | 10.0.0.4 on SSH 22                   |
| Elk-1                | No                  | Workstation Public IP using TCP 5601 |
| Load Balancer        | No                  | Workstation Public IP on HTTP 80     |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because ansible lets you deploy multiple applications quickly and efficiently. No custom is code needed to automate the systems as it can be listed on a playbook all the necessary tasks required on the machine.

Specify a different group of machines with different remote users

The playbook implements the following tasks:
-   name: Config elk VM with Docker
    hosts: elk
    remote_user: sysadmin
    become: true
    tasks:

Increase system memory:

- name: Use more memory
  sysctl:
    name: vm.max_map_count
    value: '262144'
    state: present
    reload: yes

To download a file using curl

-   name: Download filebeat .deb file
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

(Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1 : 10.0.0.5
- Web-2 : 10.0.0.6

We have installed the following Beats on these machines:
- Elk-1, Web-1, and Web-2
- The ELK Stack Installed are: Filebeat and MetricBeat

These Beats allow us to collect the following information from each machine:
- Filebeat : Collects log events on the traffic and user logins through the site.
- Metricbeat : Collects metric and system statistics which are then put into charts to view website analytics.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the _____ file to _____.
- Update the _____ file to include...
- Run the playbook, and navigate to ____ to check that the installation worked as expected.

For ELK VM Configuration:

  Copy the Ansible ELK Installation and VM Configuration
  Run the playbook using this command : ansible-playbook install-elk.yml

For FILEBEAT:

Download Filebeat playbook in : [Asible/filebeat-config.yml]

Copy the '/etc/ansible/files/filebeat-config.yml' file to '/etc/filebeat/filebeat-playbook.yml'
Update the filebeat-playbook.yml file to include installer
curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb
Update the filebeat-config.yml file : 
	root@c1e0a059c0b0:/etc/ansible/files# nano filebeat-config.yml

output.elasticsearch:

  #Array of hosts to connect to.
    hosts: ["10.1.0.4:9200"]
      username: "elastic"
      password: "changeme” 

   setup.kibana:
    host: "10.1.0.4:5601"

Run the playbook using this command ansible-playbook filebeat-playbook.yml and navigate to Kibana > Logs : Add log data > System logs > 5:Module Status > Check the data and confirm the installation.

For METRICBEAT:

Download Metricbeat playbook using this command: [Asible/metricbeat-config.yml]

Copy the /etc/ansible/files/metricbeat file to /etc/metricbeat/metricbeat-playbook.yml
Update the filebeat-playbook.yml file to include installer
curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.6.1-amd64.deb
Update the metricbeat file rename to metricbeat-config.yml
	root@c1e0a059c0b0:/etc/ansible/files# nano metricbeat-config.yml

output.elasticsearch:

  #Array of hosts to connect to.
  hosts: ["10.1.0.4:9200"]
    username: "elastic"
    password: "changeme"

   setup.kibana:
    host: "10.1.0.4:5601"

Run the playbook, (ansible-playbook metricbeat-playbook.yml) and navigate to Kibana > Add Metric Data > Docker Metrics > Module Status to check that the installation worked as expected.

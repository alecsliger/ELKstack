## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

!(Images/diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

  - elk-playbook.yml
  - metricbeat.yml
  - filebeat.yml

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly stable, in addition to restricting traffic to the network.
- Load balancers protect the availability of a resource
- Using a jump box limits the network to one way in for administration purposes, therefore less surface to work with from an attacker's perspective

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
- Filebeat collects and parses logs created by the system logging service of Unix/Linux based distributions
- Metricbeat collects CPU, memory, network, and disk statistics from the host. It collects system wide statistics and statistics per process and filesystem.
 (source: Directly from description in Kibana)

The configuration details of each machine may be found below.

| Name      | Function   | IP Address | OS    |
|-----------|------------|------------|-------|
| Jump Box  | Gateway    | 10.0.0.4   | Linux |
| Web-1     | Webserver  | 10.0.0.5   | Linux |
| Web-2     | Webserver  | 10.0.0.6   | Linux |
| ElkServer | Monitoring | 10.1.0.4   | Linux |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jump box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- HostMachineIPv4

Machines within the network can only be accessed by the ansible container on the jump box.
- 10.0.0.4

A summary of the access policies in place can be found in the table below.

| Name      | Publicly Accessible | Allowed IP Addresses |
|-----------|---------------------|----------------------|
| Jump Box  | No                  | HostIPv4             |
| Web-1     | No                  | 10.0.0.4, HostIPv4   |
| Web-2     | No                  | 10.0.0.4, HostIPv4   |
| ElkServer | No                  | 10.0.0.4, HostIPv4   |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Ansible allows the execution of playbooks, that therein allowing multiple machines to be configured with a single .yml file and subsequent command

The playbook implements the following tasks:
- Installs Docker
- Installs Python3-pip
- Raises available memory utilized
- Downloads and launches ELK container
- Enable docker with systemd on boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

!(Images/screenshot1.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5
- 10.0.0.6

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
Again:
- Filebeat collects and parses logs created by the system logging service of Unix/Linux based distributions
  - some example log files being from the directory /var/log/ as a *.log
- Metricbeat collects CPU, memory, network, and disk statistics from the host. It collects system wide statistics and statistics per process and filesystem. 
  - metricbeat displays information collected as a graph of % in use on the kibana dashboard, similar to windows task manager 
### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbook files to /etc/ansible.
- Update the ansible config file to include the remote user, and the hosts file to include the IP addresses of the webservers you want to monitor
- Run the playbook, and navigate to your webservers via ssh to check that the installation worked as expected by running a 'docker ps'.

- The playbook file to deploy elk is elk-playbook.yml
- Update the line near the top of the playbook file that references 'elk' with the group of IP addresses you designated in the ansible hosts file (should just be one) 
- When running the '-beat' playbooks, make sure to ensure that the 'webservers' group in the hosts file has the IP addresses of the server(s) you want to monitor
- Navigate to http://[your.ELK-VM.External.IP]:5601/app/kibana to test if your elk stack is online


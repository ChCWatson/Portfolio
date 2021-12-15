# ELK-STACK-PROJECT
UC-Davis Elk-Stack-Project

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![](https://github.com/Plampking/ELK-STACK-PROJECT/blob/main/Diagrams/HW%20Diagram.jpg)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

  - _TODO: Enter the playbook file._

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
Load blancers help a environment's availability through distribution of incoming fata to web servers. 
The Jump Box provides a more efffect method to manage administration of multiple systems.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the event logs and system metrics.
- Filebeat watches for log directories or specific log files.
- Metricbeat helps monitor servers by collecting metrics from the system and services running within the server.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.1   | Linux            |
| Web-1    | Server  | 10.0.0.8    | Linux            |
| Web-2    | Server  | 10.0.0.9    | Linux            |
| Elk-Stack| Log Server | 10.0.1.5 | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box-Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
 - Personal Host IP Address

Machines within the network can only be accessed by the Jump-Box-Provisioner.
The Jump-Box-Provisioner is able to connect via SSH to the Elk-Stack machine throught port 22. In addition the Personal Host IP address is also about to access the Elk-Stack machine through port 5601

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes              | Personal IP    |
|Web-1          | No                    |  10.0.0.8                    |
|Web-2          | No                    |  10.0.0.9                    |
|Elk-Stack          | Yes                    | Personal IP, 10.0.0.1                     |
|Load Balancer          | Yes                    | Open*                     |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because system installation and updates can be done efficiently. 

The playbook implements the following tasks:
```bash
 - name: Instal Packages
   hosts: elk
   become: true
   tasks:
   - name: set maximum map count in sysctl/systemd
     sysctl:
       name: vm.max_map_count
       value: 262144
       state: present
       reload: yes
   - name: docker.io
     apt:
       force_apt_get: yes
       update_cache: yes
       name: docker.io
       state: present
   - name: instal pip3
     apt:
       force_apt_get: yes
       name: python3-pip
       state: present
   - name: Install Docker module
     pip:
      name: docker
      state: present
   - name: download and launch elk
     docker_container:
       name: elk
       image: sebp/elk:761
       state: started
       restart_policy: always
       published_ports:
         - 5601:5601
         - 9200:9200
         - 5044:5044
    # Use command module
   - name: Increase virtual memory
     command: sysctl -w vm.max_map_count=262144
    # Use shell module
   - name: Increase virtual memory on restart
     shell: echo "vm.max_map_count=262144" >> /etc/sysctl.conf
```


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![](https://github.com/Plampking/ELK-STACK-PROJECT/blob/main/Images/docker%20ps.JPG)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1 (10.0.0.8)
- Web-2 (10.0.0.9)

We have installed the following Beats on these machines:
- FileBeat
- Metric Beat

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the configuration file to ELK container.
- Update the host file to include the Elk containers IP address.
- Run the playbook, and navigate to Kibana http://[Elk IP address]5601/app/kibana to check that the installation worked as expected.

Commands in order to download the playbook, update the files, etc.

Commands for installing Filebeat playbook
```bash
---
- name: installing and launching filebeat
  hosts: webservers
  become: yes
  tasks:

  - name: download filebeat deb
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb

  - name: install filebeat deb
    command: dpkg -i filebeat-7.4.0-amd64.deb

  - name: drop in filebeat.yml
    copy:
      src: /etc/ansible/files/filebeat-config.yml
      dest: /etc/filebeat/filebeat.yml

  - name: enable and configure system module
    command: filebeat modules enable system

  - name: setup filebeat
    command: filebeat setup

  - name: start filebeat service
    command: service filebeat start

  - name: enable service filebeat on boot
    systemd:
      name: filebeat
      enabled: yes

```
Commands for Install Metricbeat playbook.
```bash
- name: Install metric beat
  hosts: webservers
  become: true
  tasks:
  - name: Download metricbeat
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.4.0-amd64.deb

  - name: install metricbeat
    command: dpkg -i metricbeat-7.4.0-amd64.deb

  - name: drop in metricbeat config
    copy:
      src: /etc/ansible/files/metricbeat-config.yml
      dest: /etc/metricbeat/metricbeat.yml

  - name: enable and configure docker module for metric beat
    command: metricbeat modules enable docker

  - name: setup metric beat
    command: metricbeat setup

  - name: start metric beat
    command: service metricbeat start
```

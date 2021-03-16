## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Elk](https://github.com/JasonTorre/Cybersecurity-MyProject-1/blob/main/Diagrams/Elk%20Stack%20Diagram.jpg)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Azure cloud environment file may be used to install only certain pieces of it, such as Filebeat.

 ```
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
 

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly dependable, in addition to restricting access to the network. 

- Load balancers distribute traffic evenly and prevents one machine from being overwhelmed. This action protects the makes load balancers an effective security tool against DoS attacks. Load balancers are also extremely useful when a machine fails, any other machine in the back end pool can pick up the slack without any interuption of service noticeable by the end user.

- The advantage of using a Jump-Box is to limit and restrict the access to a single point or one administrator.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the actual machine and system logs.
-  Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.
- Metricbeat is a lightweight shipper that you can install on your servers to periodically collect metrics from the operating system and from services running on the server. Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.

The configuration details of each machine may be found below.

| Name     | Function         | IP Address | Operating System |
|----------|------------------|------------|------------------|
| Jump-Box | Gateway          | 10.0.0.8   | Linux            |
| Web-1    | DVWA Containers  | 10.0.0.9   | Linux            |
| Web-2    | DVWA Containers  | 10.0.0.10  | Linux            |
| Web-3    | DVWA Containers  | 10.0.0.11  | Linux            |
| Elk      | Configuration VM | 10.1.0.5   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

- 104.34.75.64 - My home IP address

Machines within the network can only be accessed by through the DVWA container in the Jump Box VM.
- I allowed only my Jump-Box VM IP 10.0.0.8 using a DVWA Conatainer through a peering connetion and my home IP 104.34.75.64 permision to access The Elk Server. 

A summary of the access policies in place can be found in the table below.

| Name        | Publicly Accessible | Allowed IP Addresses  |
|-------------|---------------------|-----------------------|
| Jump-Box VM | Yes                 | 104.34.75.64          |
| Web-1       | No                  | 10.0.0.8              |
| Web-2       | No                  | 10.0.0.8              |
| Web-3       | No                  | 10.0.0.8              |
| Elk-1       | Yes                 | 104.34.75.64/10.0.0.8 |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it allows admins to automate daily mundane task. This frees them up to focus on more important business or troubleshoot other issues. Also, using Ansible to configure machines reduces errors.

The playbook implements the following tasks:

- Install docker
- Download image
- Configure ansible container
- Create anible playbook
- Run ansible playbook to launch container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![Docker](https://github.com/JasonTorre/Cybersecurity-MyProject-1/blob/main/Pictures/Docker_PS.PNG)

### Target Machines & Beats

This ELK server is configured to monitor the following machines:
- 10.0.0.9
- 10.0.0.10
- 10.0.0.11

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat monitors the log files of the choosen system, and forwards them to Elasticsearch or Logstash. The Data can then be viewed and it will represent data such as system log events in a dashboard:

![Filebeat](https://github.com/JasonTorre/Cybersecurity-MyProject-1/blob/main/Pictures/Filebeats.PNG)

- Metricbeat collects metrics from the operating system and from services running on the server. The metrics and statistics that it collects and is sent to the output that you specify, such as Elasticsearch or Logstash and can be displayed on a dashboard in Kibana, such as below.

![Metricbeat](https://github.com/JasonTorre/Cybersecurity-MyProject-1/blob/main/Pictures/MetricBeat.PNG)


### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

```curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/filebeat-config.yml```

SSH into the control node and follow the steps below:
- Copy the filebeat-playbook.yml file to /etc/ansible/roles.
- Update the filebeat-config.yml file to include ELK-1 private IP 10.1.0.5 (Your IP will vary) line 1106 and 1806.
- Run the playbook, and navigate to [ELK public IP]/app/kibana to check that the installation worked as expected.


### Below is he specific commands the user will need to run to download the playbook, update the files, etc.

- ssh into Jump-Box VM: ```ssh [username]@[IP_Address]```

Install, setup and launch docker using the commands below
- ```sudo apt install docker.io```
- ```sudo docker pull cyberxsecurity/ansible```
- ```sudo docker run -ti cyberxsecurity/ansible:latest bash```
The command will create and start a container. To find the name of your container run the command below.
- ```sudo docker container list -a```

When starting your conatianer you would use the following command below.
- ``` sudo docker start (Name) && sudo docker attach (Name)```

You would then need to edit the host files.
 1. ```nano /etc/ansible/hosts```
 2. Uncomment the [webservers] header
 3. Under the webservers header add the internal IP addresses of the web VM's and add the python script to the end. The line should look like ```10.0.0.6 ansible_python_interpreter=/usr/bin/python3```
 
Then edit the config files.
 1.  ```nano /etc/ansible/ansible.cfg```
 2. On line 106, change remote user to ```remote_user = [username]```
 
Create the Playbook and Configure the Container.
- ```touch pentest.yml```
- ```nano pentest.yml```

In the nano text editor add the following so the playbook reads:
```---
- name: Config Web VM with Docker
  hosts: webservers
  become: true
  tasks:
  - name: docker.io
    apt:
      force_apt_get: yes
      update_cache: yes
      name: docker.io
      state: present

  - name: Install pip3
    apt:
      force_apt_get: yes
      name: python3-pip
      state: present

  - name: Install Docker python module
    pip:
      name: docker
      state: present

  - name: download and launch a docker web container
    docker_container:
      name: dvwa
      image: cyberxsecurity/dvwa
      state: started
      restart_policy: always
      published_ports: 80:80

  - name: Enable docker service
    systemd:
      name: docker
      enabled: yes
```
Save and exit the file and then run the playbook using the following command.

- ```ansible-playbook /etc/ansible/pentest.yml```

The final results should look similar to below.

![Playbook](https://github.com/JasonTorre/Cybersecurity-MyProject-1/blob/main/Pictures/Playbook.PNG)



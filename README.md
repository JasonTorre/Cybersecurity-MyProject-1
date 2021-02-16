## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Elk](https://github.com/JasonTorre/Cybersecurity-MyProject-1/blob/main/Diagrams/Elk%20Stack%20Diagram.jpg)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Azure clould envirment file may be used to install only certain pieces of it, such as Filebeat.

  - _TODO: Enter the playbook file._

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

- Metricbeat collects metrics from the operating system and from services running on the server. The metrics and statistics that it collects and is sent to the output that you specify, such as Elasticsearch or Logstash and can be distplayed on a dashboard such as below.
[Metricbeat](https://github.com/JasonTorre/Cybersecurity-MyProject-1/blob/main/Pictures/MetricBeat.PNG)


### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-playbook.yml file to /etc/ansible/roles.
- Update the _____ file to include...
- Run the playbook, and navigate to ____ to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

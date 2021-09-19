ELK-Stack-Project
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Diagram](Diagrams/NetworkDiagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML file may be used to install only certain pieces of it, such as Filebeat.

Install Files:<br />
[Ansible Playbook](Ansible/myplaybook.yml)<br />
[Elk Install](Ansible/install_elk.yml)<br />
[Metricbeat.yml](Ansible/metricbeat.yml)<br />
[Filebeat.yml](Ansible/filebeat.yml)<br />

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting in-bound access to the network.

What aspect of security do load balancers protect?<br /> 
- A load balancer effectively minimizes response time and enables a way to layer security without any changes to our applications. The layers of protection mean the load balancers can authenticate user access, protect against denial-of-service attacks, and protect the applications from emerging threats. Since the load balancer sits between the client device and the backend server, they are also vital in helping to ensure that no single server becomes unreliable due to being overworked.

What is the advantage of a jump box?
- The jump box is a server that is accessible from the internet and provides us with a way to access other  machines on a private network via an SSH protocol. It is used to access and also manage devices that are in separate security zones. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the jump box and system network.
- What does Filebeat watch for?<br />Filebeat is a logging agent, it monitors the log files on our servers, looking for any changes in files, collects log events and inputs them into ELK Stack for analysis.
- What does Metricbeat record?<br />Metricbeat helps keep us informed by collecting metrics and information on the services we run on a server and those of the operating system. It then takes the information collected and puts them into a server-side data processing pipeline such as Elasticsearch or Logstash.

The configuration details of each machine may be found below.

| Name       | Function   | IP Address | Operating System |
|------------|------------|------------|------------------|
| Jump Box   | Gateway    | 10.0.0.4   | Linux            |
| Web-1      | Webserver  | 10.0.0.5   | Linux            |
| Web-2      | Webserver  | 10.0.0.6   | Linux            |
| ELK-Server | Monitoring | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jump box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- My personal machine, IP: 70.181.XXX.XXX

Machines within the network can only be accessed by the Jump Box, which has the IP address: 10.1.0.4.

Machines allowed to access the ELK Server is the home machine, IP 70.181.XXX.XXX, via port 5601.

A summary of the access policies in place can be found in the table below.
| Name       | Publicly Accessible | Allowed IP Addresses |
|------------|---------------------|----------------------|
| Jump Box   | Yes                 | 70.181.XXX.XXX       |
| Web-1      | No                  | 10.0.0.4             |
| Web-2      | No                  | 10.0.0.4             |
| ELK-Server | No                  | 10.0.0.4             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

- Ansible performs all functions over OpenSSH, and it is straightforward when it comes to use.
- It is run from the command line without the use of configuration files for simple tasks.
- It is self-documenting.
- Tasks are executed in order.
- It is a free, open-source tool, which does not require budget allocations.

A total of three playbooks were installed, each implemented:

Install ELK Playbook<br />
- Install docker.io package using apt
- Install python3-pip package manager using apt
- Install the docker module using pip
- Configure the VM to use more memory using the sysctl module
- Download and launch the docker container for the ELK stack

Install Filebeat Playbook<br />
- Download and install Filebeat
- Copy Filebeat configuration
- Enable Filebeat system module
- Setup Filebeat
- Start and enable Filebeat service

Install Metricbeat Playbook<br />
- Download and install Metricbeat
- Copy Metricbeat configuration
- Enable Metricbeat docker module
- Setup Metricbeat
- Start and enable Metricbeat service

- The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.
Docker ps output

- ![Docker Output](Images/Container.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:

| Name  | IP Addresses |
|-------|--------------|
| Web-1 | 10.0.0.5     |
| Web-2 | 10.0.0.6     |


We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat: Allows us to observe and simplify the way we collect and visualize logs such as: login attempts, failed processes, and other errors.
![Filebeat](Images/Filebeat.png)

- Metricbeat: Collects the metrics of the operating system and the running services. We are able to view information on such things as CPU usage and load.
![Metricbeat](Images/Metricbeat.png)

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the three playbook files or use nano to create the (install_elk.yml, filebeat.yml, and metricbeat.yml) to the /etc/ansible/roles directory on your control node. 
- Update the /etc/ansible/hosts file. You will need to include a group called elkservers which contains the IP address of the server you wish to install the ELK stack on.
- Create a group called webservers with the IP addresses of the target machines you wish to monitor.
- The [webservers] group should have your Web 1 and Web 2 machines on it, while [elk] should have the ELK vm.

Here is a sample hosts file configuration of the Ansible inventory:
cd /etc/ansible<br />
cat hosts <br />
[webservers]<br />
10.0.0.4 ansible_python_interpreter=/usr/bin/python3<br />
10.0.0.5 ansible_python_interpreter=/usr/bin/python3<br />
10.0.0.6 ansible_python_interpreter=/usr/bin/python3<br />

[elk]<br />
10.1.0.4 ansible_python_interpreter=/usr/bin/python3<br />

- Run the playbook, and navigate to Kibana ((http://[ELK VM PUBLIC IP:5601]/app/kibana#/home) to verify the installation was successful.


To run the above playbooks, do the following:

- Ensure the destination directories exist<br />
$ mkdir -p /etc/ansible/roles/files<br />

- Update the /etc/ansible/hosts file with appropriate configuration<br />
$ cat /etc/ansible/hosts<br />

- Run the ELK installation playbook<br />
$ ansible-playbook /etc/ansible/install_elk.yml<br />

- Run the Filebeat installation playbook<br />
$ ansible-playbook /etc/ansible/filebeat.yml<br />

- Run the Metricbeat installation playbook<br />
$ ansible-playbook /etc/ansible/metricbeat.yml<br />


To check the ELK server is running:
http://[Host IP]/app/kibana#/home


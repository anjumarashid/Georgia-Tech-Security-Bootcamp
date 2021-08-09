# Georgia-Tech-Security-Bootcamp
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted in \Georgia-Tech-Security-Bootcamp\HW13-Elk-Stack-Project\Images\Elk_Deployment.png.

- Georgia-Tech-Security-Bootcamp\HW13-Elk-Stack-Project\Ansible\install-elk.yml
- Georgia-Tech-Security-Bootcamp\HW13-Elk-Stack-Project\Ansible\filebeat-playbook.yml
- Georgia-Tech-Security-Bootcamp\HW13-Elk-Stack-Project\Ansible\metricbeat-playbook.yml

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured \Images\Elk_Deployment.png. Alternatively, select portions of the filebeat-playbook.yml file may be used to install only certain pieces of it, such as Filebeat.

  - install-elk.yml
  - filebeat-playbook.yml
  - metricbeat-playbook.yml
  - pentest.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
Jump box added layer of security since this is the only host that needs to be deployed in front of Internet. Internal hosts connect SSH/connectivity requests only from Jumpbox.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file system and system resources like CPU and RAM.

The configuration details of each machine may be found below.

| Name     | Function		 | IP Address | Operating System 	|
|----------|-----------------|------------|---------------------|
| Jump Box | Gateway  		 | 10.0.0.4   | Linux ubuntu 18.04  |
| Elk-vm   | Elk Server      | 10.1.0.4   | Linux ubuntu 18.04  |
| Web-1    | Web Server    	 | 10.0.0.5   | Linux ubuntu 18.04  |
| Web-2    | Web Server		 | 10.0.0.6   | Linux ubuntu 18.04  |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox machine with IP 13.86.152.158 can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 104.188.181.93

Machines within the network can only be accessed by Jumpbox with internal IP 10.0.0.4 and Public IP 13.86.152.158.


A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box |    Yes              | 104.188.181.93       |
| Web-1    |    No               | 10.0.0.4             |
| Web-2    |    No               | 10.0.0.4             |
| Elk-vm   |    No               | 10.0.0.4             |
|          |                     |                      |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because Asnible is
- Simple: Ansible playbook use YAML, easy to understand and develop. 
- Flexible: Ansible plabook can be easily modified and reuse in various environments.
- Faster deployment: Ansible makes managing lifecycle easier, for example Dockers/containers can be deployed easily and images can be updated instantaneously. 
- Error prune: Playbook can be tested and reused versus doing manually.
- Powerful: Highly complex workflow can be easily modeled.
- Efficient: Because of its nature of being agentless, it doesn't require lot of software to install which saves space and memory. 


The playbook implements the following tasks:
 
- Download and configure the elk-docker container onto this new VM.
- Installs the following apt packages including docker.io and python3-pip
- Download image on Elk server.
- Launch the elk-docker container to start the ELK server.
- Install Filebeat on the VM's
- Install Metricbeat

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

The files in this repository were used to configure the network depicted in \Georgia-Tech-Security-Bootcamp\HW13-Elk-Stack-Project\Images\docker_ps_output.png


### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1; IP = 10.0.0.5
- Web-2; IP = 10.0.0.6

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- githubbeat: Monitors GitHub repository activity. 
- httpbeat: Polls multiple HTTP(S) endpoints and sends the data to Logstash or Elasticsearch. Supports all HTTP methods and proxies.
- jmxproxybeat: Reads Tomcat JMX metrics exposed over JMX Proxy Servlet to HTTP.
- macwifibeat: Reads various indicators for a MacBook’s WiFi Signal Strength

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-config.yml file to _/etc/ansible/filebeat-config.yml.
- Update the _/etc/filebeat/filebeat-config.yml file to include IP of your Elk-vm IP 10.1.0.4. Filebeat is installed on DVWA (Web-1, Web-2) VMs. You will use ELK server's GUI     to begin installing Filebeat on your DVWA VM. You will a new VM to install Elk and create container.
- Run the Playbook, and navigate to _http://20.46.245.177:5601/app/kibana to check that the installation worked as expected.

 _As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._


 - sudo docker start elk OR sudo docker run -p 5601:5601 -p 9200:9200 -p 5044:5044 -it --name elk sebp/elk:761
 
 - docker run hello-world
 
 - nano /etc/ansible/hosts
    [webservers]
    10.0.0.4 ansible_python_interpreter=/usr/bin/python3
    10.0.0.5 ansible_python_interpreter=/usr/bin/python3
    10.0.0.6 ansible_python_interpreter=/usr/bin/python3

    [elk]
    10.1.0.4 ansible_python_interpreter=/usr/bin/python3
 
- curl http://localhost:5601/app/kibana
 
- curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/filebeat-config.yml
 
- nano /etc/ansible/filebeat-config.yml
 
- nano filebeat-playbook.yml
 
- dpkg -i filebeat-7.4.0-amd64.deb
 
- mv /etc/ansible/files/filebeat-config.yml /etc/filebeat/filebeat.yml
 
- filebeat modules enable system
 
- filebeat setup
 
- service filebeat start

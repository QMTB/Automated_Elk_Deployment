# Automated_Elk_Deployment

The files in this repository were used to configure the network depicted below.

![Network%20Topography](Images/Network_Topology.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML Playbook file may be used to install only certain pieces of it, such as Filebeat.

![Filebeat%20Playbook](Ansible/YAML_Playbooks/Filebeat_Playbook.odt)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
- Beats in Use
- Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly responsive, in addition to restricting access to the network.
- Load balancers primarily protect the Availability aspect of security.  Load balancing provides additional filtering of traffic through its own set of security rules, and health probe function.  In this deployment, the load balancer is acting as a proxy in order to protect the web servers in the backend pool. Load balancing also protects the availability of data by mitigating the risk of DDoS attacks.  It does this by balancing web traffic to the backend pool servers, and monitoring the respective health of the individual servers.   
The Network infrastructure is deployed by ways of the Jump Box Provisioner.  The main advantage of a Jump box is it maintains secure, tightly controlled remote access to the entire virtual network. It provides one location by which an administrator can safely manage the deployment, configuration and maintenance of the entire internal virtual network. 
Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.

- Filebeat watches for any changes to files or files systems.  
- Metricbeat records system metrics such as CPU, or Memory.  Metricbeat also records metrics associated to services such as Apache servers.  Metricbeat can even monitor container performance.

The configuration details of each machine may be found below.
                                                    

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| Web-1    |Web Server| 10.0.0.5   | Linux            |
| Web-2    |Web Server| 10.0.0.6   | Linux            |
| Web-3    |Web Server| 10.0.0.7   | Linux            |
| Elk      |ELk Server| 10.1.0.4   | Linux            |


### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- my Workstation Public IP 

Machines within the network can only be accessed by Ansible control node SSH.
- Only the Jump Box Provisioner 10.0.0.4 has access to the Elk Server.


- ***Note--- Access is currently limited to the public IP of my home workstation.  However, this network does accept public HTTP traffic via port 80 through the Load balancer gateway Public IP address.  If the load balancer security rule 103 was upended, the load balancer could grant public access to the backend pool DVWA servers.  

- The Jump Box Provisoner is only accessible via SSH from my Home Public IP workstation.  The Jump Box in turn has SSH access to all of the internal servers by way of the Ansible control node. 

- The Elk server allows HTTP through port 5601.  Again, access is currently limited to only my Workstation Public IP. 




A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |                 | 
|----------|---------------------|----------------------------------------|
| Jump Box | Yes                 | Workstation Public IP                  |
| Web-1    | No                  | 10.0.0.4, 40.04.56.12(Load-Balancer)   |                                
| Web-2    | No                  | 10.0.0.4, 40.04.56.12(Load-Balancer)   |
| Web-3    | No                  | 10.0.0.4, 40.04.56.12(Load-Balancer)   |  
|  Elk     | Yes                 | Workstation Public IP, 10.0.0.4        |                                                 



### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because, the process is incredibly efficient, with lightning fast almost limitless scalability. It is also flexible, and light on resources.
- One of the most important advantages to automating with Ansible is the documentation aspect.  The commands in the YAML playbook which execute all of the infrastructure also double, as an easy to use and read document.   It is not even necessary to to be able to read code in order to configure these files.  


The elk-server playbook implements the following tasks:

- Install Docker
- Install Python
- Install Docker Python Module
- Increase system memory
- Download Elk container
- Set Docker container restart to always
- Configure for ports 5601,9200,and 5044
- Start Elk-Server on boot
- Set restart to always


The following screenshot displays the result of running `Docker ps` after successfully configuring the ELK instance.

![Docker%20PS](Images/Docker_PS.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1: 10.0.0.5, Web-2: 10.0.0.6, Web-3: 10.0.0.7

We have installed the following Beats on these machines:
- Filebeat, and Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat is being used to monitor the Apache service which is installed on our DVWA web servers as well as the MySQL databases they generate. These logs give us information such as any files deleted or downloaded.
- Metricbeat is monitoring the overall system health of the DVWA servers. It watches for memory management problems, or if the CPU is being stressed potentially by outside factors such as a DDoS attack.


### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

- The playbook is a YAML file located within this repository in the Playbook_YAMLS file.  Copy the appropriate file, and add it to /etc/ansible/roles located inside the Ansible container. You need to create this directory if it does not already exist. 

- In addition to the roles directory, there should also be a files directory located at etc/ansible/files.  The files directory contains configuration files which work in conjunction with the playbooks.  You must make sure these are updated with the appropriate IP addressing and port configurations.  There are copies of the configuration files located in this repository in the Config_YAMLS file.
  
- In order to designate the desired machine(s) on which the playbook will run, go to the /etc/ansible/hosts file.  Add the internal IP address of your target machine(s) along with the desired Python version, and specify which hosts group the machine belongs to.  The hosts group names are written inside of brackets.  This hosts group name will correspond to the hosts entry at the top of the YAML playbook.  

- the hosts file should look like this following example. 

![hosts%20file%20example](/Images/Hosts_File_Example.png) 


- Run the playbook, and navigate to a browser, and then enter the elk server public IP:5601 to check, if the installation worked as expected.



Here are the specific commands the user will need to run to download the playbook, update the files, etc.

- Ansible automatically  generates unique names for newly created ansible containers.  In this example, the container is named “jovial_bouman”.  
- sudo docker start jovial_bouman
- sudo docker attach jovial_bouman   
- nano /etc/ansible/files/config.yml
- make any necessary updates to the config.yml file.
- nano /etc/ansible/roles/<playbook>.yml
- paste the proper playbook and save it.
- cd /etc/ansible/hosts
- Add the IP to the proper group with the Python version specified in the “hosts” files 
- Now run ansible-playbook /etc/ansible/roles/<playbook>.yml from within the "roles" directory.

- If there are no errors, check the containers are running, and confirm it in your browser.  To check if the DVWA servers are running enter the Public IP of the load balancer, which is the web server gateway.  To check if the Elk server is running enter the public IP and port of the Elk Server like so Public IP:5601. 

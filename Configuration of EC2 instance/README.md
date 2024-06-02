Project Report: Automated Configuration Management of EC2 Instances Using Ansible

Project Overview:
In this project, I set up a control node named ansibleserver and a target node named targetserver on Amazon EC2. I used the control node to manage the configuration of the target server using Ansible. The objective was to install and start the Nginx web server on the target server using Ansible automation.

Project Steps:

Setting up EC2 Instances:

Launched two EC2 instances on AWS: ansibleserver and targetserver.
Configured security groups to allow SSH access to both instances.

Establishing Passwordless SSH:
connected to ec2 instance from ubuntu using command.
"ssh-copy-id -i /path/to/ssh-key.pem ubuntu@public-ip-address"

after connecting to the severs generated public & private key pair on both instances using command.
"ssh-keygen"

made a password less authentication between them by copying the 1st ec2 key pair to the authorize keys of targetserver.

Copied the SSH key pair from ansibleserver to targetserver to enable passwordless authentication between the two servers.

Creating Inventory File:

Created an inventory file named inventory on ansibleserver to specify the IP address of targetserver.
"vim inventory"

Testing Connectivity:

Checked if ansibleserver has passwordless authentication to targetserver by executing a command from ansibleserver to create a file (devops) on targetserver using Ansible adhoc command
ansible -i inventory all -m shell -a "touch devops"

Verified passwordless SSH connectivity by logging into targetserver from ansibleserver using command.
"ssh ubuntu@private/public-ip-address"

Writing Ansible Playbook:

Created a playbook named first_playbook.yml on ansibleserver using coomand.
"vim first_playbook.yml"
Defined tasks in the playbook to install and start Nginx on all hosts specified in the inventory file.

Executing Ansible Playbook:

Run the playbook using the ansible-playbook command, specifying the inventory file.
"ansible-playbook -i inventory first_playbook.yml"
Ansible installed and started Nginx on the targetserver as instructed in the playbook.

Verification:

Checked the status of Nginx on targetserver using systemctl status nginx.
"sudo systemctl status nginx"

Confirmed that Nginx was successfully installed and running on targetserver.

Ansible Use Cases:

Automation of Software Installation:

Ansible was used to automate the installation of Nginx on the target server (targetserver). This eliminated the need for manual installation steps, saving time and reducing human error.

Configuration Management:

Ansible facilitated the configuration management of the target server by ensuring that Nginx was installed and started as specified in the playbook. This standardized the server configuration across environments.

Infrastructure Orchestration:

Ansible allowed for the orchestration of tasks across multiple servers (ansibleserver and targetserver). Tasks were executed in a coordinated manner, simplifying the management of the infrastructure.

Remote Execution of Commands:

Ansible enabled the remote execution of commands on the target server (targetserver). This provided a centralized control point (ansibleserver) for managing remote servers efficiently.

Conclusion:
Through this project, I successfully configured the targetserver using Ansible automation. Ansible proved to be a powerful tool for automating IT tasks, simplifying configuration management, and improving overall efficiency in managing server infrastructure.

Commands Used:

SSH Key Copy:
ssh-copy-id -i /path/to/ssh-key.pem ubuntu@targetserver-ip

Create Inventory File:
vim inventory

Ansible Command to Test Connectivity:
ansible -i inventory all -m shell -a "touch devops"

Creating Ansible Playbook:
vim first_playbook.yml

Executing Ansible Playbook:
ansible-playbook -i inventory first_playbook.yml

Checking Nginx Status on Target Server:
sudo systemctl status nginx


Errors and Solutions:

1.Errors in the playbook syntax or execution.

Solution: Corrected the playbook syntax by ensuring all tasks were properly defined.
yaml


- name: Install and start nginx
  
  hosts: all
  
  become: true # initially wrote "root" insted of "true"

  tasks:
    - name: Install nginx
      apt:
        name: nginx
        state: present
    - name: Start nginx
      service:
        name: nginx
        state: started
      
Error: SSH Key Not Found

Description: While attempting to copy the SSH key, the file was not found.
Solution: Navigated to the correct directory where the SSH key was located and ensured the file name was correct.

cd /path/to/key/directory
ssh-copy-id -i ansible.pem ubuntu@targetserver-ip

Error: chmod Command File Not Found

Description: Attempted to change permissions on a file that did not exist.
Solution: Verified the file path and name, then used chmod correctly on the SSH key.

chmod 400 ansible.pem

Error: Inventory File Creation Issue

Description: Encountered errors while creating or editing the inventory file.
Solution: Used Vim to correctly create and edit the inventory file, ensuring the correct IP addresses were listed.

vim inventory

Error: System Not Using systemd

Description: Attempted to use timedatectl on a system that does not use systemd.
Solution: Used the date command to manually set the system time.

sudo date MMDDhhmm[[CC]YY][.ss]

Error: Playbook Execution Errors


Difficulties Faced:

SSH Configuration:

Difficulty in ensuring passwordless SSH setup between control and target nodes.
Solution: Carefully followed steps to copy SSH keys and verify connectivity.

Inventory Management:

Ensuring the correct format and content of the Ansible inventory file.
Solution: Double-checked IP addresses and file formatting to avoid syntax issues.

Time Synchronization:

Encountered issues with time synchronization due to the system not using systemd.
Solution: Used alternative methods (date command) to manually set the system time.

Ansible Playbook Syntax:

Initial errors in playbook syntax led to failed executions.
Solution: Reviewed Ansible documentation and examples to correct the playbook.

Nginx Verification:

Ensuring Nginx was properly installed and running on the target server.
Solution: Used systemctl status nginx to verify the service status.

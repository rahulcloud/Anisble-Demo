# ANSIBLE INSTALLATION DOC and WORLFLOW  
For Ansible to work,python and SSH should be configured on all the servers  
Prerequistie  
Python  
SSH  
####################################  
Installing Ansible on target instances   
Ansible can be installed as part of the bootstrapping of the instance or with Run Command. The following is some reference information   you can use to install Ansible on different Linux distributions:  

# amazon linux:-->  
  
  For Amazon Linux Ansible can be installed using pip. You can use the following command.  
  
  sudo pip install ansible  
# Ubuntu:-->  
  For Ubuntu you can install Ansible using the default package manager. Use this command.  
  
  sudo apt-get install ansible -y  
# RedHat 7:-->  
  For RedHat 7 you can install Ansible by enabling the epel repo. Use the following commands:  
  
  sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm  
  sudo yum -y install ansible  
   
############################################  
# inventoryfile: example  
  
#host-dev  
  
[webservers]  
x.x.x.x  
x.x.x.x  
  
[loadbalancers]  
x.x.x.x  
  
[local]  
control ansible_connection=local  
###########################################
# Once Inventory setup complated just check below commands  
ansible --list-hosts all  
  
ansible -i inventoryfile --list-hosts all    
  
default ansible.cfg file  
https://raw.githubusercontent.com/ansible/ansible/devel/examples/ansible.cfg  
###########################################  
sample ansible.cfg  
# ansible.cfg  
  
[defaults]  
inventory = ./ec2.py  
remote_user = ec2-user  
private_key_file = /root/.ssh/key.pem  
host_key_checking = False  
retry_files_enabled = False  
  
#######################################
  
# anisble.cfg file creation  
  
#ansible.cfg  
  
[defaults]  
inventory = ./inventoryfilepath  
  
##########################################  
ansible --list-hosts all  
ansible --list-hosts webservers  
ansible --list-hosts loadbalancers  
  
##########################################  
# inventoryfile: example with tags included  
  
#host-dev  
  
[webservers]  
web1 ansible_host=x.x.x.x  
web2 ansible_host=x.x.x.x  
  
[loadbalancers]  
lb1 ansible_host=x.x.x.x  
  
[local]  
control ansible_connection=local  
############################################  
ansible --list-hosts all  
ansible --list-hosts "*"  
ansible --list-hosts web*  
ansible --list-hosts webservers:loadbalancers  
ansible --list-hosts \!control  
ansible --list-hosts webservers[0]  
###########################################  
ansible -m ping all  
###########################################  
  
  
# ansible.cfg   
   
   
 [defaults]   
 inventory = ./hosts-dev   
 remote_user = <SSH_USERNAME>[ec2-user]   
 private_key_file = /path_to/<SSH_KEY>.pem   
 host_key_checking = False #finger print check  
   
  
 # Windows only users   
 [ssh_connection]   
 ssh_args = -o ControlMaster=no   
   
 #############################################  
 # Try some other simple commands   
 ansible -m shell -a "uname" webservers:loadbalancers  
  ansible -m command -a "/bin/false" \!control  
  --> to check RC-return code  
   
###############################################  
#  Modules list  
  https://docs.ansible.com/ansible/latest/modules/modules_by_category.html  
  the ansible command  
  https://docs.ansible.com/ansible/latest/cli/ansible.html  
###############################################  
# Introduction to playbooks  
https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html  
###############################################  
anisble-playbook ping.yml  
anisble-playbook uname.yml  
anisble-playbook yum-update.yml  
#####################################  
To skip retry files creation in ansible.cfg setup below parameter  
retry_files_enabled = False  

######################################  
ansible-playbook install-services.yml  
######################################  
 create a simple index.php  
 '<?php    
 echo "<h1>Hello, World! This is my Ansible page.</h1>";  
 ?>  
  
#######################################  
ansible-playbook setup-app.yml  (without Configure php.ini file)  
######################################  
Now go for some configuration chnages with playbook  
ansible-playbook setp-app.yml  
          ##################      
Configuring Own Load balancer for HTTPD server  
Create Folder Config place inside lb-config.j2  
Ansible-playbook setub-lb.yml  

once setup loadbalancer completes for lb server and check the status  
IP/balancer-manager  
#######################################
# Now let see how service Handlers work in Ansible  

ansible-playbook setup-app.yml  
ansible-playbook setup-lb.yml
  
######################################  
setup loadbalancer for lb server and check the status  
IP/balancer-manager  
  

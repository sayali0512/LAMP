# WordPress and WooCommerce Installation 
![Workflow](https://github.com/krishnaitalent/LAMP/blob/lamp_docmentation/images/WordPress_Flow_Diagram.png)
This document will guide us how to install WordPress and WooCommerce plugin using Ansible on top of LAMP stack .
## Prerequisites

- This requires LAMP stack deployment.
- Make sure that Host VM (where WordPress to be installed) and Ansible VM should be in the same resource group and region.
- Current WordPress installatin script supports Ngnix & MySql Database

### Enabling Password Authentication  

- Login into host VM (controller VM) and navigate to /home/azureadmin(user)/ 
- Run the following commands to enable password authentication.
```
sudo sed -i "s~PasswordAuthentication no~PasswordAuthentication yes~" /etc/ssh/sshd_config
sudo sed -i "s~#UseLogin no~UseLogin yes~" /etc/ssh/sshd_config
sudo sed -i "s~#   StrictHostKeyChecking ask~   StrictHostKeyChecking no~" /etc/ssh/ssh_config
sudo systemctl restart sshd
sudo passwd azureadmin
```
- Above command will promt user to set new password. 
```
Note: 
- This new password should be given as an input to the below Ansible VM template deployment.
- Host VM should be password enable to copy SSH keys.
- Database will be created as part of WordPress installation.
```	
### Deploying Ansible VM
	
- Install Ansible VM through [ARM Template](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fummadisudhakar%2FLAMP%2Fansible_playbook_mat32%2Fansibledeploy-wordpress.json) by giving inputs as follows.
```
o	Host VM Name
o	SSH Public Key
o	Host VM Username
o	Host VM IP
o	Host VM Password
o	Database Server Name
o	Database Login Name
o	Database Login Password
o	Database Name
o	WordPress Domain Name
```
- This template will execute a script (wordpress_main.sh) in extensions.
- wordpress_main.sh will download the WordPress script (wordpress_script.sh) from the GitHub.
- The script will be downloaded to the /home/azureadmin(username).

### Installing WordPress into Host VM.
	
Using SSH please login into Ansible VM and go to the path /home/azureadmin(username)
```
To login to Anisble VM below details are required,
Ansible VM IP (User can get it from Azure Portal after successfull Anisble deployment in the previous step)
UserID: azureadmin
SSH Key
 ```
 To generate SSH key [clickhere](https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
  
Run the following command to install WordPress in Host VM by using ansible playbook.
```
Command to execute: bash run.sh
```
### Installation and Replication of WordPress with WooCommerce Plugin

#### Installing WordPress into host VM.
	
- run.sh file will run ansible script wordpress_script.sh which will run the playbook with the user inputs.
- Once run.sh executed, ansible scripts will execute below roles,
```
Below steps will install WordPress & its configuration. 

Role: SSH Key Configuration. 
	Which will Generate the SSH key pair and will copy to the targeted Host VM using password authentication.
Role: WordPress
	It will download & Install WordPress
Role: WooCommerce
	It will download & Install WooCommerce plugin
Role: Replication

- Configure SSL certs: Generate OpenSSL certificates to /azlamp/certs/ folder.
- Linking data location: Links data (/azlamp/data/) to all shared web frontend instances.
- Create Nginx Configuration: Create Nginx configuration file in VMSS instance
- Replication: Replicate the WordPress folder to the VMSS instance.
```
Inorder to access the WordPress use Load Balancer IP.

User should be able to get Username and password of the WordPress from wordpress.txt at /home/azureadmin location.

Following above steps WordPress installation with WooCommerce plugin will be completed successfully.
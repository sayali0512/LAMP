# Moodle Installation 

This document will guide us how to install Moodle using Ansible on top of LAMP stack .
## Prerequisites

- This requires LAMP stack deployment.
- Make sure that Host VM (where Moodle to be installed) and Ansible VM should be in the same resource group and region.

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
- Database will be created as part of Moodle installation.
```	
### Deploying Ansible VM
	
- Install Ansible VM through [ARM Template](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fummadisudhakar%2FLAMP%2Fansible_playbook_mat32%2Fansibledeploy-moodle.json) by giving inputs as follows.
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
o	Moodle Domain Name
o	Host Load Balancer IP

```
- This template will execute a script (moodle_main.sh) in extensions.
- moodle_main.sh will download the Moodle script (moodle_script.sh) from the GitHub.
- The script will be downloaded to the /home/azureadmin(username).

### Installing Moodle into Host VM.
	
Using SSH please login into Ansible VM and go to the path /home/azureadmin(username)
```
To login to Anisble VM below details are required,
Ansible VM IP (User can get it from Azure Portal after successfull Anisble deployment in the previous step)
UserID: azureadmin
SSH Key
 ```
 To generate SSH key [clickhere](https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
  
Run the following command to install Moodle in Host VM by using ansible playbook.
```
Command to execute: bash run.sh
```
### Installation and Replication of Moodle
[Workflow]
#### Installing Moodle into host VM.
	
- bash run.sh file will run ansible script moodle_script.sh which will run the playbook with the user inputs.
- Once run.sh executed, ansible scripts will execute below roles,

#### Moodle Install
Clone the Moodle repo “ansible_playbook” which contains Roles.
- There are three roles in the ansible playbook for Moodle
    1. sshkeyconfig
    2. Moodle
    3. Replication

- Role: SSH Key Configuration. 
	Which will Generate the SSH key pair and will copy to the targeted Host VM using password authentication.
- Role: Moodle
	Moodle will be downloaded in .zip format and installed in Host VM at target location.
- Role: Replication: 
  It will download the script “moodle_replication.sh” and executes the script by this Moodle directory will be replicating to Virtual Machine Scale Set (VMSS) 

```
Replication Script(moodle_replication.sh) will have following functions
- Change location: It will move the Moodle folder to /azlamp/html/ location
- Configure SSL certs: Generate OpenSSL certificates to /azlamp/certs/ folder
- Linking data location:  This function will link data to all shared  web frontend instances
- Update Nginx Configuration: This function will update the Nginx configuration file.
- Moodledata: It will create moodledata directory in azlamp folder.
- Replication: This function will replicate the Moodle folder to the VMSS instance.
```
Inorder to access the Moodle use Load Balancer IP ((User can get it from Azure Portal)

User should be able to get Username and password of the Moodle from moodle.txt at /home/azureadmin location.

Following above steps Moodle installation will be completed successfully.
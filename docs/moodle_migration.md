# Moodle Migration

This document explains how to  migrate Moodle from OnPrem servers to Azure cloud. 

## Prerequisites

- Must have access to the OnPrem servers to take backup of Moodle and database/configurations.
- Should have a Azure subscription and the Azure Blob storage created before migration.
- This migration activity supports and validated with Softwares.
	*	Ubuntu 18.04 LTS
	*	Nginx web server 1.14.3
	*	MySQL PaaS 5.6, 5.7 or 8.0 database server
	*	PHP 7.2, 7.3, or 7.4 
	*   Moodle 3.8 & 3.9

## Migration 

Moodle Migration involves following steps,

- Data Export from OnPrem to Azure Cloud
- Import data to Azure cloud.
- DB migration.

## Data Export from OnPrem to Azure Cloud

- Create Azure Blob storage in the Azure subscription and create below folders,
		- moodle 
		- moodledata
		- configurations
		- dbbackup

![Workflow](https://github.com/krishnaitalent/LAMP/blob/lamp_docmentation/images/moodle_export.png)

- Make a tar file of the Moodle folder. Ex: /moodle 
- Make a tar file of the Moodle data folder. Ex: /moodledata
- Make a tar file of php, nginx & moodle configurations. Ex: config.tar.gz
- Take the back of the MySQL DB.
- Use AZCopy to copy above backup files to respective folders in Azure Blob storage.
	

## Import data to Azure cloud

![Workflow](https://github.com/krishnaitalent/LAMP/blob/lamp_docmentation/images/moodle_migration_import.png)

### Option 1:

- Use ARM templates to install Moodle. [Click here](https://github.com/Azure/Moodle/blob/master/README.md) 
	* There are different kinds of deployments where user can pick one depending on the Business requirement.

- This will install infrastructure & Moodle as per the predefined deployment types. Infrastructure includes,
	* Virtual Network, Subnets
	* Azure load balancer or Application gateway.
	* VM scaleset for auto scaling.
	* Storage account Ex: NFS, Gluserfs, Azure Files
	* Network Sercirity Groups.
	* Redis Cache
- More options are defined through custom template deployment.
	* [Click here](https://github.com/Azure/Moodle/blob/master/README.md) to choose Fully configurable deployment
	* Click Deploy to Azure button and it should go to Azure sbscription custom deployment page after successful login.
- Copy the moodle & moodle data files from Azure blob storage to shared folder (/moodle) in the Azure VM.
- Create Database for Moodle.
- Set the Moodle related folder permissions.
- Configure moodle, certs, PHP, nginx configuration based on the OnPrem.


## Option 2

- Install the infra using azure portal
- Create Vnets, Subnets, NIC, Load balancer, VMscaleset etc.
- Install LAMP stack using a shell script or manual install on Azure VMs.
- Copy the PHP, nginx configurations from blob to azure using a script.
- Copy Moodle folder from blob storage to respective file storage which will be used by the VM’s
	* For Gluster FS Files storage please [Click here](https://docs.gluster.org/en/latest/Install-Guide/Install/)
	* NFS file storage please [Click here](https://docs.microsoft.com/en-us/azure/cyclecloud/how-to/create-fileserver?view=cyclecloud-7#configuring-an-nfs-server-and-file-share)
	* Azure Files please [Click here](https://docs.microsoft.com/en-us/azure/cyclecloud/how-to/create-fileserver?view=cyclecloud-7#configuring-an-nfs-server-and-file-share)
	
- Create Database for Moodle.
- Set the Moodle related folder permissions. Make sure that /moodle folder has www-data ownership.[Click here](https://docs.moodle.org/39/en/Installation_quick_guide#Configure_Moodle)
- Configure moodle, certs, PHP, nginx configuration (site enabled, logs configurations)based on the OnPrem
- Make sure Moodle Cron job is up and running. [Click here](https://docs.moodle.org/39/en/Installation_quick_guide#Set_up_cron) for more details.





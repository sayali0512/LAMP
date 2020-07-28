# Moodle Migration

This document explains how to  migrate Moodle from OnPrem servers to Azure cloud. 

## Prerequisites

- Must have access to the servers to take backup of Moodle and database/configurations.
- Should have a Azure subscription and the Azure Blob storage created before migration.
- This migration activity supports and validated with Softwares.
	*	Ubuntu 16.04 LTS
	*	Nginx web server 1.10.3
	*	MySQL PaaS 5.6, 5.7 or 8.0 database server
	*	PHP 7.2, 7.3, or 7.4 

## Migration 

Moodle Migration involes following steps,

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
- Use AZCopy to above backup files respective folder in Azure Blob storage.
	

## Import data to Azure cloud

![Workflow](https://github.com/krishnaitalent/LAMP/blob/lamp_docmentation/images/moodle_migration_import.png)

### Option 1:

- Use ARM templates to install Moodle. Click here https://github.com/Azure/Moodle/blob/master/README.md
There are different kinds of deployments where user can pick one depending on the Business requirement.

- This will install infra & Moodle as per the predefined deployment types.
	* Virtual Network, Subnets
	* Infrastructure includes Azure load balancer or Application gateway.
	* VM scaleset for auto scaling.
	* Storage account Ex: NFS, Gluserfs, Azure Files
	* Network Sercirity Groups.
	* Redis Cache
- More options are defined through custom template deployment.
	* Click here to choose Fully configurable deployment
	* Click Deploy to Azure button and it should go to Azure sbscription custom deployment page after successful login.
- Copy the moodle & moodle data files from Azure blob storage to shared folder in the Azure VM.
- Create Database for Moodle.
- Set the Moodle related folder permissions.
- Configure moodle, certs, PHP, nginx configuration based on the onprem.


## Option 2

- Install the infra using azure portal
- Create Vnets, Subnets, NIC, Load balancer, VMscaleset etc.
- Install LAMP stack using a shell script or manual install on Azure VMs.
- Copy the PHP, nginx configurations from blob to azure using a script.
- Copy Moodle folder from blob storage to azure file storage which will be used by the VM’s
- Create Database for Moodle.
- set the Moodle related folder permissions.
- Configure moodle, certs, PHP, nginx configuration based on the onprem.





# Manual Migration

This document explains how to manually migrate Moodle from OnPrem to Azure. 

The following diagram shows how the required system components participate in the installation process.

## Prerequisites

- Must have access to the servers to take backup of Moodle and database/configurations.
- Should have a Azure subscription and the Azure Blob storage created before migration.
- This migration activity supports and validated with Softwares.
	*	Ubuntu 16.04 LTS
	*	Nginx web server 1.10.3
	*	MySQL PaaS 5.6, 5.7 or 8.0 database server
	*	PHP 7.2, 7.3, or 7.4 

## Migration 

Manual Migration involes following steps,

- Data Export from OnPrem to Azure Cloud
- Import data to Azure cloud.
- DB migration.

## Data Export from OnPrem to Azure Cloud

![Workflow](https://github.com/krishnaitalent/LAMP/blob/lamp_docmentation/images/moodle_manual_migration_export.png)

Install WPCLI on the OnPrem server.
Make a tar file of the Moodle folder. Ex: moodle.tar.gz
Make a tar file of the Moodle data folder. Ex: moodledata.tar.gz
Make a tar file of php, nginx configurations. Ex: config.tar.gz
Take the back of the MySQL DB.
Copy above backup files Azure Blob storage.

## Import data to Azure cloud

![Workflow](https://github.com/krishnaitalent/LAMP/blob/lamp_docmentation/images/moodle_manual_migration_import.png)

### Option 1:

- Use ARM templates to install Moodle. Click here https://github.com/Azure/Moodle/blob/master/README.md
There are different kinds of deployments where user can pick one depending on the Business requirement.

- This will install infra & Moodle as per the predefined deployment types.
- More options are defined through custom template deployment.
- Copy the moodle & moodle data files from Azure blob storage to shared folder in the Azure VM.
- Configure moodle, certs, PHP, nginx configuration based on the onprem.
- Create Database for Moodle.
- set the Moodle related folder permissions.

## Option 2

- Install the infra using azure portal
- Create Vnets, Subnets, NIC, Load balancer, VMscaleset etc.
- Install LAMP stack using a shell script or manual install on Azure VMs.
- Copy the PHP, nginx configurations from blob to azure using a script.
- Copy Moodle folder from blob storage to azure file storage which will be used by the VM’s
- Configure moodle, certs, PHP, nginx configuration based on the onprem.
- Create Database for Moodle.
- set the Moodle related folder permissions.




# FTSupportKit

Swiss knife shell interface for FTL Support team
---
- backup_acs            => Backup ACS cluster
- backup_db             => Backup DB*
- upgrade               => Upgrade ACS cluster
- liquibase             => Upgrade DB*
- clean_install_acs     => Install new ACS cluster
- clean_install_db      => Install new DB*

Author:    Kirill Rudenko
Date:      22.03.2020


Usage:
--- 
This is a first step into automation of deploying and upgrading FTL software.
Main idea is to minimize time investment, efforts and human factor related errors.
All you need to do is:

1. Define server login details in hosts.txt file
2. Place files for deployment in "clean_install" folder or files for upgrading in "upgrade" folder
3. Define customer-specific details in "kit" file after the section "Edit customer-dependent settings below"

Once all is set - you can deploy/upgrade the farm of the servers using several commands.

Example of project structure:
---
$ tree
.
├── ansible.cfg
├── clean_install
│   ├── acsv5.sql
│   └── FTACS5.zip
├── hosts_default.txt
├── kit
├── liquibase
│   └── place-liquibase-files-here
├── logs
│   └── folder-for-old-ftsupportkit-logs
└── upgrade
    └── FTACS5_new.zip

Example of hosts file:
----
[ACS]
acs1 ansible_host=10.0.0.1
acs2 ansible_host=10.0.0.2

[ACS:vars]
ansible_user=acsuser
ansible_pass=acspassword

[TR_DB]
prod_db	ansible_host=10.0.0.3
lab_db ansible_host=10.0.0.4

[TR_DB:vars]
ansible_user=dbuser
ansible_pass=dbpassword

In case user/password is different for each node, place them in sigle section, without vars section, like below:

[ACS]
acs1 ansible_host=10.0.0.1 ansible_user=acsuser_1 ansible_pass=acspassword_1
acs2 ansible_host=10.0.0.2 ansible_user=acsuser_2 ansible_pass=acspassword_2

[TR_DB]
prod_db	ansible_host=10.0.0.3 ansible_user=dbuser_1 ansible_pass=dbpassword_1
lab_db ansible_host=10.0.0.4 ansible_user=dbuser_2 ansible_pass=dbpassword_2


Example of settings in kit:
---

DB_USR='root'                           // Database user with DBA permissions
DB_PASS='root'                          // Password for the user above
DB_DUMP_PATH='/home/friendly/'          // Path where DB dump should be stored
DB_TYPE='mysql'                         // DB type (mysql/oracle)
DB_HOST='10.0.2.1'                      // DB IP address
FTDB_USR='ftacs'                        // DB user of FT schema (for Liquibase)
FTDB_PASS='ftacs'                       // Password for the above user
FTDB_SCHEMA='ftacs'                     // FT DB schema

ACSPATH='/usr/local'                    // Parent folder for FTACS5 on ACS node
ACSHOME='/usr/local/FTACS5'             // Full path of FTACS5

UPGRADE_ACS='FTACS5_new.zip'            // Name of the zip for the upgrading of the ACS
NEW_ACS='FTACS5.zip'                    // Name of the zip for fresh installation of the ACS

Requirements
---
1. OS - RHEL\CentOS\Fedora\Ubuntu\Debian. Regretfully, Windows is not supported as "host" machine for this tool.
2. Java installed
3. Ansible installed
Installing of Ansible is described here - https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html

If there is no Internet and/or access to the repositories on your server - download relevant RPM from here https://releases.ansible.com/ansible/rpm/release/
and use something like:

yum localinstall ansible-2.9.19-1.el7.ans.noarch.rpm -y


Installation:
---  

1. git clone git@github.com:swifty94/FTSupportKit.git

No Git installed? Just download the zip directly from here and upload on the server.

2. cd FTSupportKit
$ ./kit 
Usage: ./kit + arg:

             - backup_acs             // Backup ACS cluster
             - backup_db              // Backup DB*
             - upgrade                // Upgrade ACS cluster
             - liquibase              // Upgrade DB
             - clean_install_acs      // Install new ACS cluster
             - clean_install_db       // Install new DB*

      * can be multinode env. as well


Demo:
---
Fresh installation:

![](https://raw.githubusercontent.com/swifty94/FTSupportKit/master/media/clean_install.mp4)


Upgrade:

![](https://raw.githubusercontent.com/swifty94/FTSupportKit/master/media/upgrade.mp4)


# Weight Tracker Ansible


 This project demonstrate the provision of infrastructure as code project  [**weight-tracker-inftastructure**](https://github.com/LiranLavie/weight-tracker-inftastructure) with Ansible .

The requested infrastructure:

![project6](doc/project6.png)


## Configuration
Two identical environments created except for the number of web servers:
1. Production -3 web servers 
2. Staging - 2 web servers

For separation between the two environments I used terraform workspace.

A variable file added for each of the environments.

From the Terraform project I exported variable files for hosts, variable file for Postgres details, and hosts details for host file to use with Ansible.

In the project root folder i created a playbook file named **webserver_playbook.yml**.


Inside this file i added the path for the Postgres server vars and the role name i used.

This role contains a file named **main.yml** which describes all the installations that are needed for the servers:

1. Node.js
2. Python
3. Cloning the weight-tracker app repository
4. Install weight-tracker app dependencies, create env file and initialize database
5. Create a service with a script to ensure the app is running after reboot

For creating the env file i used a template file located in templates folder

## instructions
To deploy the infrastructure and create files for Ansible :

Select the environment
```
terraform workspace select staging  
```

Then apply using the name of the environment var file name after **-var-file=**
```
terraform apply -var-file="staging.tfvars"
```

From Ansible:
To deploy the playbook

```
ansible-playbook -i inventory/production/host webserver_playbook.yml
```

**-i** used for the location of the host file

There are two host files of each environment located in the inventory folder.

Variable files are encrypted with Ansible Vault.


```
 weight-tracker-ansible
│   ├── README.md
│   ├── ansible.cfg
│   ├── doc
│   │   └── project6.png
│   ├── inventory
│   │   ├── db_vars
│   │   │   └── dbvars.yml
│   │   ├── production
│   │   │   ├── host
│   │   │   └── host_vars
│   │   │       ├── web-server-1-production.yml
│   │   │       ├── web-server-2-production.yml
│   │   │       └── web-server-3-production.yml
│   │   └── staging
│   │       ├── host
│   │       └── host_vars
│   │           ├── web-server-1-staging.yml
│   │           └── web-server-2-staging.yml
│   ├── roles
│   │   └── webserver_conf
│   │       ├── tasks
│   │       │   └── main.yml
│   │       └── templates
│   │           ├── app_startup_service.j2
│   │           ├── env.j2
│   │           └── start_app_script.j2
│   └── webserver_playbook.yml
```

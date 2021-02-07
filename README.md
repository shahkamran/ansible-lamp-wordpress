# Provision Wordpress Multisite (Network) with LAMP stack using Ansible Playbook (on Ubuntu)

This playbook will install a WordPress multisite network site along with LAMP stack includig Apache, MySQL & PHP on a Ubuntu instance. Based on options set in group_vars/all variable file, it wil also create a virtualhost on server.

This Ansible playbook has been tested on destination hosts running following Linux distributions but can be easily adopted for other Linux distributions.

* Ubuntu 20.04.1 LTS

## 1. Setting up your environment (Pre-requisites)
### Ansible environment installed and working on your computer or control machine
Ansible heavily depends on Python and will require Python and associated dependencies. On your client you can run following commands to install dependencies, for example.
```
apt update && apt install python python-setuptools -y
easy_install pip
pip install ansible
```

### Set up Systems Inventory and Access
* Move .ssh/config file to your ~/.ssh/config.
* Update inventory.ini file with hosts IDs matching with config file above.
* Configure ssh key access to all target systems so that ssh works without any password from a trusted computer.
* The account set up in config file and used for connectivity to hosts should have sudo access and should be set up with passwordless sudo. If the sudo commands ask for password for the user you can amend sudo configuration by running ```sudo visudo``` and modifying ```#%sudo	ALL=(ALL:ALL) ALL``` to ```%sudo  ALL=(ALL:ALL) NOPASSWD: ALL``` on all destination hosts.
* Install Python3 on all target systems.

```ansible hosts -b -m raw  -a "apt update && apt install python3 -y" -i inventory.ini```


### Settings

- `php_modules`:  An array containing PHP extensions that should be installed to support your WordPress setup. You don't need to change this variable, but you might want to include new extensions to the list if your specific setup requires it.
- `mysql_root_password`: The desired password for the **root** MySQL account.
- `mysql_db`: The name of the MySQL database that should be created for WordPress.
- `mysql_user`: The name of the MySQL user that should be created for WordPress.
- `mysql_password`: The password for the new MySQL user.
- `http_host`: Your domain name.
- `http_conf`: The name of the configuration file that will be created within Apache.
- `http_port`: HTTP port for this virtual host, where `80` is the default.

### Running this Playbook

Quickstart guide for those already familiar with Ansible:

## 3. Obtain the playbook
```shell
git clone https://github.com/shahkamran/ansible-lamp-wordpress.git
```

## 4. Customise Options

```shell
vi group_vars/all
```

```yml
---
#System Settings
php_modules: [ 'php-curl', 'php-gd', 'php-mbstring', 'php-xml', 'php-xmlrpc', 'php-soap', 'php-intl', 'php-zip' ]

#MySQL Settings
mysql_root_password: "mysql_rOOt_password"
mysql_db: "wordpress"
mysql_user: "user_name"
mysql_password: "secret_passw0rd"

#HTTP Settings
http_host: "example.com"
http_conf: "example.com.conf"
http_port: "80"
https_port: "443"
```

## 5. Run the Playbook

```command
ansible-playbook -l [target] -i [inventory file] -u [remote user] playbook.yml
```
- [target] = Host name or id from the list hosts defined in inventory file
- [inventory file] = This is the inventory.ini file with list of your hosts which have access configured in ~/.ssh/config file.
- [remote user] = Name of user to run the playbook as.

## 6. Enable Wordpress Multisite Network

* Go to your website and set up Wordpress initial install.
* Go to Tools - Network Setup (http://example.com/wp-admin/network.php)
* Select one of the two methods and run Install
* Follow the instructions to add additional definitions to /var/www/example.com/wordpress/wp-config.php and /var/www/example.com/wordpress/.htaccess files.
* Restart Apache (service apache2 restart).
* Log in to your site again and look for Network Admin under My Sites located in top menu.

## 7. Troubleshooting
* If you see below error, you do not have Python installed on remote system.
```fatal: [host-01]: FAILED! => {"changed": false, "module_stderr": "Shared connection to <target> closed.\r\n", "module_stdout": "/bin/sh: 1: /usr/bin/python: not found\r\n", "msg": "MODULE FAILURE", "rc": 127}```


You should now be up and running. Well done!

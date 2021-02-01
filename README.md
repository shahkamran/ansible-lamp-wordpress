# Wordpress Multisite (Network) with LAMP stack on Ubuntu

This playbook will install a WordPress multisite network site along with LAMP stack includig Apache, MySQL & PHP on a Ubuntu instance. Based on options set in vars/default.yml` variable file, it wil also create a virtualhost on server.

This has been created for Ubuntu 18.04 LTS and will be adopted for other popular Linux Distributions in future.

## Settings

- `php_modules`:  An array containing PHP extensions that should be installed to support your WordPress setup. You don't need to change this variable, but you might want to include new extensions to the list if your specific setup requires it.
- `mysql_root_password`: The desired password for the **root** MySQL account.
- `mysql_db`: The name of the MySQL database that should be created for WordPress.
- `mysql_user`: The name of the MySQL user that should be created for WordPress.
- `mysql_password`: The password for the new MySQL user.
- `http_host`: Your domain name.
- `http_conf`: The name of the configuration file that will be created within Apache.
- `http_port`: HTTP port for this virtual host, where `80` is the default. 

## Running this Playbook

Quickstart guide for those already familiar with Ansible:

### 1. Obtain the playbook
```shell
git clone https://github.com/shahkamran/ansible-lamp-wordpress.git
```
### 2. Set up Systems Inventory and Access
* Move .ssh/config file to your ~/.ssh/config and make necessary changes to set up passwordless ssh key access to all systems.
* Update inventory.ini file with hosts IDs matching with config file above.
* If you do not have Python installed on the hosts you may also have to install it on individual hosts.

### 3. Customize Options

```shell
nano vars/default.yml
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

### 4. Run the Playbook

```command
ansible-playbook -l [target] -i [inventory file] -u [remote user] playbook.yml
```
- [target] = Host name or id from the list hosts defined in inventory file
- [inventory file] = This is the inventory.ini file with list of your hosts which have access configured in ~/.ssh/config file.
- [remote user] = Name of user to run the playbook as.

### 5. Enable Wordpress Multisite Network

* Go to your website and set up Wordpress intial install.
* Go to Tools - Network Setup (http://example.com/wp-admin/network.php)
* Select one of the two methods and run Install
* Follow the instructions to add additional definitions to /var/www/example.com/wordpress/wp-config.php and /var/www/example.com/wordpress/.htaccess files.
* Restart Apache (service apache2 restart).
* Log in to your site again and look for Network Admin under My Sites located in top menu.


You should now be up and running. Well done!

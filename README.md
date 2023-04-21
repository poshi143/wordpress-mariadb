# wordpress-mariadb
Need two servers one for application installation and another for Db installation
vault password is redhat
var/common.yaml should be update with db server private IP
check inventory file
change key details in ansible.cfg file
cat ansiblehosts
cat main.yaml
ansible-playbook main.yaml -v --ask-vault-pass

DB commands

show databases;
create database devops;
use devops;

manual steps

1. Manual Installation of Database ::

yum install mariadb-server
systemctl enable mariadb.service
systemctl restart mariadb.service

mysql:

CREATE USER 'admin'@'%' IDENTIFIED BY 'admin123';
GRANT ALL PRIVILEGES ON * . * TO 'admin'@'%';
FLUSH PRIVILEGES;
create database devopsdb;

Verification::
select host, user, password from mysql.user;
DROP USER 'admin'@'localhost';

Automating the configuration using ansible roles ::


2. Manual Installation of application

sudo amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2
sudo yum install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd
wget https://wordpress.org/latest.tar.gz
tar -xzf latest.tar.gz
sudo cp -r wordpress/* /var/www/html/
cd /var/www/html/
sudo cp wp-config-sample.php wp-config.php

Post db installation do the below.

sudo vi wp-config.php --> add the database details to this file
sudo chown -R apache /var/www
sudo chgrp -R apache /var/www
sudo chmod 775 /var/www
sudo find /var/www -type d -exec sudo chmod 2775 {} \;
sudo find /var/www -type f -exec sudo chmod 0664 {} \;
sudo systemctl restart httpd

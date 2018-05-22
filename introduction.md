# Ubuntu Installation

## before os installation to upto os installation

1. First I have installled Ubuntu Desktop Os on a server as a host os.
2. Then I have installed Virtualbox5 v 5.1.34_Ubuntu r121010. 
3. Next I have created a new virtual mechine using new button and filled the required details and alocate 8 gb ram for this mechine


```
Name: proserv
type: linux ubuntu server 64bit
memory: 8000mb
harddisk: create a virtual hard disk now
```
4. next i have pointed out the server image file to take forward
5. the virtual mechine starts reading the image and asked the os installation and configuration. the instruction is more general so i skip those procedure here. simple google search will guide you to install and configure.


## after os installation to upto ssh installation

1. after installing the os get into the os. and execute the first step.

```
os update:
username@serverhost$ sudo apt update 
then
username@serverhost$ sudo apt upgrade
```
the above also very basic one so i skip explaining in detail.
once the os get up dated install ssh 

follow the instruction http://ubuntuhandbook.org/index.php/2016/04/enable-ssh-ubuntu-16-04-lts/ and the official manual page is in http://www.openssh.com/manual.html

brief command

to install 
```
username@serverhost$ sudo apt install openssh-server
```

to check the service status
```
username@serverhost$ sudo service ssh status
```
/-----------------un used commands--------/
to change the settings (like the listening port, and root login permission) by editing 
```
username@serverhost$ sudo nano /etc/ssh/sshd_config
```
incase configured, after configuration restart the service by 
```
username@serverhost$ sudo service ssh restart
```
/-----------------//un used commands--------/

### Virtual box image backup

1. after completing with the above steps, i took a back up of the entire setup as VBimg2 

## Mysql setup 

1. I have imported the VBimg2 and changed the name of virtual server from proserv to promysqldb 
2. after importing with default option i have updated the os by executing following cmd

```
username@serverhost$ sudo apt update
```
i have followed the instruction mentioned in digital ocean to install and basic configuration, t read more follow this link https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-16-04 
```
username@serverhost$ sudo apt-get install mysql-server
```

while installing the mysql it will asks you to setup root password:

I have setup my own (ROOTmysqlADMIN)

mysql version : 5.7.22
ubuntu version: 16.04.1

after completing the installation, to secure the installation by executing the following command

```
username@serverhost$ nysql_secure_installation
```
after installation by default "mysqld" directory will be created by the system no need to do manually by executing cmd 
```
username@serverhost$ mysqld --initialize
```

to check the service status of mysql executing the following command
```
username@serverhost$ systemctl status mysql.service

it will say the active (running)
```

/-----------------un used commands--------/
if incase the service not start yet run the following command

```
username@serverhost$ sudo systemctl start mysql
```

/-----------------//un used commands--------/

after this i have take the virtual box image as VBimg3 with default configuration

## Mysql setup

1. start the "promysqldb" server
#### 2. To open up the ip address from localhost(127.0.0.1) to global (0.0.0.0)

execute the following command

```
username@serverhost$ sudo nano /etc/mysql/mysql.conf.d/mysql.cnf
```
after the file get open reach the line 

"bind-address = 127.0.0.1" and change the number into "bind-address = 0.0.0.0"

save the change and exit from configuration file.

#### 3. To enable port 

```
username@serverhost$ sudo ufw enable
```
#### 4. To allow port 3306

```
username@serverhost$ sudo ufw allow 3306/tcp
```
#### 4. To allow port 22

```
username@serverhost$ sudo ufw allow 22/tcp
```
#### 4. To restart the firewall service by executing the following command

```
username@serverhost$ sudo service ufw restart
```

#### 4. To check the firewall status

```
username@serverhost$ sudo ufw status
```
# Database creation 

#### 1. create db

to create database first login  into root user by executing following command

```
username@serverhost$ mysql -u root -p
```
it ask root user password after submiting it will enter into the root account

#### 2. creating specific database for idm

```
mysql> create database `la-idm_portal`;
```
#### 3. creating specific database for jcr

```
mysql> create database `la-jcr_portal`;
```
#### 4. creating specific database for jpa

```
mysql> create database `la-jpa_portal`;
```

#### 5. creating specific user to administer the database

```
mysql> CREATE USER 'ladbadmin'@'localhost' IDENTIFIED BY 'ladbADMIN';
```
#### 6. permiting the user to admin the database 
by executing following command
```
mysql> GRANT ALL ON `la-idm_portal`.* TO 'ladbadmin'@'localhost' IDENTIFIED BY 'ladbADMIN'

mysql> GRANT ALL ON `la-idm_portal`.* TO 'ladbadmin'@'192.168.%.%' IDENTIFIED BY 'ladbADMIN'


mysql> GRANT ALL ON `la-jcr_portal`.* TO 'ladbadmin'@'localhost' IDENTIFIED BY 'ladbADMIN'

mysql> GRANT ALL ON `la-jcr_portal`.* TO 'ladbadmin'@'192.168.%.%' IDENTIFIED BY 'ladbADMIN'


mysql> GRANT ALL ON `la-jpa_portal`.* TO 'ladbadmin'@'localhost' IDENTIFIED BY 'ladbADMIN'

mysql> GRANT ALL ON `la-jpa_portal`.* TO 'ladbadmin'@'192.168.%.%' IDENTIFIED BY 'ladbADMIN'
```

after granting permission we need to flush the privellage by executing the following command

```
mysql> FLUSH PRIVILEGES
```

quit the database by executing following cmmand

```
mysql> quit;
bye
```

to restart the service by executing following command

```
username@serverhost$ sudo service mysql restart
```
to verify the user creaton and database 

```
username@serverhost$ mysql -u ladbadmin -p
pwd: 
```
after this the virtualbox image exported on the same name into the directory of VBimg4 

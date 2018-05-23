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


After the above steps the virtual box image exported in the name of proserv kept into VBimg1

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

/--------------------------mysql setup--------------------/ 
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
mysql> create database `dbnameidm`;
```
#### 3. creating specific database for jcr

```
mysql> create database `dbnamejcr`;
```
#### 4. creating specific database for jpa

```
mysql> create database `dbnamejpa`;
```

#### 5. creating specific user to administer the database

```
mysql> CREATE USER 'dbuser'@'localhost' IDENTIFIED BY 'dbpwd';
```
#### 6. permiting the user to admin the database 
by executing following command
```
mysql> GRANT ALL ON `dbnameidm`.* TO 'dbuser'@'localhost' IDENTIFIED BY 'dbpwd'

mysql> GRANT ALL ON `dbnameidm`.* TO 'dbuser'@'192.168.%.%' IDENTIFIED BY 'dbpwd'


mysql> GRANT ALL ON `dbnamejcr`.* TO 'dbuser'@'localhost' IDENTIFIED BY 'dbpwd'

mysql> GRANT ALL ON `dbnamejcr`.* TO 'dbuser'@'192.168.%.%' IDENTIFIED BY 'dbpwd'


mysql> GRANT ALL ON `dbnamejpa`.* TO 'dbuser'@'localhost' IDENTIFIED BY 'dbpwd'

mysql> GRANT ALL ON `dbnamejpa`.* TO 'dbuser'@'192.168.%.%' IDENTIFIED BY 'dbpwd'
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
username@serverhost$ mysql -u dbuser -p
pwd: dbpwd
```

To check the mysql service status
```
username@serverhost$ systemctl status mysql.service
```


after this the virtualbox image exported on the same name into the directory of VBimg4 

/--------------------//mysql------------------------/

# Application server configurration

For this take up the VBimg2 and do the following 

#### JDK installation

1. start the server and login to it 
2. to install jdk execute the following command

```
username@serverhost$ sudo apt install default-jdk
pwd: 
```


after the installation complete we need to setup java HOME and PATH

#### JAVA HOME and PATH configuration

first need to find the java installed directory by executing the following command

```
username@serverhost$ whereis java

it will point the default installed directory, or ls .bashrc to locate and immediately executing the following comand

username@serverhost$ sudo nano .bashrc

it will take it into the file. reach the last line and add a new line

export JAVA_HOME=/usr
export PATH=$JAVA_HOME/bin:$PATH

```
after performing this exit and come back and check the java home is set by executing 

```
username@serverhost$ $JAVA_HOME
-bash: /usr: Is a directory
```

to check the version of java run the following command
```
username@serverhost$ java -version
openJDK version "1.8.0_171"
openJDK runtime environment ...
openJDK 64-Bit Server VM ...
```

after these steps the virtualbox image exported in the name of proserv and kept it in VBimg5 directory

# Application installation

After done all the bove process, start the servers and ping with ip to check connectivity.

#### Dirctory creation

Take the tomcat standalone bundle extract it and name it "la-intranet" and keep it to home directory.
to create dirctory in home directory

```
username@serverhost:/$ cd /home

username@serverhost:/home$ sudo mkdir la-intranet
```
To check the directory is created 

```
username@serverhost:/home$ ls
```
set the permission to read and right files so that application will be copied and configuration can be done.

to give permission to the directory, first come out from the directory

```
username@serverhost:/home$ cd /

username@serverhost:/$ sudo chmod -R 777 /home/la-intranet

username@serverhost:/$ cd home

username@serverhost:/home$ ls

```
after performing this, the dircory colour will be turn from blue to green.

use winscp or fillzilla to copy the application directory in to the la-intranet directory and again give above said permission to read and write it the files.

# Configuration

take up the application la.configuration file and do certain modification

modification 1
 - accountsetup.skip=true
modification 2
 1. change the datasource line "JNDI Name for JCR datasource"
 1.1 datasource.name=java:/comp/env/ uncommend
 modification 3
 1. change the datasource line "JNDI Name for IDM datasource"
 1.1 datasource.name=java:/comp/env/ uncommend
 
 modification 4
 1. change the datasource line "JNDI Name for JPA datasource"
 1.1 datasource... uncommend

save the modification

next take up server.xml file and do the following 

IDM Datasource for portal

username="dbuser" password="dbpwd"
url="jdbc:mysql://192.168.1.16:3306/dbnameidm?..."

JCR Datasource for portal

username="dbuser" password="dbpwd"
url="jdbc:mysql://192.168.1.16:3306/dbnamejcr?..."

JPA Datasource for portal

username="dbuser" password="dbpwd"
url="jdbc:mysql://192.168.1.16:3306/dbnamejpa?..."

and the last step is to drop mysql-jdbc connector.jar file into lib folder. 

completed the basic setup

start the server 






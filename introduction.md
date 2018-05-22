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

to change the settings (like the listening port, and root login permission) by editing 
```
username@serverhost$ sudo nano /etc/ssh/sshd_config
```
incase configured, after configuration restart the service by 
```
username@serverhost$ sudo service ssh restart
```



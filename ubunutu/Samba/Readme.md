# How to configure Samba Server share

## Objective:

The objective is to configure basic Samba server to share user home directories as well as provide read-write anonymous access to selected directory.  There are other possible Samba configurations, however the aim of this instruction set is to get you started with some basics which can be later expanded to implement more features to suit your needs.

## Conventionss:

* \# - requires given linux commands to be executed with root privileges either directly as a root user or by use of sudo command
* $ - requires given linux commands to be executed as a regular non-privileged user

## Scenario

The below configuration procedure will assume a following scenario and pre-configured requirements:

* Server and MS Windows client are located on the same network and no firewall is blocking any communication between the two
* MS Windows client can resolve samba server by hostname __ubuntu-samba__
* MS Windows client's Workgroup domain is __WORKGROUP__

## Instructions

Let's begin by installation of Samba server. This is rather a trivial task. First, install tasksel command if it s not available yet on your system. Once ready use tasksel to install Samba server.

```bash
$ sudo apt install tasksel
$ sudo tasksel install samba-server
```

## Configuration

We will be starting with a fresh clean configuration file, while we also keep the default config file as a backup for reference purposes. Execute the following linux commands to make a copy of an existing configuration file and create a new one:

```bash
$ sudo cp /etc/samba/smb.conf /etc/samba/smb.conf_backup
$ sudo bash -c 'grep -v -E "^#|^;" /etc/samba/smb.conf_backup | grep . > /etc/samba/smb.conf'
```

## Home Share

In this section we will be adding user home share directories into our new /etc/samba/smb.conf samba configuration file.

Samba has its own user management system. However, any user existing on the samba user list must also exist within __/etc/passwd__ file. If your system user does not exist yet, hence cannot be located within __/etc/passwd__ file, first create a new user using the __useradd__ command before creating any new Samba user. Once your new system user eg. linuxconfig exits, use the __smbpasswd__ command to create a new Samba user:

```bash
$ sudo smbpasswd -a linuxconfig
```

Next, use your favorite text editor to edit our new __/etc/samba/smb.conf__ samba configuration file:

```bash
$ sudo nano /etc/samba/smb.conf
```

add add the following lines:

```
[homes]
   comment = Home Directories
   browseable = yes
   read only = no
   create mask = 0700
   directory mask = 0700
   valid users = %S
```

## Create Anoymous Share

In this section we will add a new publicly available read-write Samba share accessible by anonymous/guest users. First, create a directory you wish to share and change its access permission. Example:

```bash
$ sudo mkdir /var/samba
$ sudo chmod 777 /var/samba/
```

Next, add the following lines into Samba configuration file using your favorite text editor

```bash
sudo nano /etc/samba/smb.conf
```
```
[public]
  comment = public anonymous access
  path = /var/samba/
  browsable =yes
  create mask = 0660
  directory mask = 0771
  writable = yes
  guest ok = yes
```

Your current Samba configuration file should look similar to the one below:

```
[global]
   workgroup = WORKGROUP
   server string = %h server (Samba, Ubuntu)
   dns proxy = no
   log file = /var/log/samba/log.%m
   max log size = 1000
   syslog = 0
   panic action = /usr/share/samba/panic-action %d
   server role = standalone server
   passdb backend = tdbsam
   obey pam restrictions = yes
   unix password sync = yes
   passwd program = /usr/bin/passwd %u
   passwd chat = *Enter\snew\s*\spassword:* %n\n *Retype\snew\s*\spassword:* %n\n *password\supdated\ssuccessfully* .
   pam password change = yes
   map to guest = bad user
   usershare allow guests = yes
[printers]
   comment = All Printers
   browseable = no
   path = /var/spool/samba
   printable = yes
   guest ok = no
   read only = yes
   create mask = 0700
[print$]
   comment = Printer Drivers
   path = /var/lib/samba/printers
   browseable = yes
   read only = yes
   guest ok = no
[homes]
   comment = Home Directories
   browseable = yes
   read only = no
   create mask = 0700
   directory mask = 0700
   valid users = %S
[public]
  comment = public anonymous access
  path = /var/samba/
  browsable =yes
  create mask = 0660
  directory mask = 0771
  writable = yes
  guest ok = yes
```

## Restart Samba Server

Our basic Samba server configuration is done. Remember to always restart your samba server, after any change has been done to __/etc/samba/smb.conf__ configuration file:

```bash
sudo systemctl restart smbd
```

Once you restart your Samba server, confirm that all shares have been configured correctly.

```bash
$ smbclient -L localhost
```

Optionally create some test files. Once we successfully mount our Samba shares, the below files should be available to our disposal:

```bash
$ touch /var/samba/public-share 
$ touch /home/linuxconfig/home-share
```

Lastly, confirm that your Samba server is up and running.

```bash
$ sudo systemctl status smbd
```


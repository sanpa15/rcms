sophos vpn username pandi
password:Lemon@11PANDI

**SSHD CONFIGURATION FOR CENTOS 7**

OpenSSH : Password Authentication

 First we take remote server via ssh using below command
 ```bash
 ssh -l remoteservername ip address
 ```
 
 ![image](https://user-images.githubusercontent.com/98270930/166197608-0c13c5e5-e2c8-40bb-8e5f-163a10108600.png)

 ![image](https://user-images.githubusercontent.com/98270930/166194186-ac507332-fb59-4a99-89b1-4bd49ea93723.png)


# Prohibit root login
 
 vim /etc/ssh/sshd_config

 line 38 uncomment and change 

 PermitRootLogin no

 ![image](https://user-images.githubusercontent.com/98270930/166194889-07646ee5-5969-4a0c-a4fe-754beb26f312.png)
 
# Enable user limit

 vim /etc/ssh/sshd_config
 
 add the below parameter in sshd_config file
 
 AllowUsers username
 
 save and exit
 
 ![image](https://user-images.githubusercontent.com/98270930/166196937-89742a93-2444-4770-a346-e41dedb10608.png)

 
![image](https://user-images.githubusercontent.com/98270930/166196753-c944ba88-bb83-4613-ab5b-fec2663cd099.png)

# Enable group limit

 vim /etc/ssh/sshd_config

 add the userslist group in sshd_config file
 
  ![image](https://user-images.githubusercontent.com/98270930/166199157-75fe11ae-7edb-466b-9ed3-d98d1715b29c.png)
  
  
# OpenSSH : SSH File Transfer (CentOS Client)

  using scp command
  
  file transfer between local host to remote (cent) host
  
  ![image](https://user-images.githubusercontent.com/98270930/166200848-1bad9478-2e88-4d7c-b830-3241bed9aab6.png)
    
  file transfer between remote to local
  
  ![image](https://user-images.githubusercontent.com/98270930/166201607-13570f04-580a-4603-bba6-2d434be09218.png)

  
  Using sftp command
  
  ![image](https://user-images.githubusercontent.com/98270930/166209075-93383c9b-9b26-4915-8d28-8c65df733271.png)


# OpenSSH : SSH Key-Pair Authentication





    

#   how to allowing the particular userrs in ssh ervice

   ssh-keygen -R remote ip

   how to given ssh access in particular users in linux centos
   vi etc/ssh/sshd_ config file then then add AllowUsers username(user1)
   then save and exit then systemctl restart sshd  now (user1) only access the ssh service
   vi/etc/ssh/sshd_config file then add DenyUsers username (user1)
   then save and exit  systemctl  restart sshd   the user1 not access ssh service
                OR

   echo "AllowUsers user3" >> /etc/ssh/sshd_config
   systemctl restart sshd

   echo “DenyUsers  user3”  >> etcssh/sshd_config
   systemctl restart sshd

   at the same for group  AllowGroups  groupname   AllowGroups grp1 grp2 grp3

   (at a time allowusers &allowgroups) only one is enable  or working



  #  how to give sudo access for particular user  in centos7
     
     user adding in sudoers group
     sudo usermod -aG sudo username for ubuntu
     sudo usermod -aG wheel username for centos
     or 
     editing /etc/sudoers file
     vi /etc/sudoers  or visudo command 
     then use visudo command or etcsudoers file  add
      root ALL=(ALL) ALL

     UserName ALL=(ALL) ALL
     save and exit
     
     
     HOW TO ASSIGN STATIC IP 
     first knowing the gateway and subnetmask in yuor network using route -n command
     route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         192.168.0.1     0.0.0.0         UG    100    0        0 enp0s9
0.0.0.0         192.168.0.1     0.0.0.0         UG    101    0        0 enp0s10
0.0.0.0         192.168.1.1     0.0.0.0         UG    300    0        0 bond0
192.168.0.0     0.0.0.0         255.255.255.0   U     100    0        0 enp0s9
192.168.0.0     0.0.0.0         255.255.255.0   U     101    0        0 enp0s10
192.168.0.0     0.0.0.0   


then go to configuration file use vim /etc/sysconfig/network-scripts/ifcfg-enp0s10

   [root@localhost ~]# cd /etc/sysconfig/network-scripts/
[root@localhost network-scripts]# ll
total 244
-rw-r--r--. 1 root root     0 Apr 11 09:25 enp0s9
-rw-r--r--. 1 root root   202 Apr 11 03:36 ifcfg-bond0
-rw-r--r--. 1 root root   317 Apr 11 03:38 ifcfg-enp0s3
-rw-r--r--. 1 root root   317 Apr 11 03:40 ifcfg-enp0s8
-rw-r--r--. 1 root root   375 Apr 11 09:46 ifcfg-enp0s9
the ifcfg-enp0s10 file is not there you will create the file cat ifcfg-enp0s3 > ifcfg-enp0s10
vim ifcfg-enp0s10 add the some content edit enp0s3 to enps10 and   
using nmcli  c command copy uid in enp0s10
                                       BOOTPROTO=static
                                       IPADDR=your range 
                                       GATEWAY=your first ip
                                       NETMASK=verify the subnet range 22/24/23
                                       DNS1=192.168.0.1
                                       DNS2=8.8.8.8  save exit
  finaly edit vim /etc/resolve.conf
      nameserver=192.168.0.1    your gateway
      nameserver=8.8.8.8   save and exit
      finaly restart the service systemctl restart network and use ifup enp0s10 command
      
     
     
     HOW TO NETWORK  CONFIGURE NETWORK BONDING 
     modprobe bonding
modinfo bonding
Step:1 Create Bond Interface File
Create a bond interface file (ifcfg-bond0) under the folder “/etc/sysconfig/network-scripts/”

DEVICE=bond0
TYPE=Bond
NAME=bond0
BONDING_MASTER=yes
BOOTPROTO=none
ONBOOT=yes
IPADDR=192.168.3.226
NETMASK=255.255.252.0
GATEWAY=192.168.1.1
BONDING_OPTS="mode=1 primary=enp0s3 miimon=100"
ZONE=public

then edit enp0s3 file
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=none
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=enp0s3
UUID=6919a910-629b-4bbc-be3f-04f09e6054c6
DEVICE=enp0s3
ONBOOT=yes
MASTER=bond0
SLAVE=yes
ZONE=public

then edit enps8 file

TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=none
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FtartATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=enp0s8
UUID=ff4d147c-e442-3042-8a82-4cff78c9ee8d
DEVICE=enp0s8
ONBOOT=yes
MASTER=bond0
SLAVE=yes
ZONE=public   


finally restart the service systemctl restart network .service   & check ip a
then down the inteface 1 enp0s3 using  ifdown enp0s3 command restart the service


  HARD DISK NORMAL PARTITION
  Lsblk      listout hard disks
fdisk /dev/sdb    choose  mounted harddisk
Command (m for help): m
Command action
   a   toggle a bootable flag
   b   edit bsd disklabel
   c   toggle the dos compatibility flag
   d   delete a partition
   g   create a new empty GPT partition table
   G   create an IRIX (SGI) partition table
   l   list known partition types
   m   print this menu
   n   add a new partition
   o   create a new empty DOS partition table
   p   print the partition table
   q   quit without saving changes
   s   create a new empty Sun disklabel
   t   change a partition's system id
   u   change display/entry units
   v   verify the partition table
   w   write table to disk and exit
   x   extra functionality (experts only)


 p   primary (1 primary, 0 extended, 3 free)
   e   extended
Select (default p): p
Partition number (2-4, default 2): 2
First sector (2099200-16777215, default 2099200): 
Using default value 2099200
Last sector, +sectors or +size{K,M,G} (2099200-16777215, default 16777215): +2G
Partition 2 of type Linux and of size 2 GiB is set

Command (m for help): p

Disk /dev/sdb: 8589 MB, 8589934592 bytes, 16777216 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x9538e5cb

   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048     2099199     1048576   83  Linux
/dev/sdb2         2099200     6293503     2097152   83  Linux

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.

WARNING: Re-reading the partition table failed with error 16: Device or resource busy.
The kernel still uses the old table. The new table will be used at
the next reboot or after you run partprobe(8) or kpartx(8)
Syncing disks
  REBOOT THE MACHINE

e4bba5c8-d45e-485d-aec4-dd2d4ef314f6

[cent1@localhost ~]$ su -
Password: 
Last login: Mon Apr 11 05:13:01 EDT 2022 on pts/0
[root@localhost ~]# cd /
[root@localhost /]# pwd
/
[root@localhost /]# mkdir test2
[root@localhost /]# cd t
test/  test2/ tmp/   
[root@localhost /]# cd test2
[root@localhost test2]# mount /dev/sdb2   /test2
mount: /dev/sdb2 is write-protected, mounting read-only
mount: unknown filesystem type '(null)'
[root@localhost test2]# mkfs
mkfs         mkfs.btrfs   mkfs.cramfs  mkfs.ext2    mkfs.ext3    mkfs.ext4    mkfs.minix   mkfs.xfs     
[root@localhost test2]# mkfs.ext4 /dev/sdb2
 [root@localhost test2]# mount /dev/sdb2   /test2
[root@localhost test2]# df -Th
Filesystem              Type      Size  Used Avail Use% Mounted on
devtmpfs                devtmpfs  484M     0  484M   0% /dev
tmpfs                   tmpfs     496M     0  496M   0% /dev/shm
tmpfs                   tmpfs     496M  6.8M  489M   2% /run
tmpfs                   tmpfs     496M     0  496M   0% /sys/fs/cgroup
/dev/mapper/centos-root xfs       6.2G  1.6G  4.7G  25% /
/dev/sdb1               xfs      1014M   33M  982M   4% /test
/dev/sda1               xfs      1014M  168M  847M  17% /boot
tmpfs                   tmpfs     100M     0  100M   0% /run/user/1001
/dev/sdb2               ext4      2.0G  6.0M  1.8G   1% /test2
[root@localhost test2]# ls -lt /dev/disk/by-uuid/
total 0
lrwxrwxrwx. 1 root root 10 Apr 11 05:18 e4bba5c8-d45e-485d-aec4-dd2d4ef314f6 -> ../../sdb2
lrwxrwxrwx. 1 root root 10 Apr 11 05:12 2ca750b1-5051-4f76-be08-c10d32e8e0ca -> ../../dm-1
lrwxrwxrwx. 1 root root 10 Apr 11 05:12 2e68f12e-ba87-4c82-bfcd-f6d7e1333bc2 -> ../../dm-0
lrwxrwxrwx. 1 root root 10 Apr 11 05:12 906b81fb-da47-4128-8615-78c8169252e2 -> ../../sda1
lrwxrwxrwx. 1 root root 10 Apr 11 05:12 8f6adbab-a908-4829-814b-d23129c90281 -> ../../sdb1
[root@localhost test2]# cd
[root@localhost ~]# cd /
[root@localhost /]# umount /test2 
[root@localhost /]# df -Th

root@localhost /]# vim /etc/fstab 
/etc/fstab
# Created by anaconda on Fri Apr  8 23:39:24 2022
#
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
/dev/mapper/centos-root /                       xfs     defaults        0 0
UUID=906b81fb-da47-4128-8615-78c8169252e2 /boot                   xfs     defaults        0 0
/dev/mapper/centos-swap swap                    swap    defaults        0 0
UUID=8f6adbab-a908-4829-814b-d23129c90281  /test   xfs   defaults  0 0 
UUID=e4bba5c8-d45e-485d-aec4-dd2d4ef314f6  /test2  ext4   defaults   0 0
~                                                                            

reboot machine


LVM CREATION   
sudo lsblk
sudo fdisk /dev/sdc1   mfor help n option p option  t option w option partprobe
  161  sudo pvcreate /dev/sdc1      (volumegroup  volume name offvg1
  162  sudo pvdisplay               (logical volume name offlv1)
  163  sudo vgcreate offvg1 /dev/sdc1
  164  sudo vgdisplay
  165  sudo pvdisplay
  166  sudo lvcreate -L +512MB -n offlv1 offvg1
  167  sudo mkfs.xfs /dev/offvg1/offlv1 
  168  sudo mount /dev/offvg1/offlv1 /test4
  169  df -TH
  170  sudo mv download.mp4 /test4
  171  df -TH
  172  sudo lvdisplay
  173  sudo lvresize -L +512MB /dev/offvg1/offlv1 
  174  df -Th
  175  sudo resize2fs /dev/offvg1/offlv1 
  177  sudo lvremove /dev/offvg1/offlv1 
  178  sudo lvcreate -L +512MB -n off1 offvg1
  179  sudo mkfs.xfs /dev/offvg1/off1 
  180  sudo mount /dev/offvg1/off1 /test4
  181  mv download.mp4 /test4
  182  sudo mv download.mp4 /test4
  183  df -Th
  184  sudo lvextend -L +512MB /dev/offvg1/off1 
  185  sudo resize2fs /dev/offvg1/off1 
  186  sudo -i
  187  sudo resize2fs /dev/offvg1/off1 
  188  sudo xfs_growfs /dev/offvg1/off1 
  189  df -Th
  190  mv download.mp4  /test4/
  191  sudo mv download.mp4  /test4/
  192  sudo -i
  193  sudo lvresize -L -200MB /dev/offvg1/off1 
  195  df -Th
  196  sudo lvdisplay
  197  sudo xfs_growfs /dev/offvg1/off1
  200  df -TH
  201  sudo mount /dev/offvg1/off1 /test4
  202  sudo lsblk 
  203  sudo mount /dev/offvg1/off1 /test4
  204  sudo lvdisplay
  205  sudo lvremov /dev/offvg1/off1 
  209  sudo lvdisplay
  
 FTP  CONFIGURATION
BELOW CHANGES IN SERVER SIDE CONFIGURATION
 install vsftpd package
 checking the status
 enable the package vsftpd and start the vsftpd
 edit the configuration file /etc/vsftpd/vsftpd.conf
 some changes and add some content 
 anonymous_enable=NO
 local_enable=YES
 local_enable=YES
 local_umask=022
 dirmessage_enable=YES
 xferlog_enable=YES if YES is given the log messages write in xferlog filr(/etc/vsftpd/xferlog)
 connect_from_port_20=YES
 xferlog_std_format=YES
 uncomment  ftpd_banner=Welcome to blah FTP service.
 uncomment ascii_upload_enable=YES
 ascii_download_enable=YES
 uncomment chroot_list_file=/etc/vsftpd/chroot_list  create chrrot_list file add your users
 uncomment  chroot_local_user=YES
 chroot_list_enable=YES
 uncoment ls_recurse_enable=YES
 vsftpd_log_file=/var/log/vsftpd.log create var/log vsftpd.log file
 pam_service_name=vsftpd
userlist_enable=YES
userlist_deny=NO
tcp_wrappers=YES
allow_writable_chroot=YES
user_config_dir=/etc/vsftpd/users
userlist_file=/etc/vsftpd/users/keypass  adding the user to keypass file
create user and passwd for user
create one directory and change the ownership on this directory
chown user:user dir name
#####CENT OS DEFAULT ENABLE FOR SELINUX SO DISABLE IT vim /etc/selinux/config ######
use command setenforce 0
edit /etc/vsftpd/vsftpd.conf in xferlog_stdformat=NO   if NO is given log messages write in vsftpd.log(etc/vsftpd/vsftpd.log)   
  
     CLIENT SIDE CONFIGURATION
     
 install ftp service 
 and connect server server  ftp server ip
 
 
 SAMBA	
windows machine via connect or communicate network to linux machine using samba service
SAMBA SERVER SIDE CONFIGURATION
yum update & yum install samna samba-client samba-common
systemctl status smb
systemctl status nmb
systemctl start smb nmb
systemctl enable smb nmb
cd /  change root directory
create one group groupadd honey  cat/etc/group
create users useradd smbuser1 useradd smbuser2 passwd smbuser1 passwd smbuser2
add the user to the honey group usermod -aG honey smbuser1
create one directory in root cd /
mkdir honeybee
chmod 0770 honeybee/
chgrp honey honeybee/

 now go to smb configuration file vim /etc/samba/smb.conf  (backup first)
 global nochange
 comment #home
 comment #printers
 comment #prints
 add the below content
 [honey]
comment=Directory for collaboration of the company's finance team
browsable=yes
path=/honeybee
public=no
valid users=@honey
write list=@honey
writeable=yes
create mask=0770
Force create mode=0770
force group=honey
save and exit
systemctl retart smb nmb
then create samba password for users smb -a smbuser1 smb -a smbuser2
finaly checking your local server
smbclient -L localhost -U smbuser1 
enter user password
 share the files
 samba has no log file so enable logfile
 
samba log in global section
============================
# --------------------------- Logging Options -----------------------------
adding below the globle contents

        vfs objects = full_audit
        full_audit:prefix = %u|%I|%m|%S
        full_audit:success = mkdir rename unlink rmdir pwrite
        full_audit:failure = none
        full_audit:facility = local7
        full_audit:priority = NOTICE
        
        Edit the rsylogfile in vim/etc/rsyslog.conf
        add the content
#local7.*                        /var/log/samba/log.audit
        local7.* /var/log/samba.log
        
        
systemctl resart rsyslog


 Configuring SELinux and Firewalld for SAMBA

# setsebool -P samba_export_all_ro=1 samba_export_all_rw=1
# getsebool -a | grep samba_export
# semanage fcontext -at samba_share_t "/finance(/.*)?"
# restorecon /finance

FIREWALLD:

# firewall-cmd --permanent --add-service=samba
# firewall-cmd --reload
   
 su administrator
6Jm3=*123

OPEN VPN CONFIGURATION FOR USERS



first go to users  terminal go to desktop create sslvpn directory use pwd command then paste the location of user presentworking directory

/home/ubuntu/Desktop/sslvpn

then using which openvpn command and paste the text in that location

/usr/sbin/openvpn

then using whoami command then paste text in that location
  ubuntu 
  then login administrator user 
  
  then go to visudo file and add the parameter
  
  #includedir /etc/sudoers.d
username ALL=NOPASSWD:/usr/sbin/openvpn
user name =your username
ubuntu  ALL=NOPASSWD:/usr/sbin/openvpn

exit admin user

using  alias command to copy the users file in text
sudo vim ~/.bashrc
# This VPN Connecion aliase
alias rits='sudo openvpn --config /home/ubuntu/sslvpn/vijayarajan.conf'

alias vpn='sudo openvpn --config /home/rcms-lab-065/Desktop/sslvpn/your
 users.conf'
 save and exit
 use this command source ~/.bashrc
 
 THEN GO TO users desktop>sslvpn type vpn ask username password
 
 
 file:///home/ubuntu/Pictures/Screenshot%20from%202022-04-20%2014-05-17.png
 
 
 file:///home/ubuntu/Pictures/Screenshot%20from%202022-04-20%2013-59-17.png
 
 
 CENTOS7 07 vhost configuration

(server world) reference
edit userdir.conf

remove welcome.conf file before remove it and create vhost.conf file

/etc/httpd/conf.d/vhost.conf 

add this content in vhost.conf file
#orginal domain

<VirtualHost *:80>
   DocumentRoot /var/www/html this location add the index.html file
   ServerName www.rc.io  your domain name
</VirtualHost> 

 #virtual domain

<VirtualHost *:80>
   DocumentRoot /var/www/vhtml this location create index.html file
   ServerName www.vrc.io
   ServerAdmin webmaster@vrc.io   your domain name
   ErrorLog logs/vrc-error_log
   CustomLog logs/vrc-access_log combined
   </virtualHost>
   
# IF WANT MORE DOMAIN ADD ABOVE PARAMETER ONLY

then edit /etc/hosts file 
add your ip and your website ex: 192.168.3.234   www.test1.com

go to web browser enter your domain name show your index.html file text
 

 CENT OS vsftpd configuration


vim /etc/vsftpd/vsftpd.conf

anonymous_enable=NO
local_enable=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
xferlog_enable=YES
xferlog_std_format=YES
ascii_upload_enable=YES
ascii_download_enable=YES
ftpd_banner=Welcome to Radiant FTP service.
listen=YES
listen_ipv6=NO

chroot_list_file=/etc/vsftpd/vsftpd.chroot_list
pam_service_name=vsftpd
userlist_enable=YES
tcp_wrappers=YES
use_localtime=YES
user_config_dir=/etc/vsftpd/users/
allow_writeable_chroot=YES
chroot_local_user=YES
chroot_list_enable=NO


create /etc/vsftpd/vsftpd.chroot_list file add the user this file
create directory mkdir /etc/vsftpd/users/


create user usera

create the directory  jino for /var/www/html/jino/
change the owenership the jino directory in chown usera:usera jino

nc -vz 192.168.158.118 22


no route to host error  systemctl stop firewalld

use the command setenforce 0

CENT OS VSFTPD WITH SSL/TLS
anonymous_enable=NO
local_enable=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
xferlog_enable=YES
xferlog_std_format=YES
ascii_upload_enable=YES
ascii_download_enable=YES
ftpd_banner=Welcome to Radiant FTP service.
listen=YES
listen_ipv6=NO
listen_port=201
chroot_list_file=/etc/vsftpd/vsftpd.chroot_list
pam_service_name=vsftpd
userlist_enable=YES
tcp_wrappers=YES
use_localtime=YES
user_config_dir=/etc/vsftpd/users/
allow_writeable_chroot=YES
chroot_local_user=YES
chroot_list_enable=NO

rsa_cert_file=/etc/ssl/private/vsftpd.pem
rsa_private_key_file=/etc/ssl/private/vsftpd.pem

ssl_enable=YES
allow_anon_ssl=NO
force_local_data_ssl=YES
force_local_logins_ssl=YES

ssl_tlsv1=YES
ssl_sslv2=NO
ssl_sslv3=NO
require_ssl_reuse=NO
ssl_ciphers=HIGH






 


     

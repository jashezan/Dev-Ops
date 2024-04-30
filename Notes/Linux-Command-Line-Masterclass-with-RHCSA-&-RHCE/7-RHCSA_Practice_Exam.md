			                            RHCSA EXAM	(Passing - 210/300 , Duration 2.5Hrs)  
                       Physical Machine -	Non-root user .	(username , password provided with / after license agreement) 
                         
                         

Virtual Machine 	   <br>

Additional Information:   <br>

IP ADDR	-	172.25.X.11   <br>

GATEWAY-	172.25.X.254   <br>

HOSTNAME -	serverX.example.com    <br>

DNS/NAME SERVER -172.25.254.254   <br>

new root psswd -	postroll   <br>

YUM Server:- http://content.example.com/rhel8.0/x86\_64/dvd/   <br>

<br>


-------  <br>


\*\*\*\*\*u have to reset your root passwd\*\*\*\*\*  <br>

<br>

	1:-jst restart your system  <br>
  
	2:-press up down arrow   <br>
  
	3:-press e  <br>
  
	4:-go to end of linux16 line   <br>
  
	5:-jst give space and type rd.break  <br>
  
	and remove that both words ( console=tty console=ttys0)  <br>
  
	6:-ctrl+x  <br>
  
	  <br>
    
 #mount -oremount,rw /sysroot   <br>
 
 #chroot /sysroot   <br>
 
 #passwd root   <br>
 
 type new passwd:-redhat   <br>
 
 retype same passwd:-redhat   <br>
 
 #touch /.autorelabel   <br>
 
 #exit   <br>
 
 #exit   <br>
 
<br>

   <br>
   
<br>


----------------------------------  <br>


   <br>
   
<br>

  #login name:root   <br>
  
  #Password:   <br>
  
  #systemctl isolate graphical.target<br>
     <br>
  
    <br>
    
<br>

----------------------------------  <br>


   <br>
   
<br>

Set proper network #   <br>

  #nmcli con  show           <br>
                                           <br>
  
  #nmcli con mod eth0 ipv4.addresses '172.25.X 11/24 172.25.X.254' ipv4.dns '172.25.254.254' connection.autoconnect yes ipv4.method manual<br>
                                                   <br>
  
  #nmcli con up eth0   <br>
  
  #hostnamectl set-hostname serverX.example.com   <br>
  
  #ping 172.25.254.254   <br>
  
  #ping 172.25.X.254   <br>
  
<br>

   <br>
   
<br>

----------------------------------  <br>


<br>

<br>

  
1. SELINUX  <br>

- et selinux to enforcing mode   <br>

<br>

  #sestatus   <br>
  
  #setenforce 1   <br>
  
  #vim /etc/selinux/config   <br>
  
  insert   <br>
  
     SELINUX=enforcing   <br>
     
  :wq   <br>
  
  #sestatus   <br>
  
<br>

   <br>
   
<br>

<br>

<br>


2)Create a repo by using url http://content.example.com/rhel7.0/x86\_64/dvd   <br>

  #cd /etc/yum.repos.d   <br>
  
  #vim base.repo   <br>
  
(insert)   <br>

   \[1\]   <br>
   
   name=redhat   <br>
   
   baseurl=http://content.example.com/rhel7.0/x86\_64/dvd/   <br>
   
   enabled=1   <br>
   
   gpgcheck=0   <br>
   
:wq   <br>

  #yum clean all  <br>
  
  #yum repolist (it should be hving almost 4305 rpm packages)  <br>
  
<br>

<br>

<br>
   <br>

<br>


----------------------------------  <br>


<br>

2. LVM  <br>


- xtend your /mnt/lvm to 950M   {ans should between 850M-960M}  <br>

<br>

 #df -hT /mnt/lvm  <br>
 
   <br>
   
 #lvextend  -r -L 950M /dev/mapper/vggroup/wshare   <br>
 
<br>

 #df -h /mnt/lvm  <br>
 
<br>

<br>

<br>



---------  <br>


3. Usermgmt  <br>

- reate a group sysadmin .Create users tom , jerry  & harry .  <br>

- sers tom & jerry should be a part of the sysadmin group.   <br>

- arry should not be a part of that group.  <br>

- e is given an non interactive shell.password of all users are "password"			                  <br>

      #groupadd sysadmin  <br>
      
      #useradd -G sysadmin tom  <br>
      
      #useradd -G sysadmin jerry  <br>
      
      #useradd -s /sbin/nologin harry  <br>
      
      #su - harry  <br>
      
      #echo "password" |passwd --stdin tom                           #passwd tom  <br>
      
      #echo "password" |passwd --stdin harry             OR          #passwd harry   <br>
      
      #echo "password" |passwd --stdin jerry                         #passwd jerry                       <br>
      
											     <br>
                           
<br>

<br>

<br>

<br>

<br>

---------  <br>


4. ACL  <br>

- opy /etc/fstab to /var/tmp/fstab.  <br>

- he user & group owner of that file should be root .  <br>

- ll users should be able to read the file,No one should be able to execute.  <br>

- om should not be able to read nor write . jerry should be able to read & write.  <br>

<br>

      #cp /etc/fstab /var/tmp/fstab  <br>
      
      #getfacl /var/tmp/fstab  <br>
      
      #setfacl -m u:tom:- /var/tmp/fstab  <br>
      
      #setfacl -m u:jerry:rw /var/tmp/fstab  <br>
      
      #getfacl /var/tmp/fstab  <br>
      
<br>

<br>

<br>

<br>

<br>

<br>

<br>

----  <br>


5. direct collaboration special permission  <br>

- reate directory  /data such that only group sysadmin have all access on it .  <br>

- roup owner should be sysadmin,all child directories files by default should have group sysadmin.  <br>

<br>

<br>

     #mkdir /data  <br>
     
     #ls -ld /data  <br>
     
     #chgrp sysadmin /data  <br>
     
     #chmod 2070 /data  <br>
     
     #cd /data  <br>
     
     #touch p1 p2 p3   <br>
     
     #ll  <br>
     
<br>

<br>

<br>

<br>

<br>

---------  <br>


6. LDAP  <br>

- onfigure an ldap client,  <br>

- uthenticate your ldap server using the certificate http://classroom.example.com/pub/example-ca.crt  <br>

<br>

<br>

<br>

      # yum install authconfig-gtk sssd  <br>
      
      # authconfig-gtk  <br>
      
       User Acc   <br>
       
 		user acc database:- LDAP  <br>
     
		LDAP search base DN:-dc=example,dc=com  <br>
    
		LDAP Server:- classroom.example.com  <br>
    
		\*use TLS to encrypt connection  <br>
    
		download CA certificate  <br>
    
		http://classroom.example.com/pub/example-ca.crt  <br>
    
		password:LDAP password  <br>
    
<br>

           apply  <br>
           
<br>

	 #ssh  ldapuserX@localhost  <br>
   
         #password:password  <br>
         
	 #pwd  <br>
   
	#exit  <br>
  
<br>

<br>

<br>

<br>

<br>

<br>

----------------------------------  <br>


7. NTP  <br>

- onfigure Your NTP server should be synchronize with classroom.example.com  <br>

	#timedatectl  <br>
  
	#vim /etc/chrony.conf  <br>
  
	remove 4 line of iburst  <br>
  
        then enter server name  <br>
        
       (insert)  <br>
       
                      server   classroom.example.com   iburst  <br>
                      
	:wq  <br>
  
	#systemctl restart chronyd  <br>
  
	#chronyc sources -v  <br>
  
	#timedatectl  <br>
  
<br>

<br>

<br>

<br>

<br>

---------------------------------  <br>

 
8. Autofs  <br>

- ou can use the user ldapuserX,with the passwd password.  <br>

- ome directories for your LDAP users should be automatically mounted on access.   <br>

- hese home directories are served from the NFS share classroom.example.com:/home/guests/ldapuserX   <br>

   <br>
   
<br>

   #yum -y install autofs  <br>
   
   #systemctl enable autofs  <br>
   
   #systemctl start autofs  <br>
   
   #vim /etc/auto.master  <br>
   
   insert  <br>
   
   /home/guests	/etc/auto.misc  <br>
   
   :wq  <br>
   
   #vim /etc/auto.misc  <br>
   
   insert  <br>
   
    ldapuserX    -rw,sync      classroom.example.com:/home/guests/ldapuserX  <br>
    
  :wq  <br>
  
   #systemctl restart autofs  <br>
   
   #ssh ldapuserX@localhost  <br>
   
   password:-password  <br>
   
   #pwd  <br>
   
   check home dir:- /home/guests/ldapuserX  <br>
   
<br>

<br>

<br>

<br>

<br>

<br>

---------------------------  <br>

<br>


9. Kernel  <br>

- nstall the kernel from the site  http://content.example.com/rhel7.0/x86\_64/errata  <br>

- he newly installed kernel should be default kernel .  <br>

- he previous kernel should be available & bootable at grub .  <br>

<br>

 	#cd /etc/yum.repos.d  <br>
   
	#vim kernel.repo  <br>
  
        insert  <br>
        
       \[update\]  <br>
       
        name=update  <br>
        
        baseurl=http://content.example.com/rhel7.0/x86\_64/errata/  <br>
        
        enabled=1  <br>
        
        gpgcheck=0  <br>
        
        :wq  <br>
        
        #yum clean all  <br>
        
        #yum repolist (it should be hving almost 3 rpm packages  <br>
        
        #yum -y update  <br>
        
        #reboot	  <br>
        
<br>

<br>

<br>

<br>

<br>

		  <br>
      
--------------------------  <br>


10. cron  <br>

- dd a cron job for the user jane /bin/echo hiya .  <br>

- his job should run every day at 1:30pm .  <br>

	# useradd jane  <br>
  
	#crontab -e  -u jane  <br>
  
	 insert   <br>
   
	30 13 \* \* \* /bin/echo hiya  <br>
  
	:wq		  <br>
  
	#crontab -l  -u jane  <br>
  
<br>

<br>

<br>

<br>

<br>

<br>

---------------------------  <br>


11. swap  <br>

Create a Swap partition of 100MB mount it persistantly  <br>

	#fdisk -l   <br>
  
	#fdisk /dev/vda  <br>
  
	n  <br>
  
	e  <br>
  
	1st sector :-  <br>
  
	last sector:-  <br>
  
	n  <br>
  
	1st sector :-  <br>
  
	last sector:-+100M  <br>
  
	t  <br>
  
	82  <br>
  
	w  <br>
  
	#partprobe  <br>
  
	#mkswap /dev/vda5  <br>
  
	#swapon /dev/vda5  <br>
  
	#blkid  <br>
  
	#vim /etc/fsatb  <br>
  
	insert  <br>
  
	UUID="XXXXXXXXXXXXX"	swap 	swap	defaults 0 0  <br>
  
	:wq  <br>
  
	#mount -a  <br>
  
	#swapon -s		  <br>
  
	#free -h	  <br>
  
<br>

<br>

------  <br>


12.  <br>

- ynchronize the /etc directory to the /configbackup directory  <br>

- reate an archive named /root/configuration-backup-server.tar.gz with the /configbackup directory as content.  <br>

<br>

<br>

	# rsync -av   /etc   /configbackup  <br>
  
	#tar czf /root/configuration-backup-sever.tar.gz /configbackup  <br>
  
	  <br>
    
<br>

<br>

<br>

<br>

--  <br>


13. UID  <br>

- ser of Specific UID  <br>

- reate a user jean .User id of this user should be 3564 .  <br>

<br>

	# useradd  -u 3564 jean  <br>
  
	# id jean         <br>
  
<br>

<br>

<br>

<br>

<br>



14. find  <br>

- ocate all files owned by user sasha & copy them to /home/lost+found/  <br>

	#id sasha   <br>
  
	#mkdir /home/lost+found  <br>
  
<br>

	# find / -user sasha -exec cp -vp {} /home/lost+found/  \\;  <br>
  
			   <br>
         
<br>

<br>

<br>

<br>

<br>

-----------------------------------------  <br>


<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>

<br>


15. lvm  <br>

- reate a logical volume lvmgroup  from the volume group v2group .  <br>

- he volume group should have extents of size 16 . The logical volume  <br>

- hould have 100 extents . Mount this lvm as /mnt/lvm2 .  <br>

<br>

	#fdisk /dev/vda  <br>
  
	n  <br>
  
	1st sector:-  <br>
  
	last sector:-+2G  <br>
  
	t   <br>
  
	8e  <br>
  
	p  <br>
  
	w  <br>
  
	#partprobe  <br>
  
	#pvcreate /dev/vda5  <br>
  
	#vgreate -s 16M v2group /dev/vda5  <br>
  
	#lvcreate -l 100 -n lvmgroup v2group  <br>
  
	#mkfs -t xfs /dev/mapper/v2group/lvmgroup  <br>
  
	#mkdir /mnt/lvm  <br>
  
	#blkid  <br>
  
	#vim /etc/fstab  <br>
  
	insert  <br>
  
	UUID....	/mnt/lvm	xfs	defaults 0 0  <br>
  
	:wq  <br>
  
	#mount -a  <br>
  
	#df -h  <br>
  
<br>

<br>

<br>

<br>

<br>

----------------------  <br>


16.  <br>

- iplay the first 12 lines of the /usr/bin/clean-binary-file and send the output to the /home/student/headtail.txt file.  <br>

<br>

	#head -n 12 /usr/bin/bin/clean-binary-files > /home/student/headtail.txt  <br>
  
<br>

<br>

<br>

<br>

------------  <br>

<br>

<br>

------------------------------------  <br>


17  <br>

.Record the command to display all systemd journal entries recorded between 9:05:00 and 9:15:00 in the /home/student/systemdreview.txt  <br>

ans:-  <br>

	#echo "journalctl --since 9:05:00 --until 9:15:00" > /home/student/systemdreview.txt  <br>
  
	#cat /home/student/systemdreview.txt  <br>
  
<br>

<br>

<br>

<br>


18.  <br>

Deny user sasha to create cron job for herself.  <br>

	#vim /etc/cron.deny  <br>
  
	insert  <br>
  
	sasha   <br>
  
	:wq  <br>
  
	#su - sasha  <br>
  
	$crontab -e  <br>
  
	ERROrrrrrr  <br>
  
<br>

<br>

<br>

<br>


19. grep  <br>

- earch for the text 'strato' in the file /usr/share/dict/words & copy this to a file  <br>

-/root/lines.txt .The new file should not have any blank spaces or lines .  <br>

- he order of the names in the new file should be same as the order in which it appears   <br>

in the original file  <br>

<br>

	 # grep  strato /usr/share/dict/words > /root/lines.txt  <br>
   
<br>

<br>

<br>

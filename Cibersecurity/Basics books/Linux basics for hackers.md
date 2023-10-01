

# Linux basics for hackers #

## 1 GETTING STARTED WITH THE BASICS ##

### FILES STRUCTURE ###
![[Captura de pantalla_2023-07-25_22-51-52.png]]

/root The home directory of the all powerful root user
/etc Generally contains the Linux configuration files—files that control when and how
programs start up
/home The user’s home directory
/mnt Where other filesystems are attached or mounted to the filesystem
/media Where CDs and USB devices are usually attached or mounted to the filesystem
/bin Where application binaries (the equivalent of executables in Microsoft Windows)
reside
/lib Where you’ll find libraries (shared programs that are similar to Windows DLLs)


### BASIC COMMANDS IN LINUX ###
>pwd (actual directory)
>whoami(who am I , user)
>cd /etc (moving in folders) 
>ls (list files)
>ls -l (list long)
>ls-la (list long and hidden ones)
>--help (help)
>-h (help)
>man nmap (manual)

### FINDING STUFF ###

#### Searching with locate ####
```c
kali >locate aircrack-ng
/usr/bin/aircrack-ng
/usr/share/applications/kali-aircrack-ng.desktop
/usr/share/desktop-directories/05-1-01-aircrack-ng.directory
--snip--
/var/lib/dpkg/info/aircrack­ng.mg5sums
```
#### Finding Binaries with whereis ####
```C
kali >whereis aircrack-ng
aircarck-ng: /usr/bin/aircarck-ng /usr/share/man/man1/aircarck-ng.1.gz
```
#### Finding Binaries in the PATH Variable with which ####
```C
kali >whichaircrack-ng
/usr/bin/aircracK-ng
```
#### Performing More Powerful Searches with find ####
kali >find/➊ -typef➋ -nameapache2➌

First I state the directory in which to start the search, in this case /➊. Then I specify
which type of file to search for, in this case ffor an ordinary file ➋. Last, I give the name
of the file I’m searching for, in this case apache2➌.

```C
kali >find / -type f -name apache2
/usr/lib/apache2/mpm-itk/apache2
/usr/lib/apache2/mpm-event/apache2
/usr/lib/apache2/mpm-worker/apache2
/usr/lib/apache2/mpm-prefork/apache2
/etc/cron.daily/apache2

/etc/logrotate.d/apache2
/etc/init.d/apache2
/etc/default/apache2
```

```C
kali >find /etc -type f -name apache2
/etc/init.d/apache2
/etc/logrotate.d/apache2
/etc/cron.daily/apache2
```

```C
kali >find /etc -type f --name apache2.*
/etc/apache2/apache2.conf
```

#### Filtering with grep ####
```C
kali >ps aux | grep apache2
root 4851 0.2 0.7 37548 7668 ? Ss 10:14 0:00 /usr/sbin/apache2 ­k start
root 4906 0.0 0.4 37572 4228 ? S 10:14 0:00 /usr/sbin/apache2 ­k start
root 4910 0.0 0.4 37572 4228 ? Ss 10:14 0:00 /usr/sbin/apache2 ­k start
­­snip­­
```


### CREATING AND REMOVING FILES AND DIRECTORYS ###
#### Creating Files and directorys ####

cat > hackingskills (create and write if you do twice this you overwrite the file)
cat >> hackingskills (add more things to the file)
cat hackingskills(you read the file)

touch newfile (you can create with touch,and modify)
mkdir newdirectory (create directory)


#### Copy file , remove and delete ####

cp oldfile /root/newdirectory/newfile (you move it and change the name too)

mv newfile newfile2 (move the file with a new name)

rm newfile2 (delete a file)
rmdir directory (remove a directory)
rm -r directory  (removes the directory with the files inside)

## 2 TEXT MANIPULATION ##

>cat /etc/snort/snort.conf

>head /etc/snort/snort.conf

>head -20 /etc/snort/snort.conf

>tail /etc/snort/snort.conf

>tail-20/ etc/snort/snort.conf

Numbering the Lines

>nl /etc/snort/snort.conf

>cat /etc/snort/snort.conf | grepoutput

>nl /etc/snort.conf | grepoutput

 >tail -n+507 /etc/snort/snort.conf | head -n 6

>cat /etc/snort/snort.conf | grep mysql

>sed s /mysql/MySQL/g /etc/snort/snort.conf > snort2.conf

>catsnort2.conf | grepMySQL

more

less
## 3 ANALYZING AND MANAGIN NETWORKS ##

>ifconfig
>>iwconfig

>ifconfig eth0 192.168.181.115 (change ip)

>ifconfig eth0 192.168.181.115 netmask 255.255.0.0 broadcast (changing netmask and broadcast adress)

SPOOFING MAC 

kali >ifconfigeth0down
kali >ifconfigeth0hwether00:11:22:33:44:55
kali >ifconfigeth0up

NEW IP FROM THE DHCP SERVER (INVESTIGATE)

>dhclient eth0

>ifconfig

Examining DNS with dig

>dig hackers-arise.com ns (NAMESERVER)

>dig hackers-arise.com mx (MAIL NO SE QUE)

Changing Your DNS Server

>leafpad /etc/resolv.conf

Mapping Your Own IP Addresses


>leafpad/etc/hosts

## 4 ADDING AND REMOVING SOFTWARE ##

Searching for a Package


apt-cache search "example"

apt-remove snort (eliminates)
apt-get purge snort (purges dependencys)

TYPES OF REPOSITORYS 

leafpad /etc/apt/sources.list

main                Contains supported open source software
universe          Contains community­maintained open source software
multiverse       Contains software restricted by copyright or other legal issues
restricted        Contains proprietary device drivers
backports        Contains packages from later releases



## 5 CONTROLLING FILE AND DIRECTORY PERMISSIONS ##

>chown➊bob➋/tmp/bobsfile   (GIVING PERMISIONS TO AN INDIVIDUAL USER)

>chgrp➊security➋newIDS (permisions to groups
>
SETTING MORE SECURE DEFAULT PERMISSIONS WITH
MASKS)

>find / -user root -perm -4000

➊ ­rwsr­xr­x 1 root root 140944 Jul 5 2018 sudo

Note that at ➊, the first set of permissions—for the owner—has an sin place of the x.
This is how Linux represents that the SUIDbit is set. This means that anyone who runs
the sudo file has the privileges of the root user, which can be a security concern for the
sysadmin and a potential attack vector for the hacker.



## 6 PROCESS MANAGEMENT ##

ps
ps aux

>psaux | grep msfconsole

>top

>nice  -n -10 /bin/slowprocess

>nice -n 10 /bin/slowprocess

>renice 20 6996
(THERE ARE DIFFERENT TYPES OF KILL WHEN YOU DO A KILL MUST SPECIFY SOMETIMES THE NUMER, AND THERE IS ONE BY DEFAULT)
>kill -1 6996

>kill -9 6996

>killall -9 zombieprocess

PUT A DATE TO RUN A PROGRAM OR SCRIPT
```
kali >at 7:20am
at > /root/myscanningscript
```


## 7 MANAGING USER ENVIRONMENT VARIABLES ##

WATCH THE ENVIRONMENT VARIABLES
>env

VIEW ALL THE VARIABLES

>set | more

kali >set | grepHISTSIZE
HISTSIZE=1000

GIVING A NEW VALUE

>HISTSIZE=0

Making Variable Value Changes Permanent

kali > echo$HISTSIZE > ~/valueofHISTSIZE.txt
KALI> set> ~/valueofALLon01012017.txt

>export HISTSIZE

kali >HISTSIZE=1000
kali >export HISTSIZE

CHANGING YOUR SHELL PROMPT

kali >PS1="World'sBestHacker: #"
>export PS1

CHANGING YOUR PATH 
kali >echo $PATH

Adding to the PATH Variable

>PATH=$PATH:/root/newhackingtool

Adding to PATHcan be a useful technique for directories you use often, but be
careful not to add too many directories to your PATHvariable. Because the
system will have to search through each and every directory in PATHto find
commands, adding a lot of directories could slow down your terminal and
your hacking.
 
BE CAREFUL WITH THIS IF YOU DELETE THE PATH ACTUAL THE SYSTEM CANNOT USE THE VARIABLES AND COMANDS BY DEFAULT .. LIKE " SUDO " OR "CD"... IT CAN BE A DISASTER IF YOU DO IT BAD

THIS IS HOW YO DO NOT HAVE TO DO IT 

kali >PATH=/root/newhackingtool
kali >echo $PATH
/root/newhackingtool

CREATING A USER-DEFINED VARIABLE

>MYNEWVARIABLE="Hackingisthemostvaluableskillsetinthe21stCENTURY"

DELETING A USER VARIAIBLE

>unset MYNEWVARIABLE

## 8 BASH SCRITING ##

## 9 COMPRESS AND ARCHIVING ##

UNCOMPRESS
>tar -tvf HackersArise.tar 

>tar -xf HackersArise.tar

COMPRESS

>gzip HackersArise.* (uncompress is gunzip)

>bzip2 HackersArise.* (uncompress is bunzip)

compress HackersArise.* (uncompress ...)

CREATING BIT-BY-BIT OR PHYSICAL COPIES OF STORAGE
DEVICES

dd if=inputfileof=outputfile

kali >ddif=/dev/sdb of=/root/flashcopy



## 10 FILESYSTEM AND STORAGE DEVICE MANAGEMENT ##

WATCH DISPONIBLE DISKS
>fdisk -l 

List Block Devices and Information with lsblk

>lsblk

MOUNTING AND UNMOUNTING

>mount /dev/sdb1 /mnt

>umount /dev/sdb1

Getting Information on Mounted Disks

>df

Checking for Errors

kali >fsck

fsck -p (-p searchs automatically the errors)

(THE DISK MUST BE UNMOUNTED)

## 11 THE LOGGING SYSTEM ##
```
kali >locate rsyslog
/etc/rsyslog.conf
/etc/rsyslog.d
/etc/default/rsyslog
/etc/init.d/rsyslog
/etc/logcheck/ignore.d.server/rsyslog
/etc/logrotate.d/rsyslog
/etc/rc0.d/K04rsyslog

```

The rsyslog Configuration File

>leafpad /etc/rsyslog.conf

The rsyslog Logging Rules

AUTOMATICALLY CLEANING UP LOGS WITH LOGROTATE


kali >locate /var/log/auth.log.*

REMAINING STEALTHY

Removing Evidence
SHRED OVERWRITE MANY TIMES THE ARCHIVE AND ITS UNREACHABLE FOR A FORENSIC INVESTIGATOR
>shred--help

kali >shred -f -n10 /var/log/auth.log.*

Disabling Logging

>service rsyslog stop



## 12 USING AND ABUSING SERVICES ##

STARTING, STOPPING, AND RESTARTING SERVICES

service servicenamestart|stop|restart

## 14 UNDERSTANDING AND INSPECTING WIRELESS NETWORKS ##

>iwlist wlan0 scan

>nmcli dev wifi

nmcli dev wifi connect AP-SSIDpassword APpassword

>nmcli dev wifi connect Hackers-Arise password 12345678

DETECTING AND CONNECTING TO BLUETOOTH

>hciconfig

>hciconfig hci0 up
>hcitool scan
>hcitool inq
>>hcitool --help

Scanning for Services with sdptool

>sdptool browse 76:6E:46:63:72:66

l2ping MACaddress

>l2ping 76:6E:46:63:72:66 -c 4

## 15 MANAGING THE LINUX KERNEL AND LOADABLE KERNEL MODULES##

CHECKING THE KERNEL VERSION

>uname -a

>cat /proc/version

KERNEL TUNING WITH SYSCTL

kali >sysctl -a | less

kali > sysctl -w net.ipv4.ip_forward=1

MANAGING KERNEL MODULES
kali >lsmod

Finding More Information with modinfo

kali >modinfo bluetooth

Adding and Removing Modules with modprobe


kali >modprobe -a modulename
kali >modprobe -r moduletoberemoved

Inserting and Removing a Kernel Module
	
>modprobe -a HackersAriseNewVideo
	
kali >dmesg | grep video
	
kali >modprobe -r HackersAriseNewVideo
	
## 16 AUTOMATING TASKS WITH JOB SCHEDULING ##

SCHEDULING AN EVENT OR JOB TO RUN ON AN
AUTOMATIC BASIS

M H DOM MON DOW USER COMMAND
30 2 * * 1­5 root /root/myscanningscript

00 2 * * 0 backup /bin/systembackup.sh

kali >crontab-e


Adding Services to rc.d

kali >update-rc.d< nameofthescriptorservice>
<remove|defaults|disable|enable >

kali >leafpad /etc/crontab
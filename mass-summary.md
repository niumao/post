---
title: Mass Summary
date: 2020-02-02 11:40:40
categories:
tags:
---
linux下批量修改文件名后缀：
命令修改:rename 's/\.txt/\.rar/' ./*
脚本修改:
基本:
#!/bin/bash
allname=`ls *.txt`
for name in $allname
do
    mv $name ${name%.txt}.rar
done
加强:
#!/bin/bash
allname=`ls *$1`
for name in $allname
do
     mv $name ${name%$1}$2
done
例如：bash mename.sh .txt .rar  将所有的.txt文件改写为.rar文件
VIRTUALBOX下載站：http://download.virtualbox.org/virtualbox/
linux下添加非官方源(gpg處理)
debian7：aptitude install emdebian-archive-keyring kali-archive-keyring -->apt-key update
gpg: Signature made Thu May 3 11:36:31 2014 CET using RSA key ID 738D7BF6
gpg: Good signature from "Kali Linux Repository <devel@kali.org>"
$ grep kali-linux-1.0-i386.iso SHA1SUMS 796e32f51d1bf51e838499c326c71a1c952cc052 kali-linux-1.0-i386.iso
kali-linux-1.0-i386.iso: OK
Kali官方用一个分离的key(SHA1SUM.gpg)来给文件签名，kali官方key可以通过以下2个方式获得:
$ wget -q -O - http://archive.kali.org/archive-key.asc | gpg --import
# or
$ gpg --keyserver subkeys.pgp.net --recv-key 44C6513A8E4FB3D30875F758ED444FF07D8D0BF6
校验签名:
$ gpg --verify SHA1SUMS.gpg SHA1SUMS
手工进行比较:
$ sha1sum kali-linux-1.0-i386.iso 796e32f51d1bf51e838499c326c71a1c952cc052 kali-linux-1.0-i386.iso
 通过sha1sum -c:
grep kali-linux-1.0-i386.iso SHA1SUMS | sha1sum -c
win8 usb boot make step
Instructions (using the command line) 
contents:
    Mount the Windows 8 Developer Preview ISO image on your computer
    Format a USB flash drive
    Copy the Windows 8 files onto the USB flash drive
    Make the USB flash drive bootable
    Install Windows 8 from the bootable flash drive
detailed: 
 Step 1:
    Download the Windows Developer Preview ISO image (choose one of the three available)
    Using your favorite ISO image software, mount the ISO image on your computer
 Step 2: Format a USB flash drive 
    Insert a USB flash drive
    Start a Command Prompt as an Administrator and type diskpart.
    DISKPART> list disk                                 /* shows list of active disks */
    DISKPART> select disk #                         /* # is the number for your USB flash drive */
    DISKPART> clean                                     /* deletes any existing partitions on the USB flash drive */
    DISKPART> create partition primary     /* create a primary partition on the USB flash drive */
    DISKPART> select partition 1                 /* select the newly created partition */
    DISKPART> active                                    /* make the new partition active */
    DISKPART> format FS=NTFS                 /* format the USB drive with NTFS file system */
    DISKPART> assign                                   /* assign a volume and drive letter to the USB drive */
    DISKPART> exit                                        /* exit Disk Partition */
 Step 3: Make the USB flash drive bootable 
We just need to make the USB flash drive bootable before copying the files. To do this, you can use the Boot Sector Registration Tool (bootsect.exe) which is located in the boot folder of the Windows 8 ISO image.
    Start a Command Prompt as an Administrator and CHDIR into the boot folder of the Windows 8 ISO image, e.g. I:\boot where I:\ is the dhttps://www.blogger.com/blogger.g?blogID=7807535460479581138#editor/target=post;postID=6181327553558876824;onPublishedMenu=posts;onClosedMenu=posts;postNum=5;src=postnamerive where the ISO image is mounted
    Type bootsect /nt60 E:                              /* where E: is the drive assigned to the USB flash drive */
 Step 4: Copy the Windows 8 files onto the USB flash drive 
From a command line, use XCOPY to copy the Windows 8 files to the USB flash drive . In the example below, I:\ is the drive where the ISO image is mounted. F:\ is the USB flash drive.
XCOPY I:\*.* F:\ /E /F /H
 Step 5: Install Windows 8 from the bootable flash drive 
Finally, follow these instructions to install Windows 8 on your computer. 
Vmware EXSi
make usb installer：
fdisk /dev/sdb :d,n,t,c,a,w.
mkfs.vfat -F 32 -n USB /dev/sdb1
syslinux /dev/sdb1
cat /<varname>path_to_syslinux-3.86_directory</varname>/syslinux-3.86/usr/share/syslinux/mbr.bin > /dev/sdb
mount /dev/sdb1 /usbdisk
mount -o loop VMware-VMvisor-Installer-5.x.x-XXXXXX.x86_64.iso /esxi_cdrom
cp -r /esxi_cdrom/* /usbdisk
mv /usbdisk/isolinux.cfg /usbdisk/syslinux.cfg
在 /usbdisk/syslinux.cfg 文件中，将 APPEND -c boot.cfg 一行更改为 APPEND -c boot.cfg -p 1
cp /usr/lib/syslinux/menu.c32 /mnt/usb/
umount /usbdisk
umount /esxi_cdrom
now finished
my nexus5 refactory
note:for android l(lolipop),sometime gona wrong ,so tap this
fastboot flash bootloader bootloader-hammerhead-hhz12d.img
fastboot reboot-bootloader(ping -n 5 127.0.0.1 >nul)
fastboot flash radio radio-hammerhead-m8974a-2.0.50.2.22.img
fastboot reboot-bootloader(ping -n 5 127.0.0.1 >nul)
fastboot flash recovery recovery.img
fastboot flash boot boot.img
fastboot flash system system.img
fastboot flash cache cache.img -->双清
fastboot flash userdata userdata.img
fastboot reboot

MY SETTING(DEBIAN 7)


iptables:
iptables -F
iptables -X
iptables -P INPUT ACCEPT
iptables -P OUTPUT ACCEPT
iptables -P FORWARD DROP
iptables -I INPUT -j DROP
iptables -I INPUT -p tcp --dport 80 -j ACCEPT
iptables -I INPUT -p tcp --dport 53 -j ACCEPT
iptables -I INPUT -p all -m state --state RELATED,ESTABLISHED -j ACCEPT
iptables -I INPUT -p icmp --icmp-type 8 -j REJECT
iptables -I INPUT -p icmp --icmp-type 8 -s 192.168.0/24 -j ACCEPT
.bashrc:PS1='${debian_chroot:+($debian_chroot)}\[\e[00;31m\][\u@\h:\w]\[\e[00;32m\]\$ '
/root/.bashrc:PS1='${debian_chroot:+($debian_chroot)}\[\e[00;36m\][\d,\A]\[\e[07;31m\]\u@\h:\w\[\e[00;32m\]\$ '

JDK SETTING(DEBIAN 7)


1.wget http://download.oracle.com/otn-pub/java/jdk/7u79-b15/jdk-7u79-linux-x64.tar.gz?AuthParam=1430107031_6fdee52d13a3bf92d6932cd2fde9497d
tar zxvf ./jdk-7-linux-i586.tar.gz -C /usr/lib/jvm
cd /usr/lib/jvm
mv jdk1.7.0_05/ jdk7
2.gedit ~/.bashrc
export JAVA_HOME=/usr/lib/jvm/jdk7
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
3.source ~/.bashrc
update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk7/bin/java 300
update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk7/bin/javac 300
4.sudo update-alternatives --config java
5.java -version
Tencent QZone Cookie Info
uin; skey; p_uin; p_skey

Userfual top
top - original tool
htop - adds support to multicore/cpu
iotop - input/output monitoring
iftop - network monitoring
atop - merges previous elements into a single overview
slabtop – displays a listing of the top caches

Some VPS Services
my.vultr.com
www.linode.com
www.digitalocean.com
www.hostus.us $4.95/month 1GB (1000G) 2core
www.budgetvm.com
www.virtwire.com(budgetvz.com) $12/annually 1Gbit (500GB) SSD
www.pzea.com(cheapvz.com) $12/annually 256M 100Mbit(500G)
www.kvmla.com(cheapvz.com) ￥99/年 256M (500G)
www.ramnode.com $15/年 128M (500G)
www.elastichosts.com.hk
www.bluehost.com
www.yourlasthost.com $14.99/年 512M (15G)
www.alpharacks.com $10/年 256M (25G)
www.ethernetservers.com $15/年 256M (10G)
www.heroku.com free 512M (HOSTS)
Some Freelance Jobs
https://www.freelancer.com/
https://www.upwork.com/
https://www.elance.com/
https://www.topcoder.com/
https://www.hackerearth.com/

Some Beginner Programming Practice
http://blog.programmersmotivation.com/2014/07/09/list-projects/

Nginx 405 Gateway Timeout
/etc/php.ini
max_execution_time = 300
vim /etc/php5/fpm/pool.d/www.conf
request_terminate_timeout = 300
vim /etc/nginx/nginx.conf
http { #... fastcgi_read_timeout 300; #... }
service php5-fpm reload service nginx reload
if not append:
vim /etc/nginx.conf
proxy_connect_timeout 600; proxy_send_timeout 600; proxy_read_timeout 600; send_timeout 600;

Add PPA In Debian
wget http://blog.anantshri.info/content/uploads/2010/09/add-apt-repository.sh.txt
cp add-apt-repository.sh.txt /usr/sbin/apt-add-repository
chmod o+x /usr/sbin/apt-add-repository
chown root:root /usr/sbin/apt-add-repository

Install Android-studio In Debian
apt-add-repository ppa:paolorotolo/android-studio
apt-get update
apt-get install android-studio

Install Dell Win10
u盘介质 http://zh.community.dell.com/support_forums/software/w/wiki/1112
windows10:https://www.microsoft.com/zh-cn/software-download/windows10ISO

Install Debian In iwlwifi 3165
sudo apt-get update
sudo apt-get install linux-headers-generic build-essential 
wget https://www.kernel.org/pub/linux/kernel/projects/backports/2015/11/20/backports-20151120.tar.gz 
tar -zxvf backports-20151120.tar.gz 
cd backports-20151120 
make defconfig-iwlwifi 
make 
sudo make install
cd /lib/firmware sudo cp iwlwifi-7265D-13.ucode iwlwifi-3165-9.ucode sudo cp iwlwifi-7265-13.ucode iwlwifi-3165-13.ucode
cd /lib/firmware sudo cp iwlwifi-7265D-13.ucode iwlwifi-3165-9.ucode sudo cp iwlwifi-7265-13.ucode iwlwifi-3165-13.ucode
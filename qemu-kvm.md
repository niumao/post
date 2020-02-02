---
title: QEMU-KVM
date: 2018-09-14 07:16:46
tags:
---
vCpu:
1.User Mode   2.Kernel Mode   3.Guest Mode

1.check:
hard:
grep -E '(vmx|svm)' /proc/cpuinfo)
os:
lsmod | grep kvm

2.create
dd if=/dev/zero of=win7.img bs=1M count=8192
qemu-img create win7.img 35G
qemu-img create -f qcow2 win7.img 35G
qemu-img resize win7.img +2G
qemu-img info win7.img

3.install guest
qemu-system-x86_64 -hda win7.img -cdrom Ghost_XP.iso  -boot d -m 2048 -enable-kvm -net nic -net user,smb=/home/share

4.boot
qemu-system-x86_64 win7.img -m 2048 -enable-kvm
qemu-system-x86_64 -enable-kvm 
-drive file=win7.img,index=0,media=disk,format=raw,cache=writeback
-m 2048 
-smp 4 
-net nic 
-net user,smb=/home/share 
-cpu SandyBridge
-daemonize
-vnc :0

5.networking
macaddress=
(1).user mode
-net nic -net user

(2).bridge

package:bridge-utils tunctl
modeprobe tun
lsmod | grep tun
ll /dev/net/tun

brctl addbr br0
brctl addif br0 eth0
brctl stp br0 on
ifconfig eth0 br0
dhclient br0
route

ifup script:
ip link set tap0 up
sleep 1
brctl addif br0 tap0
ifdown script:
tunctl -d tap0
brctl delif br0 tap0
ip link set tap0 down

ls /sys/devices/virtual/net/
brctl show

-net nic -net tap,ifname=tap0,script=/etc/qemu-ipup,downscript=no
(3).nat

6.virtio
(1) dy mem
-balloon virtio
info balloon
balloon 512
balloon num
(2) net
-net nic,model=virtio,macaddr= -net tap,vnet_hdr=on,vhost=on
(3) block
file=win.img,if=virtio

7.Device Assignment(intel:VT-d,AMD:AMD-Vi)
BIOS:Enabled
check: dmesg | grep DMAR -i
-device pci-assign,kvm-pci-assign.host=pci-host-devaddr
-device pci-assign,host=09:00.0,id=mydev0,addr=0x6
(1).USB
(2).VGA

8.QEMU/KVM Manager Tools
libvirt
virt-manager etc


shell:
1.cpu count:
cat /proc/cpuinfo | grep "processor" | wc -l
cat /proc/cpuinfo | grep -qi "core id"
cat /proc/cpuinfo | grep "physical id" | sort | uniq | wc -l
2.cpu type list
qemu-system-x86_64 -cpu ?
qemu-system-x86_64 -net nic,model=?


console:
switch Ctrl + Alt + 1/2
info kvm
info cpus
info network

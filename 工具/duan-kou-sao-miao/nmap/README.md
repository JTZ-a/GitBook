---
description: 用于端口扫描，并支持一些漏洞扫描
---

# nmap

> * 主机探测
> * 端口扫描
> * 版本检测
> * 支持探测脚本的编写

## 基本扫描

```shell
# 基本快速扫描， 默认发送 一个 ARP 的 ping 数据包，探测目标主机在 1-10000 范围内开放的所有端口
nmap IP

# 快速扫描多个目标
nmap IP1 IP2....

# 详细描述输出扫描, 简单扫描，并对返回的结果详细描述输出
nmap [ -v | -vv ] IP 

# 指定端口和范围扫描
nmap -p [port, port...] IP

# 扫描除某一个 IP 外的所有子网主机
nmap IP/24 -exclude IP

# 显示扫描的所有主机的列表
nmap -sL IP/24

# Ping 扫描，只检测主机是否存在于网络中
nmap -sP IP

# SYN 半开放扫描, 不会在目标主机上产生记录
nmap -sS IP

# TCP 扫描
nmap -sT IP
# UDP 扫描
nmap -sU IP

# FIN  标志的数据报扫描, 不会在目标主机创建日志
nmap -sF IP

# Version 版本检测扫描
nmap -sV IP

# 操作系统探测
nmap -O IP

# 猜测匹配操作系统
nmap -O --osscan-guess IP

# NO Ping 扫描
nmap -O -PN IP

# 设置时间模板
nmap -sS -T<0-5> IP

# 网段扫描格式
nmap -sP IP/CIDR

# 从文件中读取需要扫描的 IP 列表
nmap -iL file

# 路由跟踪扫描
nmap -traceroute IP

# 包含 OS 识别、版本探测、脚本扫描、traceroute 综合扫描
nmap -A IP
```

## 脚本使用

[官方脚本文档](https://nmap.org/nsedoc/)

```shell
nmap --script 类别
# 类别
- auth: 负责处理鉴权证书（绕开鉴权）的脚本  
- broadcast: 在局域网内探查更多服务开启状况，如dhcp/dns/sqlserver等服务  
- brute: 提供暴力破解方式，针对常见的应用如http/snmp等  
- default: 使用-sC或-A选项扫描时候默认的脚本，提供基本脚本扫描能力  
- discovery: 对网络进行更多的信息，如SMB枚举、SNMP查询等  
- dos: 用于进行拒绝服务攻击  
- exploit: 利用已知的漏洞入侵系统  
- external: 利用第三方的数据库或资源，例如进行whois解析  
- fuzzer: 模糊测试的脚本，发送异常的包到目标机，探测出潜在漏洞
- intrusive: 入侵性的脚本，此类脚本可能引发对方的IDS/IPS的记录或屏蔽
- malware: 探测目标机是否感染了病毒、开启了后门等信息  
- safe: 此类与intrusive相反，属于安全性脚本，不会影响目标
- version: 负责增强服务与版本扫描（Version Detection）功能的脚本  
- vuln: 负责检查目标机是否有常见的漏洞（Vulnerability），如是否有MS08_067
```

常见的一些脚本使用

```shell
# 扫描服务器常见漏洞
nmap --script vuln IP
# 检查 FTP 是否开启匿名登陆
nmap --script ftp-anon IP
# 对 MYSQL 进行暴力破解
nmap --script mysql-brute IP
# 对 MSSQL 进行暴力破解
nmap -p 1433 --script ms-sql-brute --script-args userdb=customuser.txt,passdb=custompass.txt IP
# 对 Oracle 数据库进行暴力破解
nmap --script oracle-brute -p 1521 --script-args oracle-brute.sid=ORCL IP

# 对 抛光SQL 暴力破解
nmap -p 5432 --script pgsql-brute IP

# 对 SSH 暴力破解
nmap -p 22 --script ssh-brute --script-args userdb=users.lst,passdb=pass.lst --script-args ssh-brute.timeout=4s IP

# 使用 DNS 进行子域名暴力破解
nmap --script dns-brute www.baidu.com

# 检查VMWare ESX，ESXi和服务器（CVE-2009-3733）中的路径遍历漏洞
nmap --script http-vmware-path-vuln -p80,443,8222,8333 IP

# 查询VMware服务器（vCenter，ESX，ESXi）SOAP API以提取版本信息。
nmap --script vmware-version -p443 10.0.1.4
```

## 参数详解

Nmap 支持主机名 IP 网段表示方式

```shell
blah.highon.coffee, namp.org/24, 192.168.0.1;10.0.0-25.1-254

-iL filename                    从文件中读取待检测的目标,文件中的表示方法支持机名,ip,网段
-iR hostnum                     随机选取,进行扫描.如果-iR指定为0,则是无休止的扫描
--exclude host1[, host2]        从扫描任务中需要排除的主机           
--exculdefile exclude_file      排除文件中的IP,格式和-iL指定扫描文件的格式相同
```

### 1. 主机发现

```shell
-sL                     仅仅是显示,扫描的IP数目,不会进行任何扫描
-sP                     ping扫描,即主机发现,显对扫描做出响应的主机
-P0                     Nmap 默认只对确定正在运行的主机进行高强度扫描，使用该命令可以对每一个目标进行扫描
-Pn                     不检测主机是否存活
-PS/PA/PU               TCP SYN Ping/TCP ACK Ping/UDP Ping
-PE/PP/PM               使用ICMP echo, timestamp and netmask 请求包发现主机
-PR                     ARP Ping 扫描，
-n                      不对IP进行域名反向解析
-R                      对目标 IP 地址进行反向域名解析，一般只有当发现机器正在运行时才进行这步操作
--system-dns            使用系统域名解析器  
```

### 2. 端口扫描

| 状态               | 描述                                                           |
| ---------------- | ------------------------------------------------------------ |
| open             | 这表明与扫描端口的连接已建立。这些连接可以是TCP 连接、UDP 数据报以及SCTP 关联。               |
| closed           | 当端口显示为关闭时，TCP 协议表明我们收到的数据包包含一个RST标志。这种扫描方法也可以用来确定我们的目标是否还活着。 |
| filtered         | Nmap 无法正确识别扫描的端口是打开还是关闭，因为要么没有从目标返回该端口的响应，要么我们从目标获得错误代码。     |
| unfiltered       | 端口的这种状态只发生在TCP-ACK扫描期间，表示该端口是可访问的，但无法确定它是打开还是关闭。             |
| open\|filtered   | 如果我们没有得到特定端口的响应，Nmap则将其设置为该状态。这表明防火墙或数据包过滤器可以保护端口。           |
| closed\|filtered | 此状态仅出现在IP ID 空闲扫描中，表示无法确定扫描的端口是否已关闭或被防火墙过滤。                  |

```shell
-sS/sT/sA/sW/sM                 TCP SYN/TCP connect()/ACK/TCP窗口扫描/TCP Maimon扫描
-sU                             UDP扫描
-sN/sF/sX                       TCP Null，FIN，and Xmas扫描
--scanflags                     自定义TCP包中的flags
-sI zombie host[:probeport]     Idlescan
-sY/sZ                          SCTP INIT/COOKIE-ECHO 扫描
-sO                             使用IP 协议扫描确定目标机支持的协议类型
-b <ftp relay host>             使用FTP 弹跳扫描
```

### 3. 端口说明和扫描顺序

```shell
-p                      特定的端口 -p80,443 或者 -p1-65535
-p U:PORT               扫描udp的某个端口, -p U:53
-F                      快速扫描模式,比默认的扫描端口还少
-r                      不随机扫描端口,默认是随机扫描的
--top-ports "number"    扫描开放概率最高的number个端口,出现的概率需要参考nmap-services文件,ubuntu中该文件位于/usr/share/nmap.nmap默认扫前1000个
--port-ratio "ratio"    扫描指定频率以上的端口
```

### 4. 服务版本识别

```shell
-sV                             开放版本探测,可以直接使用-A同时打开操作系统探测和版本探测
--allports                      不为版本探测排除任何端口
--version-intensity [0~9]     设置版本扫描强度,强度水平说明了应该使用哪些探测报文。数值越高，服务越有可能被正确识别。默认是7
--version-light                 打开轻量级模式,为--version-intensity 2的别名
--version-all                   尝试所有探测,为--version-intensity 9的别名
--version-trace                 显示出详细的版本侦测过程信息
```

### 5. 脚本扫描

```shell
-sC                             根据端口识别的服务,调用默认脚本
--script=”Lua scripts”          调用的脚本名
--script-args=n1=v1,[n2=v2]     调用的脚本传递的参数
--script-args-file=filename     使用文本传递参数
--script-trace                  显示所有发送和接收到的数据
--script-updatedb               更新脚本的数据库
--script-help=”Lua script”      显示指定脚本的帮助
```

### 6. OS 识别

```shell
-O              启用操作系统检测,-A来同时启用操作系统检测和版本检测
--osscan-limit  针对指定的目标进行操作系统检测(至少需确知该主机分别有一个open和closed的端口)
--osscan-guess  推测操作系统检测结果,当Nmap无法确定所检测的操作系统时，会尽可能地提供最相近的匹配，Nmap默认进行这种匹配
```

### 7. 时间和性能

```shell
--min-hostgroup <size>; --max-hostgroup <size>        调整并行扫描组的大小
--min-parallelism <numprobes>; --max-parallelism <numprobes>        调整探测报文的并行度
--min-rtt-timeout <milliseconds>， --max-rtt-timeout <milliseconds>， --initial-rtt-timeout <milliseconds>        调整探测报文超时
--host-timeout <milliseconds>        放弃低速目标主机
--scan-delay <milliseconds>; --max-scan-delay <milliseconds>        调整探测报文的时间间隔
-T <Paranoid|Sneaky|Polite|Normal|Aggressive|Insane>        设置时间模板， Nmap提供了一些简单的 方法，使用6个时间模板，使用时采用-T选项及数字(0 - 5) 或名称。模板名称有paranoid (0)、sneaky (1)、polite (2)、normal(3)、 aggressive (4)和insane (5)。前两种模式用于IDS躲避，Polite模式降低了扫描 速度以使用更少的带宽和目标主机资源。默认模式为Normal，因此-T3 实际上是未做任何优化。Aggressive模式假设用户具有合适及可靠的网络从而加速 扫描。Insane模式假设用户具有特别快的网络或者愿意为获得速度而牺牲准确性。
```

### 8. 防火墙/IDS躲避和哄骗

```shell
-f; --mtu value                 指定使用分片、指定数据包的MTU.
-D decoy1,decoy2,ME             使用诱饵隐蔽扫描
-S IP-ADDRESS                   源地址欺骗
-e interface                    使用指定的接口
-g/ --source-port PROTNUM       使用指定源端口  
--proxies url1,[url2],...       使用HTTP或者SOCKS4的代理
--data-length NUM               填充随机数据让数据包长度达到NUM
--ip-options OPTIONS            使用指定的IP选项来发送数据包
--ttl VALUE                     设置IP time-to-live域
--spoof-mac ADDR/PREFIX/VEBDOR  MAC地址伪装
--badsum                        使用错误的checksum来发送数据包
```

### 9. Nmap 输出

```shell
# XML  转换为 HTML 展示
xsltproc target.xml -o target.html

-oN                     将标准输出直接写入指定的文件
-oX                     输出xml文件
-oS                     将所有的输出都改为大写
-oG                     输出便于通过bash或者perl处理的格式,非xml
-oA BASENAME            可将扫描结果以标准格式、XML格式和Grep格式一次性输出
-v                      提高输出信息的详细度
-d level                设置debug级别,最高是9
--reason                显示端口处于带确认状态的原因
--open                  只输出端口状态为open的端口
--packet-trace          显示所有发送或者接收到的数据包
--iflist                显示路由信息和接口,便于调试
--log-errors            把日志等级为errors/warings的日志输出
--append-output         追加到指定的文件
--resume FILENAME       恢复已停止的扫描
--stylesheet PATH/URL   设置XSL样式表，转换XML输出
--webxml                从namp.org得到XML的样式
--no-sytlesheet         忽略XML声明的XSL样式表
```

### 10. 其他 nmap 选项

```shell
-6                      开启IPv6
-A                      OS识别,版本探测,脚本扫描和traceroute
--datedir DIRNAME       说明用户Nmap数据文件位置
--send-eth / --send-ip  使用原以太网帧发送/在原IP层发送
--privileged            假定用户具有全部权限
--unprovoleged          假定用户不具有全部权限,创建原始套接字需要root权限
-V                      打印版本信息
-h                      输出帮助
```

## 参考文章

* [nmap 超详细使用教程](https://blog.csdn.net/Kris\_\_zhang/article/details/106841466?ops\_request\_misc=\&request\_id=\&biz\_id=102\&utm\_term=nmap%20%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B\&utm\_medium=distribute.pc\_search\_result.none-task-blog-2\~all\~sobaiduweb\~default-2-106841466.142^v63^pc\_rank\_34\_queryrelevant25,201^v3^control\_1,213^v1^t3\_control2\&spm=1018.2226.3001.4187)
* [NMAP 官网](https://nmap.org/)

# 反序列化

## 介绍

反序列化漏洞就是将恶意代码生成的恶意数据，交付给应用程序处理，通常可以利用这一点实现 DDOS 、RCE、等等各种攻击形式

## 危害

* 低可利用性： 在没有具体代码的情况下分析该漏洞很困难，且没有可靠的工具
* 漏洞只有在攻击者提交的恶意负载被允许处理时才具有威胁性

## 攻击

### 1. Cookie

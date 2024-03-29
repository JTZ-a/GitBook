# DHCP

## 介绍

DHCP 又称动态主机设置协议，是一个用于 IP 网络的网络协议，使用 `UDP` 协议工作

主要功能：

* 用于内部网络或内部服务供应商自动分配 IP 地址给用户
* 用于内部网管理员对所有电脑做中央管理

> 当设备连接到网络，并且`尚未分配 IP 地址时`，会发出 DHCP DIscover 请求查看网络上是否有 DHCP 服务器，后 DHCP 服务器回复一个设备可以使用的 IP 地址（DHCP Offer）。然后设备发送一个回复确认它需要提供的 IP 地址（DHCP Request），最后，DHCP 服务器发送一个确认这已经完成的回复，并且设备可以开始使用 IP 地址（DHCP ACK）。

<figure><img src="../../../.gitbook/assets/DHCP.png" alt=""><figcaption></figcaption></figure>

## 原理

动态主机设置协议（DHCP）是一种使网络管理员能够集中管理和自动分配IP网络地址的通信协议。在 IP 网络中，每个连接Internet的设备都需要分配唯一的IP地址。DHCP使网络管理员能从中心结点监控和分配IP地址。当某台计算机移到网络中的其它位置时，能自动收到新的IP地址。

DHCP 使用了租约的概念，或称为计算机IP地址的有效期。租用时间是不定的，主要取决于用户在某地连接Internet需要多久，这对于教育行业和其它用户频繁改变的环境是很实用的。透过较短的租期，DHCP能够在一个计算机比可用IP地址多的环境中动态地重新配置网络。DHCP支持为计算机分配静态地址，如需要永久性IP地址的Web服务器。

DHCP 向客户端提供的信息：

* IP 地址
* 子网掩码
* 默认网关还可以提供其他信息，例如域名服务 (DNS) 服务器地址和 Windows Internet 名称服务 (WINS) 服务器地址。系统管理员使用解析给客户端的选项配置 DHCP 服务器

## 参考文章

* [参考链接](https://zh.wikipedia.org/wiki/%E5%8A%A8%E6%80%81%E4%B8%BB%E6%9C%BA%E8%AE%BE%E7%BD%AE%E5%8D%8F%E8%AE%AE)

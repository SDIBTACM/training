# 实验室网络

### 综述
打比赛需要断网

#### 物理网络设备

板载网卡 + 3 网卡, 全为千兆网卡.

目前在系统中识别为(从上到下) enp7s0, enp3s0, enp4s0, enp6s0

enp7s0 作为 ipv4 流量出口
enp4s0 原本计划作为 ipv6 流量出口, 但目前实验室没有提供 Ipv6, 办法直接接入校园网获取ipv6

其中 enp4s0, enp6s0 被绑定为 bond0 虚拟网卡

##### bond0 设备
bond0 设备使用 balance-alb 协议绑定在一起, 可以负责提供流量均衡, 故障转移的功能.

关于使用的 bond 协议, 如果两个网卡同时在线, 需要接入在相同网内(交换机串连), 且能相互探测到, 否则会引起广播风暴致使网络瘫痪.
如果无法保证在同一网内, 请不要两个口同时上电.


#### 基础网络信息
实验室内地址池为： 172.16.0.0/16,
其中 
* 172.16.0.0/24 保留
* 172.16.1.0/24 被保留为其他服务器使用
* 172.16.2.0/24 被保留为其他网络设备使用(交换机/路由器)
* 172.16.3.0 - 172.16.7.255 保留 
* 172.16.8.0 - 172.16.238.255 用作 DHCP 池
* 172.16.239.0 - 172.16.255.254 保留

### 网络控制

网路控制主要通过 iptables 配合 ipset 实现
ipset 中 ip 地址通过 dnsmasq 增加, 超时删除
网络中所有 DNS 请求会被重定向到网关上

在 /root/network/下提供了脚本方便控制.

### 运行服务

#### dnsmasq
主要用于提供 DNS, DHCP; 同时提供 TFTP 引导服务.

#### frpc
用于提供内网穿透, 目前 frps 服务器在 sdibt.jump.boxjan.li (阿里云 青岛)

#### PXE 
实验室内网络装机提供. 
暂时没有脚本自动更新支持的系统.

新增系统可以参考 https://linuxhint.com/pxe_boot_ubuntu_server/ , 后续可以编写脚本实现自动新增.

#### Ubuntu archive mirror
上游为 rsync.archive.ubuntu.com

相关文件位于 /mnt/wd-blue-3T/ubuntu-archive

仓库位于 /mnt/wd-blue-3T/ubuntu-archive/repo/ubuntu 下

每 2 小时更新一次

实验室 dnsmasq 中设置了将 cn.archive.ubuntu.com 解析为服务器地址, 以加快速度.

#### Ubuntu release mirror
上游为 rsync.releases.ubuntu.com

相关文件位于 /mnt/wd-blue-3T/ubuntu-release

仓库位于 /mnt/wd-blue-3T/ubuntu-release/repo/ubuntu-release 下

每12小时更新一次.

#### Jump the great wall
通过 iptables + ipset + v2ray 实现. 详细网上搜吧. =.=

#### samba
```
user: share
pwd: acm
```
反正就内网使用(学校内网也可, 不过你咋知道ip呢)

#### nginx 
看看配置文件就知道了.

### 后续
对于所有的内容有不足之处可以修改, 但涉及到网络部分, 请在操作前知道自己再做什么, 再进行操作.
关于系统大版本升级 请只使用 do-release-upgrade 升级.
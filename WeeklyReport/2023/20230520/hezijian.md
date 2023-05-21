# 20230520

五一有点无聊, 写点东西好了.
最后果然没写完, 拖了~~2~~3周, 

想了想, 写点关于工作初期的第二个东西 -- 时间同步, 也是持续了很长时间的工作.
我们的承诺是 全球范围内 99.9% 分位误差不超过 50ms; 实际根据观测线上远优于这个目标.

[几点了?](https://time.is)

很多东西也是来源于其他同事的帮助, 在此表示感谢.

## 什么是时间？
时间的本质是节拍, [SI](https://zh.wikipedia.org/zh-cn/%E5%9B%BD%E9%99%85%E5%8D%95%E4%BD%8D%E5%88%B6) 中对 秒 的定义是
>铯-133原子在基态下的两个超精细能级之间跃迁所对应的辐射的9192631770个周期的时间。

时间的起点是人为决定的, OS 在启动时拿了 CMOS 时间作为起点;

CMOS 时钟对应的晶振与系统时间对应的晶振不是同一个晶振, 且 CMOS 时钟的精度远低于系统时间精度 (32.768kHz vs 48MHz);

对于 Intel Skylake / Cascade lake (Refresh)平台的物理机, 节拍来源于处理器本身 BCLK 频率, 而 BCLK 频率来源于 PCH 的 BCLK 信号;

PCH 的 BCLK 信号也不是凭空来的, 其来源于一个 48MHz 的晶振, 这个晶振会将频率输入给 PCH (XTAL_IN/XTAL_OUT), PCH 将输出 16 路 100MHz 的 CLKOUT_SRC 信号, 提供给所有的 CPU 和 PCIe 设备；详情见：[c620 芯片组手册](https://www.intel.com/content/dam/www/public/us/en/documents/datasheets/c620-series-chipset-datasheet.pdf) 8.6章

也就是说, 该 48MHz 晶振的精度会影响机器整体的时间精度, 其标定的一般精度为 10 - 20 ppm (服务器厂商不同选料可能不同, 运气不好的也可能会遇到 50 ppm 的), 成本约 1块/颗；超过 50ppm 的物理机可以认为硬件有问题.

拿来做时间同步服务器的源的钟 (OCXO) 会比这个晶振好三至四个数量级的精度, 但价格也需要约 100 倍的价格.

晶振本身的频率会受温度影响, 并不是上电之后就稳定为一个数值的.

因为 CPU 节能特性等, 默认的时间来源(TSC)在 CPU 内的硬件实现是 PMU, PMU 本身独立于各个运行中的 核心/缓存 或 Ring/PCIe/UPI 等, 不会因为各种节能(包括核心降频, Ring频率降频等)影响其频率.

在双路/多路系统中, Intel 在 /proc/cpuinfo 的 cpuflag 中的 constant_tsc 显示了其可以保证从任意一颗CPU上执行的 RDTSC 的结果均是等价的, 也就是说每个插槽上cpu的时间节拍是一致的, 不需要担心从不同核心上执行时间节拍会不一致.

在上述情况下, Linux 是怎么如何获取及设置当前时间的：https://juejin.cn/post/6844903789908983821
* 该链接中内核版本较老, 整体思路对的, 结论不对, 目前 6.1 版本内核通过系统调用 (clock_gettime 等系统调用) 的实现最终会转为使用 RDTSC 命令 (详见内核相关code) 
* 内核在实现该系统调用时**尽量**不涉及内核态及用户态的切换以提供最大的精度([vDSO](https://en.wikipedia.org/wiki/VDSO), 但根据实际的的入参, 实际函数调用会有差别, 最差情况下还是会陷入内核态)

## 协议与软件
### 协议
主流协议为 [NTP](https://en.wikipedia.org/wiki/Network_Time_Protocol), 可以达到数毫秒级精度甚至到亚毫秒的精度.

在数据中心内部对于时间精度要求高的情况下, 会采用 [PTP](https://en.wikipedia.org/wiki/Precision_Time_Protocol), 可以达到亚微秒级的精度, 但需要交换机与网卡的支持, 以保障 PTP 包被优先处理.

这里不详细介绍各协议细节于误差来源.

### 软件
(简介都来自 ChatGPT 3.5)
#### [chrony](https://chrony.tuxfamily.org/)
Chrony 是一款精准的网络时间同步工具，可以在 Linux 系统中被广泛地使用。通过 Chrony，用户可以将本地系统时间与一个或多个参考时间源进行同步，实现更加准确的时间同步。

相比起传统的 NTP（网络时间协议），Chrony 提供了更多的优化和功能，例如：
* 支持更高的精度：Chrony 的算法可以在网络抖动较大的情况下保证更高的时间同步精度。
* 快速同步：Chrony 支持快速同步功能，可以在网络断开再恢复时更快地同步时间。
* 安全性：Chrony 支持使用数字证书进行时间同步验证，防止恶意时钟攻击。
* 可靠性：Chrony 可以与多个同步源进行协作，从而保证时间同步的可靠性和准确性。

#### [openNTPD](https://www.openntpd.org/)

OpenNTPD是一种时间协议（NTP）守护进程，它用于与网络中的其他计算机同步系统时钟。OpenNTPD的主要目的是重新设计NTP协议，使其更轻巧和快速。它还具有网络安全增强功能，例如避免拒绝服务攻击和缓解时钟反射攻击等。

OpenNTPD使用一种称为“平滑时钟漂移”算法的技术来调整系统时钟，并通过使用UDP协议与其他计算机同步时间。它还使用DNS来查询其他NTP服务器，以获取当前时间信息。

OpenNTPD是免费的开源软件，可以在各种操作系统上使用，包括Linux、BSD和Solaris等。它还支持IPv6和IPSec协议，提供更强大的安全性保护。OpenNTPD非常易于配置和管理，可以通过简单的命令行工具或配置文件进行设置。

#### [ntpd](http://www.ntp.org/)
ntpd是一种网络时间协议（NTP）守护程序，可在许多Unix和Linux操作系统中使用。它旨在为主机上运行的NTP客户端提供时间同步服务。ntpd使用一种算法来将主机时钟的偏差计算出来，并将该偏差纠正为国际原子钟的时间。

ntpd可以在本地网络上使用，也可以在Internet上与其他ntpd服务器进行通信。当与其他ntpd服务器通信时，ntpd可以进行时间同步并根据最好的可用源进行自动选择。这有助于确保最高精度和可靠性。

ntpd还可以进行时钟源的鉴定，检测屏蔽和验证收到的时间数据。它还具有跟踪时钟源偏移的大量服务和功能。ntpd是许多Linux和Unix服务器和桌面系统中的标准时间同步工具。

#### ntpdate
ntpdate是一个Linux/Unix操作系统中常用的NTP客户端程序，它可以通过网络协议从NTP服务器上获取当前的时间，并将该时间同步到系统的本地时间。

当系统上的时钟失去同步或出现时间偏移时，可以通过使用ntpdate程序从远程的NTP服务器上同步时间，以保证系统上的时钟精度和准确度。

ntpdate命令通常作为一个单独的命令使用

ntpdate程序通过向NTP服务器发送包含时间戳信息的NTP协议数据包，并使用时间戳计算出系统本地时间与NTP服务器时间之间的差值，然后通过调整系统本地时间来使之与NTP服务器时间同步。

需要注意的是，ntpdate程序已被废弃，建议使用ntpd程序进行NTP时间同步。

#### [systemd-timesyncd](https://www.freedesktop.org/software/systemd/man/systemd-timesyncd.service.html)
timesyncd 是 systemd 中的一个系统时间同步服务，用于进行本地系统时间与网络时间服务器同步。与传统的 ntpd（网络时钟协议守护进程）相比，timesyncd 更加轻量级，能够在系统启动时快速进行时间同步，并能够自适应地调整时间同步间隔。

timesyncd 默认的时间服务器为采用 Google 的时间服务器（time.google.com），但也可以通过修改配置文件来指定其他的时间服务器。此外，timesyncd 还支持同时使用多个时间服务器，以提高时间同步成功的几率和速度。

timesyncd 不需要任何授权或证书，且运行时所占用的内存较小，因此更适合用于嵌入式设备和轻量级服务器。

## 具体的工作

### 工具的决定
看来看去觉得还是 chrony 好, 那就决定了使用 chrony !

### 时间上游源的选择
我们需要什么: 
* 首先是可靠性, 虽然chrony能在离线保持时间的准确性很长的一段时间, 但随着时间的流逝, 会越来越不精确, 在没有自建的能力环境下, 我们需要选择一些可靠性高的时钟源
* 其次是全球性, 我们需要在全球任意位置都有一个比较好的延迟表现, chrony的误差计算是与上游延迟强相关的
* 可达, 国内用不了 google facebook
* 一致性, 不同人对于[闰秒](https://zh.wikipedia.org/wiki/%E9%97%B0%E7%A7%92)的处理会有所不同, 这可能直接带来时间的严重误差; 近几年会不会再有闰秒就只有老天爷知道了(真靠天吃饭), 遇到了再说吧;

P.S. 闰秒算是挺麻烦的东西, 现在使用的 UTC 是原子秒+观测太阳时构成的奇怪玩意, 计算机就该用 TAI 国际原子时, 由各个应用自己决定时候手动补充闰秒; 近两年对于闰秒的讨论已经决定在 2035 年废除闰秒.

大伙第一反应是 aliyun 的 ntp 源, 但是也不大行了其实, 大家一看, 阿里云有五个域名, 还以为全球分布, 实际一测试, 就全在青岛 -、-; 那挑一个好看的域名好了(ntp.aliyun.com)

那看看腾讯云, 域名有点丑 而且表现也不怎么样 直接放弃

找来找去, 看到了 time.windows.com 和 time.apple.com ; Windows 的时钟源不支持ntp协议, Apple 的可用, 而且实测也还可以, 宣称自己是 Stratum 1, 考虑到苹果的用户基数, 那优先用!

time.apple.com 的源地区有限, 我们需要一个准确性 可靠性 延迟都还不错的上游; 来应对部分区域, 于是乎找到了 time.cloudflare.com ;但是看介绍是说是直接跟 Stratum 1 的服务器同步，但收到的却是 Stratum 3 的包，但作为几个的需求的平衡, 也加入了;

考虑到最差情况下我们需要保证时间的相对精确, 我们还是决定引入 [ntp pool](https://www.ntppool.org/), 直接使用主池 {0,1,2}.pool.ntp.org 和 ubuntu 的特殊域名 {0,1,2}.ubuntu.pool.ntp.org;
为啥没有用中国的子域名, 因为大部分解析都到国外了 -、-

最后随便添一个 ntp.ubuntu.com -^-

好了选完了, 该来看看其余的参数了, 研究研究文档; 还需要在不同版本间兼容, emmmm;

参数比较最先比较激进, 导致在一定概率下部分依赖时间步进的应用在出现了异常;

后续给了一个非常保守的参数, 主要是 maxdrift 保证了 chrony 不会非常激进的调整时间;

### 上线
写 ansible playbook, 自测, 提 PR 等待 reviewed, 讨论是否合适, 改改 PR, 需要变成一个新的 palybook, 重写, 自测, PR review, 讨论, 灰度. 影响相关业务 -、-, 改参数, 重新灰度. 

### 周边的工作
**一致**的时间对于大数据组件也是必不可少的; 但一致的时间不代表是准确的,
但最后跟数据平台的伙伴讨论后决定是用 chrony 逐替代 ntpd, 并在内部自建二级时钟源,
时钟源上游仔细挑选合适的节点, 尽量选择 Stratum 1 的上游, 并仅在集群内部使用;

这样子既保证了一致性, 并最大程度保障了准确性;

### 监控
可观测性是非常重要的, 监控分为白盒监控和黑盒监控; 这种基础设施变更对业务影响挺大的, 如果在灰度前就有监控, 就能越快发现问题; 能将影响降至最低; 

我们内部使用 [Netdata/Netdata](https://github.com/Netdata/netdata) 对主机进行监控. 大部分监控也是围绕这个生态展开.

#### 白盒监控
此部分主要对 chrony 本身的参数进行观测, 我们使用内部的[Netdata/go.d.plugin](https://github.com/Netdata/go.d.plugin) , 从社区fork了一份, 并改进调整后重新提交给社区, 内部版本包括一些上报功能, 方便统计, 而提供给社区的则摘除了这部分内容;

对于高精度时间的监控只能通过白盒数据计算得出;

#### 黑盒监控
此部分直接观测主机时间与特定上游的差距, 预期精度百毫秒级别
直接使用 ntpdate -q time.apple.com 命令, 并采集上报相关数据

出现秒级别误差可认为存在严重时间偏移问题.

## 一些别的

### GPS
NTP 中约定了 GPS 是 Stratum 0, 受到相对论影响, GPS 的时钟会快一丢丢, 所以卫星的原子钟频率会慢一丢丢以抵消误差, 同时会靠地面站来最终和保持时间的准确.

GPS 的报文中不包含年, 报文只能表示 1024 周-、-, 也就是说, 得不停的更新才能获得正确的时间, 运气不好倒回20年前

* [GPS week number rollover](https://en.wikipedia.org/wiki/GPS_week_number_rollover)

跟 Y2038 很像-、-, 哦 2038年 有两次倒霉的事故-、-

### [GPSd](https://gpsd.gitlab.io/gpsd/)
用来处理GPS原始信号, 然后在 2021 年发现了一点小小的问题 [A GPSD time warp](https://lwn.net/Articles/865044/)

不知道有几个倒霉蛋了-、-

我们反正也是密切的监控, 因为不知道上游都是怎么获取的时间信息-、-|||, 最后反正平稳的度过这一时刻了

## 未来
如果要达到更好的时间精度, 势必会选择 PTP; 但是所有的选择都是基于对需求的评估, 目标与实施成本平衡的艺术.

自建时间源, 可靠地传递时间, 这都是未来的工作, 共勉

## 其他人的工作
* google 在数据中心内部署了高精度时间 TrueTime, 并从 GPS 获取时间, 并使用原子钟维护时间的准确性, Google Spanner 是基于此实现了跨区域的事务执行; 详细见论文 [Spanner: Google's Globally-Distributed Database](hhttps://storage.googleapis.com/pub-tools-public-publication-data/pdf/65b514eda12d025585183a641b5a9e096a3c4be5.pdf) / [Spanner, TrueTime & The CAP Theorem](https://storage.googleapis.com/pub-tools-public-publication-data/pdf/45855.pdf)
* facebook 的高精度时间同步 [Building a more accurate time service at Facebook scale](https://engineering.fb.com/2020/03/18/production-engineering/ntp-service/) / [PTP: Timing accuracy and precision for the future of computing](https://engineering.fb.com/2022/11/21/production-engineering/future-computing-ptp/) / [How Precision Time Protocol is being deployed at Meta](https://engineering.fb.com/2022/11/21/production-engineering/precision-time-protocol-at-meta/) 


## 各种各样的参考与补充阅读

https://blog.cloudflare.com/roughtime/ 非精确时间，用在证书验证一类的非精确环境，主要考虑了可靠的时间

https://zh.wikipedia.org/wiki/WWVB 电波授时 北美协议

https://zh.wikipedia.org/wiki/%E7%B6%B2%E8%B7%AF%E6%99%82%E9%96%93%E5%8D%94%E5%AE%9A NTP

https://en.wikipedia.org/wiki/Precision_Time_Protocol PTP

https://zh.wikipedia.org/wiki/%E7%A7%92 秒

https://zh.wikipedia.org/wiki/%E4%B8%96%E7%95%8C%E6%97%B6 世界时

https://zh.wikipedia.org/wiki/%E6%81%92%E6%98%9F%E6%97%B6 恒星时

https://zh.wikipedia.org/wiki/%E5%A4%AA%E9%98%B3%E6%97%A5 太阳时

https://zh.wikipedia.org/wiki/%E5%9C%8B%E9%9A%9B%E5%8E%9F%E5%AD%90%E6%99%82 原子时

https://zh.wikipedia.org/wiki/%E5%8D%8F%E8%B0%83%E4%B8%96%E7%95%8C%E6%97%B6 协调世界时 

https://en.wikipedia.org/wiki/Leap_second 闰秒

https://www.iers.org/SharedDocs/News/EN/BulletinC.html 闰秒公告

https://hpiers.obspm.fr/eop-pc/index.php 原子时与地球时差距

https://www.usno.navy.mil/USNO/earth-orientation/eo-products/long-term 原子时与地球时差距（数据页面维护中）

https://berthub.eu/articles/tags/gnss/ 一些关于 GNSS 的信息



写的挺乱的, 随心所欲, 将就看看吧, 以上~

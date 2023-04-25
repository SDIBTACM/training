# 20230415 

我先 git commit 一把 ~

这周处理了几个 内存 方面的问题, 可以统一聊聊

## 内存错误 发现与纠正 
内存还是挺脆弱的, 所以服务器上了 ECC 内存, 用以识别内存错误, 并报告给 BIOS 及更上层的操作系统; 
内存产生的两种错误
 * 可纠正错误, 硬件会自动纠正数据返回给系统, 并报告相关信息, 敏感业务会感知到由于纠正电路带来的延迟下降.
 * 不可纠正错误, 轻则业务被kill, 系统隔离内存; 严重则是引发 kernel crash, 内核崩溃幸运的情况下可以正常通过 kexec 进入 kdump, 运气不好就是直接宕机/重启; 一般都是建议更换内存

比如你可以在一台内存存在问题的服务器看到类似的内核日志
服务器信息:
> inspur SA5112M5 4210 * 2 / 32GB * 4  
> Linux xxx 4.15.0-112-generic #113-Ubuntu SMP Thu Jul 9 23:41:39 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux 

``` 
[Wed Apr 12 11:05:16 2023] RAS: Soft-offlining pfn: 0x175321b
[Wed Apr 12 11:05:16 2023] mce: [Hardware Error]: Machine check events logged
[Wed Apr 12 11:05:16 2023] EDAC skx MC2: HANDLING MCE MEMORY ERROR
[Wed Apr 12 11:05:16 2023] EDAC skx MC2: CPU 10: Machine Check Event: 0 Bank 7: 9c00004001010090
[Wed Apr 12 11:05:16 2023] EDAC skx MC2: TSC 21f39e41532e626
[Wed Apr 12 11:05:16 2023] EDAC skx MC2: ADDR 175321bec0
[Wed Apr 12 11:05:16 2023] EDAC skx MC2: MISC 200001c080801086
[Wed Apr 12 11:05:16 2023] EDAC skx MC2: PROCESSOR 0:50657 TIME 1681297434 SOCKET 1 APIC 20
[Wed Apr 12 11:05:16 2023] EDAC MC2: 1 CE memory read error on CPU_SrcID#1_MC#0_Chan#0_DIMM#0 (channel:0 slot:0 page:0x175321b offset:0xec0 grain:32 syndrome:0x0 -  err_code:0101:0090 socket:1 imc:0 rank:0 bg:1 ba:2 row:d4d1 col:3f8)
[Thu Apr 13 07:53:54 2023] CMCI storm detected: switching to poll mode
[Thu Apr 13 07:58:55 2023] CMCI storm subsided: switching to interrupt mode
[Fri Apr 14 07:54:45 2023] CMCI storm detected: switching to poll mode
[Fri Apr 14 07:59:46 2023] CMCI storm subsided: switching to interrupt mode
[Sat Apr 15 07:55:36 2023] CMCI storm detected: switching to poll mode
[Sat Apr 15 08:00:37 2023] CMCI storm subsided: switching to interrupt mode
[Sat Apr 15 14:36:48 2023] RAS: Soft-offlining pfn: 0x1385ce8
[Sat Apr 15 14:36:48 2023] mce: [Hardware Error]: Machine check events logged
[Sat Apr 15 14:36:48 2023] EDAC skx MC3: HANDLING MCE MEMORY ERROR
[Sat Apr 15 14:36:48 2023] EDAC skx MC3: CPU 10: Machine Check Event: 0 Bank 8: 9c00004001010090
[Sat Apr 15 14:36:48 2023] EDAC skx MC3: TSC 22158a454a7656c
[Sat Apr 15 14:36:48 2023] EDAC skx MC3: ADDR 1385ce8380
[Sat Apr 15 14:36:48 2023] EDAC skx MC3: MISC 200000c000001086
[Sat Apr 15 14:36:48 2023] EDAC skx MC3: PROCESSOR 0:50657 TIME 1681569327 SOCKET 1 APIC 20
[Sat Apr 15 14:36:48 2023] EDAC MC3: 1 CE memory read error on CPU_SrcID#1_MC#1_Chan#0_DIMM#0 (channel:0 slot:0 page:0x1385ce8 offset:0x380 grain:32 syndrome:0x0 -  err_code:0101:0090 socket:1 imc:1 rank:0 bg:2 ba:0 row:616e col:218)
```
截取的这部分日志我也是第一次看, 看起来这台机器的CPU1 上的 CHANNEL 0 SLOT 0 位置的内存需要更换, 虽然错误本身是 CE(correctable error) 可以被纠正且被纠正.
但内存其实本身已经存在问题, ECC 纠正介入会引起降低性能, 业务可能感知到内存访问延迟下降, 提前更换内存总是一个好的选择.

这里大部分日志可以参考 [内核中 ras 文档](https://docs.kernel.org/admin-guide/ras.html)

但是本周遇到了两台 UCE(uncorrectable error), 其中一台的触发 UCE 比较频繁, 同时引发内核崩溃, 进入 kdump 转储内核;

处理过程如下: 业务报机器异常重启, 登录机器后当前内核日志无异常, 确认到 crash 目录下存在崩溃日志, 分析崩溃内核和崩溃日志, 可以得出 确认mce
内核日志如下
```
[352317.357441] Disabling lock debugging due to kernel taint
[352317.357511] mce: Uncorrected hardware memory error in user-access at 8c53aee80
[352317.357518] mce: [Hardware Error]: Machine check events logged
[352317.358250] Memory failure: 0x8c53ae: Sending SIGBUS to landscape-sysin:8540 due to hardware memory corruption
[352317.358572] Memory failure: 0x8c53ae: recovery action for dirty LRU page: Recovered
[367576.220954] mce: [Hardware Error]: CPU 35: Machine Check Exception: f Bank 1: bd80000000100134
[367576.221223] mce: [Hardware Error]: RIP 10:<ffffffff86ec1f1e> {copy_user_enhanced_fast_string+0xe/0x30}
[367576.221516] mce: [Hardware Error]: TSC 2e04ae4d30aca ADDR 8c53a7000 MISC 86
[367576.221739] mce: [Hardware Error]: PROCESSOR 0:50657 TIME 1680740101 SOCKET 1 APIC 31 microcode 5002f00
[367576.222036] mce: [Hardware Error]: Run the above through 'mcelog --ascii'
[367576.299252] mce: [Hardware Error]: Machine check: Data load in unrecoverable area of kernel
```
内核崩溃堆栈如下
```
PID: 16912  TASK: ffff8f90c7741e40  CPU: 35  COMMAND: "gzip"
 #0 [fffffe000070ac28] machine_kexec at ffffffff8646faf3
 #1 [fffffe000070ac88] __crash_kexec at ffffffff865589f2
 #2 [fffffe000070ad58] panic at ffffffff864a1c26
 #3 [fffffe000070ade0] mce_panic at ffffffff8644d5cc
 #4 [fffffe000070adf8] mce_severity_intel at ffffffff8644f87b
 #5 [fffffe000070ae30] do_machine_check at ffffffff8644e77c
 #6 [fffffe000070aea0] copy_user_enhanced_fast_string at ffffffff86ec1f1e
 #7 [fffffe000070af40] do_mce at ffffffff8644f0f5
 #8 [fffffe000070af50] machine_check at ffffffff870012eb
    [exception RIP: copy_user_enhanced_fast_string+14]
    RIP: ffffffff86ec1f1e  RSP: ffffb8628b687c78  RFLAGS: 00050206
    RAX: 000055dcd37a7d60  RBX: ffff8f8a453a7000  RCX: 0000000000001001
    RDX: 0000000000001000  RSI: ffff8f8a453a6fff  RDI: 000055dcd37a6d5f
    RBP: ffffb8628b687c80   R8: ffffb8628b687e08   R9: 0000000000000000
    R10: ffffb8628b687e50  R11: 0000000000000000  R12: 0000000000001000
    R13: ffffb8628b687e18  R14: 0000000000001000  R15: 0000000000001000
    ORIG_RAX: ffffffffffffffff  CS: 0010  SS: 0018
--- <MCE exception stack> ---
 #9 [ffffb8628b687c78] copy_user_enhanced_fast_string at ffffffff86ec1f1e
#10 [ffffb8628b687c78] copyout at ffffffff86926f16
#11 [ffffb8628b687c88] copy_page_to_iter at ffffffff86929e33
#12 [ffffb8628b687cd8] generic_file_read_iter at ffffffff86626d19
#13 [ffffb8628b687d48] generic_write_checks at ffffffff86622728
#14 [ffffb8628b687dc8] ext4_file_read_iter at ffffffff8679e946
#15 [ffffb8628b687e00] new_sync_read at ffffffff866dbfd2
#16 [ffffb8628b687e90] __vfs_read at ffffffff866df189
#17 [ffffb8628b687ea0] vfs_read at ffffffff866df22e
#18 [ffffb8628b687ed8] ksys_read at ffffffff866df3c7
#19 [ffffb8628b687f20] __x64_sys_read at ffffffff866df41a
#20 [ffffb8628b687f30] do_syscall_64 at ffffffff86404417
#21 [ffffb8628b687f50] entry_SYSCALL_64_after_hwframe at ffffffff8700008c
    RIP: 00007f5c7e35d151  RSP: 00007ffc9012b938  RFLAGS: 00000246
    RAX: ffffffffffffffda  RBX: 0000000000008000  RCX: 00007f5c7e35d151
    RDX: 0000000000008000  RSI: 000055dcd37a5d60  RDI: 0000000000000003
    RBP: 0000000000000003   R8: 000055dcd3789120   R9: 000000000000fe6c
    R10: 0000000000000000  R11: 0000000000000246  R12: 000055dcd379dd60
    R13: 000055dcd37a5d60  R14: 0000000000000000  R15: 0000000000000002
    ORIG_RAX: 0000000000000000  CS: 0033  SS: 002b
```
内核日志中已经写了 `Machine check: Data load in unrecoverable area of kernel`, 带外日志也展示了 `DIMM100 triggered an uncorrectable error, (SN:x, BN:x).` 可以断定内存存在损坏, 必须更换

跟厂商沟通后, 厂商说升级一下 CPLD/HDM/BIOS 说新版本能解决问题, 我是非常不乐意的升级的, 各家厂商现在喜欢一上来就叫用户升级各种东西, 我们表示拒绝, 厂商说根据文档, V812 的更新了关于 MCE 等相关信息, 可以改善相关问题
```
Upgrade Intel ME to SPS_E5_04.01.04.423.0;
Upgraded Intel Reference Code to 6104402,optimize memory error detection mechanism;
```
行吧, 那就升级一下

我本来以为是内存固件存在 bug 之类的, 花了九牛二虎之力升级后一看, 内存故障仍然存在, 机器内存变成 54GB, 闭着眼睛都能猜到是内存初始化失败, 部分内存地址被bios屏蔽, 好消息是不会挂了, 坏消息是花了 64G 的钱, 只有 54G 内存-.- 算是浪费了大家几天的时间...

## 内存错误 修复
CE 被内核捕获后, 内核会通过一些方式, 将对应的虚拟页定向到新的硬件页上, 并将损坏内存的页面屏蔽掉, 如果由于各种原因, CE 的数量很多, 注册到 MCE 寄存器的内容是有限的, 这时候有可能一部分 CE 捕获不到, 这时候业务可能感知到内存吞吐和延迟变差; 但如果存在 CE 的页面是属于内核代码段的, 内核会如何处置这部分还没有探究过.

UCE 被内核捕获后, 内核会找到对应的应用 kill 掉, 之后将损坏的内存页屏蔽掉, 此时业务就倒霉了.

如果系统本身不理睬 MCE 寄存器的内容, 那么这部分会有 bios 捕捉到, 并记录到带外日志里, 带外根据设置不同, 可能会直接让系统重启.

在硬件启动过程中, 有一步是初始化内存, BIOS 可以相关屏蔽存在问题的地址, 于是你就会看到内存存在 CE/UCE 的时候, 重启一下能恢复, 但是 meminfo 一看, 少了一丢丢内存...... 或者 10G 内存 .....

内存发现 CE 或者 UCE 一般都是尽快更换内存, 但是如果在更换内存的时候发现新内存存在问题, 此时应该交叉测试内存插槽与 cpu; 以排除 CPU 和 插槽问题, 遇到过一次 CPU 接触不良导致特定位置内存无法被正确识别, 顺便说一句, 服务器 CPU 散热器会写清楚需要多少牛的扭矩来固定内存, 这时候你就需要定扭扳手了.

## 内存错误 测试
一般使用 memtest, 对于内核使用的内存无法测试, 此时需要 memtest86.bin 等特殊工具引导至特殊操作系统下进行测试.

memtest 原理主要是反复读写内存, 触发系统 MCE 流程, 以在带外和系统内捕获错误并记录

## 内存错误 Extra
2020年还真是离谱, 反正我这边线上大概有 1000 多条内存存在缺陷, 但具体缺陷厂商未提供, 大部分内存虽然有缺陷, 但整体故障表现偏低.

但我们有一批 DELL 机器三天两头内存存在 CE/UCE, 频繁发生, 业务不堪其扰. 多次跟厂商沟通, 厂商只认可带外日志上的对于故障内存的表述, 对于内核日志完全不认可, 沟通得吐血, 机器全面内存测试需要一天左右, 需要多次测试尝试触发问题, 有时候内核因为 UCE 都已经崩溃了, 但是带外完全不显示. 最后多次沟通后, 要求原厂调查一年以来的报障机器(数十台, 超过 40 条内存)才得以确认内存存在缺陷, 并后续安排内存更换. 更换周期长达数个月....

反正是吐了...

## 内存性能 频率与延迟
周末在公司跟同事聊天, 聊到了说 DDR5 与 DDR4 现在性能差距其实不大, 可能就相差 20% 左右, 目前 DDR4 频率可以到 4800Mhz, CL 值可以到 19 算下来延迟也就 7.9ns 多一点, 而现在 DDR5 最高频率在 8000, 但 CL 在 38, 这么计算延迟在 9.5ns 多一些; 也就是现阶段 (2023年4月) 其实 DDR5 顶级内存在延迟上打不过 DDR4 的顶级内存; 但是带宽由于频率的提升确实能有比较大的提升, 但是你总不能天天都在往内存里拷贝数据吧(上个 1T 内存, 当作各种缓存用, 牛的

但是呢 现在新一点的芯片组, 大家已经不做 DDR4 的主板了, 性能又没提升太多, 又贵; 真是离谱.

还是接着 DDR4 吧, 几年后再看吧...

## 总结
写得比较随意, 将就看看, 以后尽量每周更新, 不限任何主题, 不限制来源; 想看啥也可提, 会的我就而写点

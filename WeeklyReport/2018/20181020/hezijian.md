# 20181020

这周还是没干什么，回来之后装了一堆机子的系统，走之前让大二的装，回来一看，什么都没有变化，那只能自己来了

周四起床一看邮箱，github发了一堆邮件告诉我说sdibtacm/sdibtvoj 使用的 struct 和 spring 框架都具有漏洞,让升级. 于是简单的按照建议直接修改版本, github 提供的建议是 一个区间, 但会导致编译错误, 把版本写死了就没有这个问题了; 运行的时候却发现, 一直无法启动, 看了日志好久和 Google 了半天, 才弄明白因为 spring 从4.1.x版本过后, 改变了函数的使用, 与 quartz 绑定时调用某个2.x.x的才有函数, 就直接上官网找到了新的版本, 并修改 pom 文件, 然后由于 quart 升级, 所以 applicationContext 中的绑定对应的函数也得修改. 由于对Java不熟悉, 所以弄了很久, 大概两个小时吧  

剩下的时间就先设计了 训练计划 的数据库表, 由于不是很有经验, 在 dalao 的帮助下前前后后弄了半天, 然后就开始写后端, 第一次从头开始写东西, 希望能又好又快的完成~~

又有坑要填了Orz...

PS. 本周 Ubuntu 18.10 正式推出, 关于新特性,以及如何升级, 欢迎查看 [从 Ubuntu 18.04 升级到 Ubuntu 18.10](https://boxjan.com/2018/10/update_ubuntu_from_18_04_to_18_10.html)![Ubuntu-18-10-simple](https://static.boxjan.com/wp-content/uploads/2018/10/ubuntu-18-10-simple.png) 
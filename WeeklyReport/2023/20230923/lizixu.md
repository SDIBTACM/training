# 本周完成
## 先是好几门的测验
从线代开始，好不容易才把第一章弄机密，第二章有糊涂了

大物和算法没啥好说的，都是正常的东西

数字逻辑啊啊，为什么我会傻乎乎的把卡诺图的标号弄错了呀，还是测验的时候才发现

## 另外就是打了个网络赛了把
算上上周的一共两场

说说第一场把
### a题
一个队伍排名问题，去重排序再去重就好了
### l题
一个签到题，就是2和t/T找最大
### j题
这个我虽然过了，但考虑的并不全面，最后一个点的距离应该是一个菱形，
但我只考虑了一条边（也就是没考虑绝对值），不过最后结果一样，都是两圆心距离
减去第二个圆的半径除根号二
### d题
想到了找联通子图，最后的代码实现是队友弄出来的，为队友点个赞

第二场的话
### m题
一个期望排序的问题，就是读题读傻了，看了好半天才知道要杂木算
### d题
一个站在一个点上，可以覆盖一行和一列，问全覆盖的代价是多少
其实很简单就是一行找（列）一个最小加起来，比较一下取最小
但是因为那个可恶的负数wa了一次，负数全加，越加越小
### e题i题
这俩提没过，大概提一下思路吧

e题的话是想着用最近公共祖先，把树改成图（一个点一下可以到其他点的图），然后广搜。

i题就是模拟了把，想着把每个阶段的期望算出来，比较一下就好，最后是精度问题一直不合格，算了一下如果是0.5的话
要达到他的误差以外需要32次好像。

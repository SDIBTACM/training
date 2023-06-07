#### 省赛过程及反思



##### day_1 

* 早上早起出发前往济南,中午就到达汉庭酒店,简单休息了一会去 $QLU$报道,下午进行了热身赛,前三个题还是容易的,被最后一题卡了很久,想着用**二分**来解决(结果是个假做法,其实答案并不满足单调性),造了许多样例也没过,还好帆哥换了**贪心**的策略就过了.

* 晚上吃完饭,回去已经是 $8$点过了,打了会 $Atcoder$,敲完前三个题(花了不到半个小时)官网就开始崩了,当时看了一眼 $D$,没撒思路,然后开了 $E$,发现是个图论题,做了一下发现是个并查集(写得比较暴力,发现不太会优化加之评测崩溃的原因),等到比赛结束后，补了一下 $D$时二分找到它对应的  $x$和 $y$所属的块, $E$则是需要将对应的边的两个点的祖先结点存入集合中,再判断新插入的边的两个点的祖先结点是否在集合中.

* 后续到了 $11$点, jyf和 lcy找到了热身赛题 $D$ 的 $bug$, 第一天草草结束

```
4 2
3 4 1 2

6----3本
7----2本
8----3本
不满足单调性(不能用二分)

贪心的做法是先选前m本(前面的必选),然后在最后剩余本数中找最小值,结果就为前m本的钱+最小值-1,但是还得将0元的书先抛开再进行计算.
```

##### day_2

###### 赛前

* 早上醒的挺早的( $5$点过),吃完早饭,队友才刚开始吃,先走一步带领 $4$队去比赛场地(第二批出发),当我到时,队友已经到了.

* 比赛时间 $9$点开始,(题目是三份**中文题面**和一份英文题面)

###### 1h-2h(9:00-11:00)

紧跟整体过题的节奏,先后过了 $A,I$两道签到题,然后就开始准备多开题,看了一眼 $G$, 发现移项后变成了 $a_i-i=a_j-j$,就可以做了,将同类的放在一起, $G$在 $33$分钟就过了,前三题挺顺利的,看了一眼榜,发现 $D$的通过率挺多的,打算冲一下 $D$,f哥和cy试了**贪心**,没过(由于热身赛被假二分的做法折磨,正式赛就没多往二分的路探索,一眼顶真(贪心)).

###### 3h(11:00-12:00)

试着看看其他题能否再做,看了一眼 $B$,有点像操作系统里的银行家算法,但是不会优化,f哥看了一下 $E$,和我交流了一下思路,是个进制问题(说实话还是没太懂),差点连进制转化都不会蒜(算)了,主要精力还在 $D$题上.

###### 4h-5h(12:00-14:00)

封榜前 $30$分钟,发现 $L$的过题率猛增,cy直接就想到了就是上学期老板讲的`棋盘覆盖`问题,将 边长为 $2^k$的正方形扩展为更为普通的边长为 $n$的正方形用 $L$形的块进行填满,cy挺强的(递归这玩意雀氏挺绕的,加之我上学期开学时在家讲到棋盘覆盖时只是粗略懂了点皮毛,全靠cy),花了一个多小时把这个题调了几次(细节没处理好),过完 $L$后,就剩下不到 $10$分钟, $D$着实没想到怎么做(感觉 $D$题有点带偏榜,其实是太菜了),正式赛就结束了.

###### 赛后

* 参加了闭幕式和颁奖仪式(山东高校都在猛抓 $XCPC$和 $ACM$,青岛大学真的强(5金6银,所有队伍都拿奖了),确实应该加训,看到出实力差距太明显了)
* 回学校 $\cdots \cdots$


##### 反思

从大一到现在参加的区域赛和省赛,虽然经历过好 $3$次的线下的比赛(第一次沈阳**初生牛犊**(签完到就罚坐),第二次西安(三题差罚时,四题冲不动),第三次济南省赛(差题数,太蒟蒻了)),确实辜负了毛老师和肖老板的期待.

说实话 $XCPC$一半运气一半实力,运气不暂优势,队友的思维比我强,我的思维总是比他们慢半拍.

其次,想说的是学校现在整体的状态不太行,确实该大为调整,除了每周打完校内的比赛,很少抽出其他时间练练其他题(比如小白月赛,或者 $Atcoder$等等,但我尽量有时间就做做,光做每周抓的比赛).

等打完蓝桥杯,重心就侧重放在`六级和考研`上了,希望以后的几届能发光发亮吧.

#### 省赛后补题

##### $D-Fast\ and\ Fat$ 

`贪心`得**贪两次**, 能力值表示其重量$w_i$,难度表示为 $w_i+v_i$,`二分答案`为速度 $x$,速度小于 $x$的必须将其安排,能力值越高的应该安排难度越大的.

```cpp
// Problem: D. Fast and Fat
// Contest: Codeforces - The 13th Shandong ICPC Provincial Collegiate Programming Contest
// URL: https://codeforc.es/gym/104417/problem/D
// Memory Limit: 1024 MB
// Time Limit: 2000 ms
// Date: 2023-06-05 16:52:21

/**
  * @author  : SDTBU_LY
  * @version : V1.0.0
  * @上联    : ac自动机fail树上dfs序建可持久化线段树
  * @下联    : 后缀自动机的next指针DAG图上求SG函数
**/

#include<iostream>
#include<cstring>
#include<algorithm>
#include<vector>

#define MAXN 100010

#define int long long

using namespace std;

//typedef long long ll;

int t,n;

struct node{
    int v;
    int w;
    int sum;
    int id;
}s[MAXN],s1[MAXN],s2[MAXN];

int cmp1(node a,node b)//能力值
{
    return a.sum>b.sum;
}

int cmp2(node a,node b)//难度
{
    return a.w>b.w;
}

int check(int num)
{
    
    vector<int> v1,v2;
    for(int i=1;i<=n;i++)
    {
        if(s2[i].v<num)
            v1.push_back(s2[i].id);
        if(s1[i].v>=num)
            v2.push_back(s1[i].id);
    }
    
    if(v2.size()<v1.size())
        return 0;
    
    
    /*
    s1[i].w<=s2[i].w 
    s2[i].v-(s1[i].w-s2[i].w)>=num
    s2[i].v-num+s2[i].w>=s[1].w
    */

    for(int i=0;i<v1.size();i++)
    {
        int pos2=v2[i],pos1=v1[i];
        if(s[pos1].w>s[pos2].w&&(s[pos2].v-(s[pos1].w-s[pos2].w)<num))
            return 0;        
    }
    
    return 1;
}

signed main()
{
    // ios::sync_with_stdio(false);
    // cin.tie(0),cout.tie(0);
    
    cin >> t;
    
    while(t--)
    {
        cin >> n;
        
        for(int i=1;i<=n;i++)
        {
            cin >> s[i].v >> s[i].w;
            s[i].sum=s[i].v+s[i].w;
            s[i].id=i;
            s2[i]=s1[i]=s[i];
        }
        
        sort(s1+1,s1+n+1,cmp1);
        sort(s2+1,s2+n+1,cmp2);
        /*
        for(int i=1;i<=n;i++)
            cout << s[i].v << " " << s[i].w << " " << s[i].sum << endl; 
        */
        int l=0,r=1e18;
        while(l<r)
        {
            int mid=(l+r+1)/2;
            
            if(check(mid)==1)
                l=mid;
            else r=mid-1;
            //cout << l << " " << r << endl;
        }
        
        cout << l << endl;
        
        //cout << endl;
    }
    
    return 0;
}
```


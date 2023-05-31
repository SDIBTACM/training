**组队赛第四周**

- jyf
- 刘骋羽
- 廖银

### 比赛地址
[sdtbu组队赛4](https://vjudge.net/contest/560008)

+ [x] A-Broken Keyboard
+ [x] B-Watch the Videos
+ [x] C-Exchange
+ [ ] D-Hospital Queue
+ [x] E-Torus Path
+ [ ] F-Project Manager
+ [x] G-Minimum LCM
+ [x] H-Number Reduction

### 做出来的题目

#### A-Broken Keyboard

##### 题意

> 给定一个空字符串 $S$,当按压次数为**奇数**时,且字符为 $c$,将一个 $c$插入到 $S$中,反之,按压次数为**偶数**时,则将两个 $c$ 插入到 $S$中。求给定 $t$组中,每组字符串 $s$是否满足以上条件.

##### 思路

> **模拟**,设 $len$为字符串的长度且下标从 $1$开始,字符串每三个字符一组,只需判断第 $i\times 3-1$和 $i\times 3$ 对应的字符(其中 $1 \leqslant i \leqslant len/3 $)是否都相等,若满足输出 `YES`,不满足输出`NO`.此处用**字符数组**存储,防止 $string$判断时越界访问.

##### 代码

```cpp
#include<iostream>
#include<cstring>
#include<algorithm>

using namespace std;

typedef long long ll;

int t,n;
char s[110];

int main()
{
    // ios::sync_with_stdio(false);
    // cin.tie(0),cout.tie(0);
    cin >> t;
    while(t--)
    {
        cin >> n >> s+1;

        int len=strlen(s+1);

        int flag=1;
        for(int i=2;i<=len;i+=3)
            if(s[i]!=s[i+1])
            {
                flag=0;
                break;
            }
        if(flag==0)
            cout << "NO" << endl;
        else cout << "YES" << endl;
    }
    return 0;
}
```
----


#### B-Watch the Videos

##### 题意

> 给定 $n$部视频和一个电脑硬盘容量为 $m$大小空间,每部电影容量(时间)为  $a_i$,观看每部电影需要的时间为 $1$,当硬盘剩余空间 $r$足够且正在观看时,可以同步下载满足条件的电影(即 $a_i \leqslant r$),看完视频后可通过删除视频腾出空间,问你需要看完所有电影花费的时间.

##### 思路

> **贪心**
>
> * 在不考虑**硬盘容量**限制的条件下,必然需要花费 $\color{Red}{\displaystyle (\sum_{i=1}^{n}a_i )+1}$(如下图所示,在观影阶段同步下载另一部视频)
>
> ![QQ图片20230531195907.png](https://cdn.acwing.com/media/article/image/2023/05/31/85276_92c735f9ff-QQ图片20230531195907.png) 
>
> * 若考虑**硬盘容量**限制的条件下,怎么贪心会更好呢?肯定是**一大一小**相互组合最优,若当前`一大一小的容量和`大于`硬盘容量` $m$,则需要在一部视频看完后,才能接着下载(**为什么不能选择其他的呢?**因为此时的一大一小必为当前剩余视频中的最大值和最小值,选择其他的反而不会有更优的情况:若将最小值替换,则无效果;若将最大值替换,虽然这轮有可能成功安排完,但在下一轮仍将用到这个最大值,故不会更优)。
>
> * **注意**:尽量`让小的去带动大的`会更优.
>
>   ![QQ图片20230531201507.png](https://cdn.acwing.com/media/article/image/2023/05/31/85276_d5144454ff-QQ图片20230531201507.png) 
>
> * 总结,当前`一大一小的容量和`大于`硬盘容量` $m$时,需要在原来的基础上加 $1$,反之则继续往里面走,该题需要`排序`.

##### 代码

```cpp
#include<iostream>
#include<cstring>
#include<algorithm>

#define MAXN 200010

using namespace std;

typedef long long ll;

int n;

ll m,a[MAXN];

ll res;

int main()
{
    // ios::sync_with_stdio(false);
    // cin.tie(0),cout.tie(0);

    cin >> n >> m;

    for(int i=1;i<=n;i++)
    {
        cin >> a[i];
        res+=a[i];
    }
    sort(a+1,a+n+1);

    int l=1,r=n;

    while(l<r)
    {
        //cout << l << " " << r << endl;
        if(a[l]+a[r]<=m)
            l++;
        else res++;

        //cout << res << endl;
        r--;
    }
    cout << res+1 << endl;
    return 0;
}
```
----
#### C-Exchange

##### 题意

> 已知 $1$枚金币可以换取 $a$枚银币, $b$枚银币可以换取 $1$枚金币,同时,每完成 $1$个任务可获得 $1$枚金币,问至少需要完成多少任务才能拥有 $n$枚银币.

##### 思路

> **思维题**
>
> * 当 $a \leqslant b$时,此时只能通过完成任务才能拥有 $n$枚银币,次数为 $\left \lceil \frac{n}{a} \right \rceil$.
> * 当 $a>b$时,只需要获得 $1$个金币,即可通过`金-银-金-银`实现银币翻倍的效果.

##### 代码

```cpp
#include <iostream>

using namespace std;

int main()
{
    int _;
    cin >> _;
    while(_--)
    {
        int n, a, b;
        cin >> n >> a >> b;
        if(a <= b)
        {
            int ans = n % a ? n / a + 1 : n / a;
            cout << ans << endl;
        }
        else
            cout << 1 << endl;
    }
    return 0;
}
```
----
#### E-Torus Path

##### 题意

> 在 $n\times n$的正方形中,每个位置都有对应的权值 $a_{ij}$,起点坐标为 $(1,1)$,终点坐标为 $(n,n)$.你可以向`右移或者下移`,当位于坐标 $(i,n)$且往右移后到达 $(i,1)$,同理位于坐标 $(n,j)$且往下移后到达 $(1,j)$.不能访问已经访问过的点.问你从起点到达终点所能获得的最大权值和.

##### 思路

> ~~说句实话,当时比赛的时候直接猜的样例,即`所有数的和-副对角线的最小值`,正解雀氏是这个样子~~
>
> **证明**: 副对角线上的点最多访问 $n-1$次(即点 $(1,n)$,点 $(2,n-1)$, $\cdots$,点 $(n,1)$ 这 $n$ 个点**不能同时**访问 ).
>
> 
>
> **明天补上**
>
> 
>
> 答案为 $\color{Red}{\displaystyle\sum_{i=1}^{n} \sum_{j=1}^{n} a[i][j] -min_{1\leqslant k \leqslant n} a[k][n+1-k] }$



##### 代码

```cpp
#include<iostream>
#include<cstring>
#include<algorithm>

#define MAXN 210

using namespace std;

typedef long long ll;

const int inf=0x3f3f3f3f;

int n;

ll a[MAXN][MAXN];
ll sum;

ll minx=inf;

int main()
{
    // ios::sync_with_stdio(false);
    // cin.tie(0),cout.tie(0);

    cin >> n;

    for(int i=1;i<=n;i++)
        for(int j=1;j<=n;j++)
        {
            cin >> a[i][j];
            sum+=a[i][j];
        }

    for(int i=1;i<=n;i++)
        minx=min(minx,a[i][n-i+1]);

    cout << sum-minx << endl;

    return 0;
}
```


----
#### G-Minimum LCM

##### 题意



##### 思路





##### 代码


----
#### H-Number Reduction

##### 题意



##### 思路





##### 代码




### 补的题目

#### D-Hospital Queue



##### 题意



##### 思路





##### 代码










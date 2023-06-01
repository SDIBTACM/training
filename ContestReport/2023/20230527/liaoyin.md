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
> ![图片](https://cdn.acwing.com/media/article/image/2023/05/31/85276_92c735f9ff-QQ图片20230531195907.png) 
>
> * 若考虑**硬盘容量**限制的条件下,怎么贪心会更好呢?肯定是**一大一小**相互组合最优,若当前`一大一小的容量和`大于`硬盘容量` $m$,则需要在一部视频看完后,才能接着下载(**为什么不能选择其他的呢?**因为此时的一大一小必为当前剩余视频中的最大值和最小值,选择其他的反而不会有更优的情况:若将最小值替换,则无效果;若将最大值替换,虽然这轮有可能成功安排完,但在下一轮仍将用到这个最大值,故不会更优)。
>
> * **注意**:尽量`让小的去带动大的`会更优.
>
>   ![图片](https://cdn.acwing.com/media/article/image/2023/05/31/85276_d5144454ff-QQ图片20230531201507.png) 
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

> **思维题**
>
> ~~说句实话,当时比赛的时候直接猜的样例,即`所有数的和-副对角线的最小值`,正解雀氏是这个样子~~
>
> **证明**: 副对角线上的点最多访问 $n-1$次(即点 $(1,n)$,点 $(2,n-1)$, $\cdots$,点 $(n,1)$ 这 $n$ 个点**不能同时**访问 ).
>
> 已知副对角线上的坐标为 $(i,n+1-i)$,它可由左边的点 $(i,n-i)$ 或者从上面的点 $(i-1,n+1-i)$ 转移而来。
>
> |  到达点   |        上面的点         |         左边的点         |
> | :-------: | :---------------------: | :----------------------: |
> |  $(1,n)$  | $\color{Purple}{(n,n)}$ |  $\color{Red}{(1,n-1)}$  |
> | $(2,n-1)$ | $\color{Red}{(1,n-1)}$  | $\color{Blue}{(2,n-2)}$  |
> | $(3,n-2)$ | $\color{Blue}{(2,n-2)}$ | $\color{Green}{(3,n-3)}$ |
> | $\cdots$  | $\color{Green}{\cdots}$ |         $\cdots$         |
> |  $(n,1)$  |        $(n-1,1)$        | $\color{Purple}{(n,n)}$  |
>
> 由上表知:共有 $n$个这样的点(即 $(1,n-1)$, $(2,n-2)$, $(3,n-3)$, $\cdots$, $(n,n)$),其中 $(n,n)$点为终点,不能再往外**扩展**,故最多只能经过 $n-1$个副对角线上的点。
>
> 若选择**非副对角线**上的点,则能访问点会减少,反而让最终的结果变小(适当带点贪心)
>
> * **举例**: 给定 $6\times 6$的正方形,假设不通过点$(5,2)$,如下图所示
>
>   ![图片](https://cdn.acwing.com/media/article/image/2023/06/01/85276_ee989fc000-QQ图片20230601162532.png) 
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

> 有 $t$组数据,每组给出一个整数 $n$，找出两个正整数 $a,b$，使其满足 $a+b=n$ 且 $a,b$ 的最小公倍数为 $a,b$ 的所有可能值中最小的一组。如有多解，输出任意一组。

##### 思路

> **数论**
>
> * 当 $n$ 为质数时,选择 $1$和 $n-1$最优.
> * 当 $n$不为质数时,假设 $k$为 $n$能整除的最小值(即 $n$的排除 $1$外的最小因数),则此时一份为 $\frac{n}{k}$,另一份为 $n-\frac{n}{k}$此时为最优解.

> **证明**:
>
> 设:  $a=p_1 \times k$, $b=p_2 \times k$, $s=lcm(a,b)$,其中 $p_1,p_2$互质.
>
> 由于 $a+b=n$,故 $(p_1+p_2)\times k=n$
>
> 又由于 $lcm(a,b)=s$,故 $p_1\times p_2 \times k=s$
>
> 当 $s < n$时, $p_1 \times p_2 < p_1+p_2$ .
>
> 可发现当 $min(p_1,p_2)=1$时才能成立(假设  $min(p_1,p_2)\geqslant 2$,此时 $p_1 \times p_2 \geqslant p_1+p_2$,此时不满足条件)
>
> 故令设: $a=m$, $b=K\times m$ ,(即 $a < b$), 此时 $S=lcm(a,b)=b=K\times m$
>
> 由于 $a+b=n$, 代入式子得: $m\times (K+1)=n$
>
> 故 $S=K\times m=n-m$
>
> 若让 $S$最小, $n$已知,故只能让 $m$最大, $K$最小.

> **综上**: 可通过 $O(\sqrt{n})$的复杂度枚举即可.

##### 代码

```cpp
#include<iostream>
#include<cstring>
#include<algorithm>
#include<cmath>

#define int long long
using namespace std;
int t,n;

signed main()
{
    // ios::sync_with_stdio(false);
    // cin.tie(0),cout.tie(0);
    cin >> t;
    while(t--)
    {
        cin >> n;
        int flag=0;

        int a,b;
        for(int i=2;i<=sqrt(n);i++)
        {
            if(n%i==0)
            {
                int cnt=n/i;
                a=cnt,b=n-cnt;
                cout << a << " " << b << endl;
                flag=1;
                break;
            }
        }
        if(flag==0)
            cout << 1 << " " << n-1 << endl;

    }
    return 0;
}
```


----
#### H-Number Reduction

##### 题意

> 给定一个正整数 $x$，你可以移除 $x$ 的任意一位，前提是移除后的 $x$ 不含前导 0，且仍是一个正整数。求 $k$ 次操作后 $x$ 的最小值。
>
> 一共 $t$ ( $1 \leqslant t \leqslant 10^5$ )  组数据。
>
> 对于每组数据，给出 $x$ ( $1 \le x < 10^{500000}$ ) 和 $k$ ( $0 \leqslant  k < |x|$ )。其中 $|x|$ 表示 $x$ 的位数。
>
> 对于每组数据，输出 $k$ 次操作后 $x$ 的最小值。
>
> 对于 $100\%$ 的数据满足： $\displaystyle \sum_{i=1}^t|x_i| \le 5 \cdot 10^5 $
>
> 有多测测试样例。

##### 思路

> **贪心**
>
> 用**线段树**维护区间的最小值(方法有点大材小用,但是能用就行)
>
> 始终尽量保留最小的数(注意第一次需要**特判**).
>
> 设当前考虑到第 $i$位,剩余可删除的位数为 $k$位.找到最小的在 $[i,i+k-1]$位出现过的数字,跳到它的下一个出现的位置 $p$,将 $[i,p-1]$之间的数全部删掉。
>
> 时间复杂度为 $O(nlogn)$,此题亦可用 $O(n)$的时间复杂度解决.

##### 代码1(线段树)

```cpp
#include <iostream>
#include <cstring>
using namespace std;
#define N 500005
char s[N];
int number[N];
struct Node
{
    int l;
    int r;
    int pos;
}node[N << 2];
void push_up(int rt)
{
    if(number[node[rt << 1].pos] <= number[node[rt << 1 | 1].pos])
        node[rt].pos = node[rt << 1].pos;
    else
        node[rt].pos = node[rt << 1 | 1].pos; 
}
void build(int rt, int l, int r)
{
    if(l == r)
        node[rt] = {l, r, l};
    else
    {
        node[rt] = {l, r, 0};
        int m = l + r >> 1;
        build(rt << 1, l, m);
        build(rt << 1 | 1, m + 1, r);
        push_up(rt);
    }
}
int query(int rt, int l, int r)
{
    if(node[rt].l >= l && node[rt].r <= r)
        return node[rt].pos;
    else
    {
        int m = node[rt].l + node[rt].r >> 1;
        int pos1 = 0, pos2 = 0;
        if(l <= m)
            pos1 = query(rt << 1, l, r);
        if(r > m)
            pos2 = query(rt << 1 | 1, l, r);
        if(pos1 != 0 && pos2 == 0)
            return pos1;
        else if(pos1 == 0 && pos2 != 0)
            return pos2;
        else
        {
            int mn1 = number[pos1];
            int mn2 = number[pos2];
            if(mn1 <= mn2)
                return pos1;
            else
                return pos2;
        }
    }
}
int main()
{
    ios::sync_with_stdio(0);
    cin.tie(0), cout.tie(0);
    int _;
    cin >> _;
    while(_--)
    {
        int n, k;
        cin >> s + 1;
        cin >> k;
        n = strlen(s + 1);
        for(int i = 1; i <= n; i++)
            number[i] = s[i] - '0';
        build(1, 1, n);
        string ans;
        int pos = 0, mn = 1e9;
        for(int i = 1; i <= k + 1; i++)
        {
            if(number[i] != 0 && number[i] < mn)
            {
                mn = number[i];
                pos = i;
            }
        }
        ans += s[pos];
        
        //cout << ans << endl;
        for(int i = k + 2; i <= n; i++)
        {
            pos = query(1, pos + 1, i);
            ans += s[pos];
            
            //cout << pos << " " << ans << endl;
            // if(pos == i)
            // {
                // for(int j = i + 1; j <= n; j++)
                    // ans += s[j];
                // break;
            // }
            //cout << ans << endl;
        }
        cout << ans << endl;
    }
    return 0;
}
```

##### 代码2(线性时间)

```cpp
// Problem: H - Number Reduction
// Contest: Virtual Judge - sdtbu组队赛4
// URL: https://vjudge.csgrandeur.cn/contest/560008#problem/H
// Memory Limit: 512 MB
// Time Limit: 2000 ms
// Date: 2023-06-01 18:35:57

/**
  * @author  : SDTBU_LY
  * @version : V1.0.0
  * @上联    : ac自动机fail树上dfs序建可持久化线段树
  * @下联    : 后缀自动机的next指针DAG图上求SG函数
**/

#include<iostream>
#include<cstring>
#include<algorithm>
#include<queue>

#define MAXN 15

using namespace std;

typedef long long ll;

int t,k;
string s;

int main()
{
    // ios::sync_with_stdio(false);
    // cin.tie(0),cout.tie(0);
    cin >> t;
    while(t--)
    {
        queue<int> q[MAXN];
        
        cin >> s >> k;
        
        int len=s.size();
        
        for(int i=0;i<len;i++)
            q[s[i]-'0'].push(i);
        
        len=len-k;
        
        int pre=-1;
        
        string res="";
        
        for(int i=1;i<=len;i++)
        {
            for(int j=(i==1?1:0);j<=9;j++)
            {
                while(q[j].size()>0&&q[j].front()<=pre)
                    q[j].pop();
                
                if(q[j].size()>0)
                {
                    if(q[j].front()-pre-1<=k)
                    {
                        k-=q[j].front()-pre-1;
                        res+=char(j+'0');
                        pre=q[j].front();
                        break;
                    }
                }                
                
            }
        }
        
        cout << res << endl;
    }

    return 0;
}
```


### 补的题目

#### D-Hospital Queue

##### 题意

> 有 $n$ 个病人来排队看病，一种合法的排队方案需要满足以下条件：
>
> 一、 ${\forall}$ $1$ ${\leq}$ $i$ ${\leq}$ $n$，编号为 $i$ 的病人必须排在前 $p_i$  位。
>
> 二、 ${\forall}$ $1$ ${\leq}$ $i$ ${\leq}$ $m$，编号为 $a_i$ 的病人必须排在编号为 $b_i$ 的病人前面。
>
> 你需要对于每一位病人，求出在所有合法的排队方案中，他能排在的最靠前的位置。
>
> **数据保证一定存在合法的排队方案。**
>
> 保证 $1$ ${\leq}$ $n$ ${\leq}$ $2000$； $0$ ${\leq}$ $m$ ${\leq}$ $2000$

##### 思路

> ~~最开始想用**差分约束**解决,发现能找到相对位置,仍无法构造出结果~~
>
> **拓扑排序+二分+优先队列+图论** $\to$**思维题**
>
> 将 $n$个人看作点, $m$组有向关系看作边,可构建一个图。由已知,若图中**存在环**,必然无解。而题目中给出**数据保证一定存在合法的排队方案**,故该图一定是**有向无环图**,可考虑采用使用`拓扑排序`,但是该题存在 $p_i$的限制,故普通的拓扑排序无法得到合法方案(入度为 $0$的点放入队列的顺序会影响到最终结果)。
>
> 设 $minval_i$表示从 $i$出发,能走到的所有点 $j$中(包含 $i$点),最小的 $p_j$.
>
> 对每一个人,进行**二分**答案,若当前第 $i$个人二分的答案是 $x$,在检验的时候把 $p_i$对 $x$取最小值,然后按照上述方法跑一遍**拓扑排序**。
>
> 但是,由于朴素的实现拓扑排序,需要用到**优先队列**。二分答案的时间复杂度是 $O(logn)$,优先队列的时间复杂度也为 $O(logn)$,总的时间复杂度为 $O(n^2log^2n)$,这种复杂度过不了.需要在 $O(n)$的时间内,把原本 $O(nlogn)$的拓扑排序模拟出来,以此把总的时间复杂度降为 $O(n^2logn)$
>
> **结论**: $\forall 1\leqslant i,j\leqslant n$,如果从 $i$出发能到达 $j$，则 $minval_i$一定小于等于 $minval_j$,根据 $minval$数组,可以在 $O(n)$时间内计算出每个点的拓扑排序.

##### 代码

```cpp
// Problem: D - Hospital Queue
// Contest: Virtual Judge - sdtbu组队赛4
// URL: https://vjudge.csgrandeur.cn/contest/560008#problem/D
// Memory Limit: 512 MB
// Time Limit: 3000 ms
// Date: 2023-06-01 21:06:15

/**
  * @author  : SDTBU_LY
  * @version : V1.0.0
  * @上联    : ac自动机fail树上dfs序建可持久化线段树
  * @下联    : 后缀自动机的next指针DAG图上求SG函数
**/

#include<iostream>
#include<cstring>
#include<algorithm>

#define MAXN 2010
#define MAXM 2010

using namespace std;

typedef long long ll;

int n,m;

int p[MAXN];
int head[MAXN],ed[MAXM],nex[MAXM],idx;

void add(int a,int b)
{
    ed[idx]=b;
    nex[idx]=head[a];
    head[a]=idx++;
}

int minval[MAXN],sum[MAXN];

void dfs(int root,int fa)
{
    minval[root]=p[root];
    
    for(int i=head[root];i!=-1;i=nex[i])
    {
        int j=ed[i];
        if(j!=fa)
        {
            if(minval[j]==0)
                dfs(j,root);
            minval[root]=min(minval[root],minval[j]);
        }
    }
}

int check(int pos,int num)
{
    memset(minval,0,sizeof(minval));
    memset(sum,0,sizeof(sum));
    
    int temp=p[pos];
    p[pos]=num;
    
    for(int i=1;i<=n;i++)
        if(minval[i]==0)
            dfs(i,-1);
    
    for(int i=1;i<=n;i++)
        sum[minval[i]]++;
    
    for(int i=1;i<=n;i++)
        sum[i]+=sum[i-1];
    
    for(int i=1;i<=n;i++)
        if(sum[minval[i]]>p[i])
        {
            p[pos]=temp;
            return 0;
        }
    
    p[pos]=temp;
    return 1;
    
}

int main()
{
    // ios::sync_with_stdio(false);
    // cin.tie(0),cout.tie(0);
    
    memset(head,-1,sizeof(head));
    cin >> n >>m;
    
    for(int i=1;i<=n;i++)
        cin >> p[i];
    
    for(int i=1;i<=m;i++)
    {
        int a,b;
        cin >> a >> b;
        add(a,b);
    }

    for(int i=1;i<=n;i++)
    {
        int l=1,r=p[i];
        
        int res=0;
        
        while(l<=r)
        {
            int mid=(l+r)/2;
            if(check(i,mid)==1)
            {
                res=mid;
                r=mid-1;
            }
            else l=mid+1;
            
        }
        cout << res << " ";
    }
    
    cout << endl;

    return 0;
}
```

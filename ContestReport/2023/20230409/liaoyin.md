### A-Asking for Money

[题目链接](https://codeforces.com/gym/104252/problem/A)

#### 思路

给定 $n$个人和每个人 $i$对应的两个借钱人 $a_i$ 和 $b_i$, 问 每个人 $i$ 是否会亏钱.

假定看第 $i$个人,追溯到 $a_i$ 和 $b_i$,如果 $i$会亏钱,说明有人向 $a_i$, $b_i$, $i$借钱在 $i$ 向 $a_i$ 和 $b_i$ 借钱`之前`.

若**正向建图**,实现复杂度为 $O(n^3)$,故考虑`反向建图`,可将时间复杂度降为 $O(n^2)$去判断.

#### 代码

```c++
// Problem: A - A
// Contest: Virtual Judge - sdtbu选拔赛3
// URL: https://vjudge.net/contest/552058#problem/A
// Memory Limit: 1024 MB
// Time Limit: 500 ms
// Date: 2023-04-09 14:01:00

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

#define MAXN 1010
#define MAXM 3010

using namespace std;

typedef long long ll;

int n;

int head[MAXN],ed[MAXM],nex[MAXM],idx;

struct node{
    int a;
    int b;
}s[MAXN];

void add(int a,int b)
{
    ed[idx]=b;
    nex[idx]=head[a];
    head[a]=idx++;
}

int vis[MAXN],cnt[MAXN];


void dfs(int root,int fa)//遍历不能到fa的所有点
{
    vis[root]=1;
    for(int i=head[root];i!=-1;i=nex[i])
    {
        int j=ed[i];
        if(fa!=j&&vis[j]==0)
            dfs(j,fa);      
    }
}

int main()
{
    // ios::sync_with_stdio(false);
    // cin.tie(0),cout.tie(0);
    cin >> n;
    memset(head,-1,sizeof(head));
    
    for(int i=1;i<=n;i++)
    {
        cin >> s[i].a >> s[i].b;
        
        add(s[i].a,i);
        add(s[i].b,i);        
    }
    
    
    for(int i=1;i<=n;i++)
    {
        
        vector<int> v;
        
        memset(cnt,0,sizeof(cnt));
        
        v.push_back(i),v.push_back(s[i].a),v.push_back(s[i].b);
        
        for(int j=0;j<=2;j++)
        {
            memset(vis,0,sizeof(vis));
            dfs(v[j],i);
            /*
            for(int k=1;k<=n;k++)
                cout << vis[k] << " ";
            cout << endl;
            */
            for(int k=1;k<=n;k++)
                cnt[k]+=vis[k];
        }
        
        int flag=0;
        for(int j=1;j<=n;j++)
        {
            //cout << cnt[j] << endl;
            if(cnt[j]>=3)
            {
                flag=1;
                break;
            }
        
        }
        if(flag==1)
            cout << "Y";
        else cout << "N";
        
        
    }
    return 0;
}
```

### B-Daily Trips

[题目链接](https://codeforces.com/gym/104252/problem/D)

#### 思路

模拟题,当状态是`下雨`( $Y$)或者`不下雨`( $N$)`且对面没有伞`时,带伞(并**同步更新两边状态**),否则,不带伞.


#### 代码

```c++
// Problem: B - B
// Contest: Virtual Judge - sdtbu选拔赛3
// URL: https://vjudge.net/contest/552058#problem/B
// Memory Limit: 1024 MB
// Time Limit: 500 ms
// Date: 2023-04-09 14:36:07

/**
  * @author  : SDTBU_LY
  * @version : V1.0.0
  * @上联    : ac自动机fail树上dfs序建可持久化线段树
  * @下联    : 后缀自动机的next指针DAG图上求SG函数
**/

#include<iostream>
#include<cstring>
#include<algorithm>

using namespace std;

typedef long long ll;

int n,h,w;

int cnt[2];

int main()
{
    // ios::sync_with_stdio(false);
    // cin.tie(0),cout.tie(0);
    cin >> n >> cnt[0] >> cnt[1];
    

    
    for(int i=1;i<=n;i++)
    {
        string s1,s2;
        cin >> s1 >> s2;
        
        //cout << cnt[0] << " " << cnt[1] << "---" << endl;
        if(s1=="Y"||(s1=="N"&&cnt[1]==0))
        {
            cnt[1]++;
            cnt[0]--;
            cout << "Y" << " ";
        }
        else cout << "N" << " ";
        
        //cout << cnt[0] << " " << cnt[1] << "***" << endl;
        
        if(s2=="Y"||(s2=="N"&&cnt[0]==0))
        {
            cnt[0]++;
            cnt[1]--;
            cout << "Y" << endl;
        }
        else cout << "N" << endl;
        
        //cout << cnt[0] << " " << cnt[1] << "---" << endl;
    }
    return 0;
}
```


### C-Empty Squares

[题目链接](https://codeforces.com/gym/104252/problem/E)

#### 思路

给定长度为 $N$的板,长度为 $K$的已放置,且左边剩余长度为 $E$,问最少剩下多少块未放置.

考虑采用`动态规划`, $f(i,j)$表示左半部分已放置 $i$块,右半放置 $j$块的状态($1$为可放置, $0$为不可放置)

初始化 $f(0,0)=1$, 当 $i\ge len$时, $f(i,j)|=f(i-len,j)$ (当满足即置为 $1$) 且当 $j \ge len$时, $f(i,j)|=f(i,j-len)$,时间复杂度为 $O(n^3)$

由于用的二维时,故应该`从大到小`枚举(与 $01$`背包问题`降低空间复杂度一样)

最后用 $O(n^2)$ 时间复杂度,遍历找出长度最大值 $max\\{i+j+k\\}$.

反向求最小值,结果为 $n- max\\{i+j+k\\}$




#### 代码

```c++
// Problem: C - C
// Contest: Virtual Judge - sdtbu选拔赛3
// URL: https://vjudge.net/contest/552058#problem/C
// Memory Limit: 1024 MB
// Time Limit: 250 ms
// Date: 2023-04-13 20:38:17

/**
  * @author  : SDTBU_LY
  * @version : V1.0.0
  * @上联    : ac自动机fail树上dfs序建可持久化线段树
  * @下联    : 后缀自动机的next指针DAG图上求SG函数
**/

#include<iostream>
#include<cstring>
#include<algorithm>

#define MAXN 1010

using namespace std;

typedef long long ll;

int n,k,e;

int f[MAXN][MAXN];

int res;

int main()
{
    // ios::sync_with_stdio(false);
    // cin.tie(0),cout.tie(0);
    cin >> n >> k >> e;
    int l=e,r=n-k-e;
    
    f[0][0]=1;
    
    for(int len=1;len<=n;len++)
    {
        if(len==k)//跳过
            continue;
        
        for(int i=l;i>=0;i--)
            for(int j=r;j>=0;j--)
            {
                if(i>=len)
                    f[i][j]|=f[i-len][j];
                if(j>=len)
                    f[i][j]|=f[i][j-len];
            }
    }


    for(int i=0;i<=l;i++)
        for(int j=0;j<=r;j++)
            if(f[i][j]==1)
                res=max(res,i+j+k);
    
    cout << n-res << endl;

    return 0;
}
```


### D-Italian Calzone & Pasta Corner

[题目链接](https://codeforces.com/gym/104252/problem/I)

#### 思路

给定一个 $n\times m$的数组 $a$, 每个点 $a_{ij}$可上下左右 $4$个方向移动(移动到的值必须大于当前值),问最多的移动次数.

    4 1 3
    8 5 9
    7 2 6
以样例 $3$解释, 结果为 $6$,可以理解为从 $1$出发,可到达 $[4,8,5,3,9]$,总共 $6$个点;亦可从 $2$出发,可到达 $[7,8,5,6,9]$,总共 $6$个点.

该题类似于[滑雪](http://poj.org/problem?id=1088),但差别在于方向由大到小(其实一样),但是求的是`一条路的最大值`,可参考其`记忆化搜索`实现, $f(i,j)=1+(a_{i+1 j}>a_{ij})\times f(i+1,j)+(a_{i-1 j}>a_{ij})\times f(i-1,j)+$
$(a_{i j+1}>a_{ij}) \times f(i,j+1)+(a_{i j-1}>a_{ij})\times f(i,j-1)$


#### 代码

```c++
// Problem: D - D
// Contest: Virtual Judge - sdtbu选拔赛3
// URL: https://vjudge.net/contest/552058#problem/D
// Memory Limit: 1024 MB
// Time Limit: 5000 ms
// Date: 2023-04-09 15:35:43

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

#define MAXN 110

using namespace std;

typedef long long ll;

int n,m;

int a[MAXN][MAXN];

int dir[4][2]={{-1,0},{0,1},{1,0},{0,-1}};

int f[MAXN][MAXN];
int maxlen;

int vis[MAXN][MAXN];

int dfs(int x,int y)
{
    vis[x][y]=1;
    if(f[x][y]!=-1)
        return f[x][y];
    f[x][y]=1;
    
    for(int i=0;i<=3;i++)
    {
        int dx=x+dir[i][0],dy=y+dir[i][1];
        if(dx>=1&&dx<=n&&dy>=1&&dy<=m&&a[dx][dy]>a[x][y]&&vis[dx][dy]==0)
            f[x][y]+=dfs(dx,dy);
    }
    
    return f[x][y];

}




int main()
{
    // ios::sync_with_stdio(false);
    // cin.tie(0),cout.tie(0);
    cin >> n >> m;
    
    for(int i=1;i<=n;i++)
        for(int j=1;j<=m;j++)
            cin >> a[i][j];
    
    //memset(f,-1,sizeof(f));
    
    for(int i=1;i<=n;i++)
    {
        for(int j=1;j<=m;j++)
        {
            memset(vis,0,sizeof(vis));
            memset(f,-1,sizeof(f));
            maxlen=max(maxlen,dfs(i,j));

            //cout << dfs(i,j) << " ";
        }
        //cout << endl;
    
    }
    cout << maxlen << endl;
    
    return 0;
}
```

### E-Maze in Bolt

[题目链接](https://codeforces.com/gym/104252/problem/I)

#### 思路

未补

#### 代码

```c++

```


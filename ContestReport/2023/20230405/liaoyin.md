### D. Busy As a Bee

[题目链接](https://qoj.ac/problem/5678)

#### 思路

> 已知在 $n\times m$ 的小蜂巢群中,总的`小蜂巢个数cnt`为 $m\times\frac{n+1}{2}+ (m-1)\times \frac{n}{2}$


![QQ图片20230408204704.png](https://cdn.acwing.com/media/article/image/2023/04/08/85276_8a522ffbd6-QQ图片20230408204704.png) 

**证明**:

* 奇数行有 $m$ 个小蜂巢,共有 $\frac{n+1}{2}$ 个奇数行(当 $n$为奇数时,有 $\left \lceil \frac{n}{2} \right \rceil$个奇数行,当 $n$为偶数时,有 $\frac{n}{2}$个奇数行,**综上**为 $\frac{n+1}{2}$个奇数行);
* 偶数行有 $m-1$个小蜂巢,共有 $\frac{n}{2}$个偶数行(当 $n$为奇数时,有 $\left \lfloor \frac{n}{2} \right \rfloor$个偶数行,当 $n$为偶数时,有 $\frac{n}{2}$个偶数行,**综上**为 $\frac{n}{2}$个偶数行(下取整 $\left \lfloor \frac{a}{b} \right \rfloor$操作在执行时同 $\frac{a}{b}$操作)).

`组数cnt'`为:由于每两个一组(单个也算作一组),共有 $\frac{n+1}{2}$(或者 $\frac{n}{2}+ n\bmod 2$)组;

> 求该蜂巢的`边数edge`:(分**奇偶讨论**即可)

* 当 $n$为奇数时,结论是 $\frac{n}{2}\times m + \frac{n+1}{2}\times (6+5\times(m-1))$

![QQ图片20230408210501.png](https://cdn.acwing.com/media/article/image/2023/04/08/85276_005a015cd6-QQ图片20230408210501.png) 

在`奇数行`时,每一个奇数行中,一个小蜂巢是 $6$,每增加一个小蜂巢增加 $5$,故奇数行若有 $k$个小蜂巢,其边数为 $6+5\times (k-1)=5\times k+1$,共有 $\frac{n+1}{2}$个奇数行(此处可参考**上方证明**);
在`偶数行`时,只看其竖边,每个偶数行有 $m$个竖边,共有 $\frac{n}{2}$个偶数行.


* 当 $n$为偶数时,结论是: $\frac{n}{2}\times m + \frac{n+1}{2}\times (6+5\times(m-1))+(m-1)\times 2$

![QQ图片20230408211638.png](https://cdn.acwing.com/media/article/image/2023/04/08/85276_a188762ad6-QQ图片20230408211638.png) 

此处`奇数行`和`偶数行`与上述类似,唯一增加的是底部的边,共有 $m-1$个小蜂巢,每个小蜂巢贡献两条边,即 $(m-1)\times 2$


最终的结果为 $总边数-总组数+1=edge-cnt'+1$(此处 $+1$表示所有组数可用 $1$步完成)

#### 代码

```c++
// Problem: D - Busy As a Bee
// Contest: Virtual Judge - sdtbu选拔赛2
// URL: https://vjudge.net/contest/551762#problem/D
// Memory Limit: 1024 MB
// Time Limit: 1000 ms
// Date: 2023-04-08 19:25:05

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

ll m,n;

ll cnt,edge;

int main()
{
    // ios::sync_with_stdio(false);
    // cin.tie(0),cout.tie(0);
    cin >> m >> n;
    
    if(n%2==1)
        edge=n/2*m+((n+1)/2)*(6+5*(m-1));
    
    else edge=(6+5*(m-1))*((n+1)/2)+(n/2)*m+(m-1)*2;
    
    //cout << edge << endl;
    cnt=m*((n+1)/2)+(m-1)*(n/2);//总蜂房个数
    
    cnt=cnt/2+cnt%2;
    //两个一组(单个一组)
    
    //cout << cnt << endl;
    
    cout << edge-cnt+1 << endl;
    

    return 0;
}
```

### E Caravan Trip Plans

[题目链接](https://qoj.ac/problem/5681)

#### 思路

给定 $n$个点一维坐标 $x_i (1 \le i \le n)$ ,起点从 $0$开始向正方向行进,给定 $n$个点和起点可停留数天,其余点最多停留一天,问在第 $t$天到达 $x_{d}$点的方案数.

令 $f(i,j)$ 表示到达 $i$点且花费 $j$天的方案数;

初始化 $f(0,i)=1$表示无论花费 $j$天停在起点 $0$的方案数为 $1$.

由已知得, $\color{Red}{f(i,j)}$(表示到达 $i$点且花费 $j$天的方案数)必由 $\color{Red}{f(i-1,j-1)}$ (表示到达 $i-1$点且花费 $j-1$天的方案数) 再走一步转移而来;

当到达这 $n$个点中的某个点 $i$,考虑其结果应还有 $\color{Red}{f(i,j-1)}$(表示到达 $i$点且花费 $j-1$天的方案数)转移过来(**原因**: 在该点歇一天)

综上`状态转移方程`为: ![CodeCogsEqn.png](https://cdn.acwing.com/media/article/image/2023/04/12/85276_69171f58d9-CodeCogsEqn.png) 


#### 代码

```c++
// Problem: E - Caravan Trip Plans
// Contest: Virtual Judge - sdtbu选拔赛2
// URL: https://vjudge.net/contest/551762#problem/E
// Memory Limit: 1024 MB
// Time Limit: 1000 ms
// Date: 2023-04-12 15:39:25

/**
  * @author  : SDTBU_LY
  * @version : V1.0.0
  * @上联    : ac自动机fail树上dfs序建可持久化线段树
  * @下联    : 后缀自动机的next指针DAG图上求SG函数
**/

#include<iostream>
#include<cstring>
#include<algorithm>

#define MAXN 25
#define MAXK 70

using namespace std;

typedef long long ll;

int n,m;

int a[MAXN];

int vis[MAXK];

ll f[MAXK][MAXK];//f[i][j]表示到达第i个点,花费j天的方案数

int main()
{
    // ios::sync_with_stdio(false);
    // cin.tie(0),cout.tie(0);
    
    cin >> n >> m;
    
    for(int i=1;i<=n;i++)
    {
        cin >> a[i];
        vis[a[i]]=1;
    }
    for(int i=0;i<=60;i++)
        f[0][i]=1;//无论花费j天,都可以不出发
    
    for(int i=1;i<=60;i++)//枚举每个点
        for(int j=1;j<=60;j++)//枚举天数
        {
            f[i][j]=f[i-1][j-1];//到达i点且花费j天可由到达i-1点花费j-1天再走一步到达
            if(vis[i]==1)
                f[i][j]+=f[i][j-1];//到达i点且花费j天可在到达i点花费j-1天后歇息一天
        }
    
    for(int i=1;i<=m;i++)
    {
        int d,t;
        cin >> d >> t;
        cout << f[a[d]][t] << endl;
        
    }

    return 0;
}
```

### F PSET

[题目链接](https://qoj.ac/contest/1126/problem/5682)

#### 思路

给定 $n$对牌面,每面都有对应的特征(个数形状颜色填充),任选其中三副(正反分别作为一种)组成一套牌,该套需满足相对正反面各属于对应的集合(**要么都相同,要么都不同**);

此处枚举时,将 $(000)_2$和 $(111)_2$对应,将 $(001)_2$和 $(110)_2$对应,将 $(010)_2$和 $(101)_2$对应,将 $(011)_2$和 $(100)_2$对应,正面全部翻过去实际上就是反面,`保证不会被重复计算`.

时间复杂度为 $O(n^3)$


#### 代码

```c++
// Problem: F - PSET
// Contest: Virtual Judge - sdtbu选拔赛2
// URL: https://vjudge.net/contest/551762#problem/F
// Memory Limit: 1024 MB
// Time Limit: 1000 ms
// Date: 2023-04-12 17:19:19

/**
  * @author  : SDTBU_LY
  * @version : V1.0.0
  * @上联    : ac自动机fail树上dfs序建可持久化线段树
  * @下联    : 后缀自动机的next指针DAG图上求SG函数
**/

#include<iostream>
#include<cstring>
#include<algorithm>

#define MAXN 25

using namespace std;

typedef long long ll;

int n;

string s[MAXN][2];

int res;

int check(int num1,int num2,int num3)
{
    for(int i=0;i<=3;i++)
    {
        if(
            (s[num1][0][i]==s[num2][0][i]&&s[num3][0][i]==s[num1][0][i])||
            (s[num1][0][i]!=s[num2][0][i]&&s[num2][0][i]!=s[num3][0][i]&&s[num1][0][i]!=s[num3][0][i])
            
          );
        
        else return 0;
    }
    return 1;
}

int main()
{
    // ios::sync_with_stdio(false);
    // cin.tie(0),cout.tie(0);
    cin >> n;
    
    for(int i=1;i<=n;i++)
        cin >>s[i][0] >> s[i][1];
    
    for(int i=1;i<=n;i++)
        for(int j=i+1;j<=n;j++)
            for(int k=j+1;k<=n;k++)
            {
                swap(s[j][0],s[j][1]);
                
                res+=check(i,j,k);//010||101
                
                swap(s[k][0],s[k][1]);
                
                res+=check(i,j,k);//011||100
                
                swap(s[j][0],s[j][1]);
                
                res+=check(i,j,k);//001||110
                
                swap(s[k][0],s[k][1]);
                    
                res+=check(i,j,k);//000||111
            }

    
    cout << res << endl;
    return 0;
}
```

### G You You See What? 

[题目链接](https://qoj.ac/contest/1126/problem/5680)

#### 思路

根据题意`模拟去环`,环的**起点不删**,优先删除成环后面的字符串即可

#### 代码

```c++
// Problem: G - You You See What?
// Contest: Virtual Judge - sdtbu选拔赛2
// URL: https://vjudge.net/contest/551762#problem/G
// Memory Limit: 1024 MB
// Time Limit: 1000 ms
// Date: 2023-04-12 19:35:16

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

using namespace std;

typedef long long ll;

string s;

char to_low(char c)
{
    if(c>='A'&&c<='Z')
        return char(c+32);
    return c;
}

int check(string s1,string s2)
{
    if(s1.size()!=s2.size())
        return 0;
    
    for(int i=0;i<s1.size();i++)
        if(to_low(s1[i])!=to_low(s2[i]))
            return 0;
    return 1;
}

vector<string> v;

int main()
{
    // ios::sync_with_stdio(false);
    // cin.tie(0),cout.tie(0);
    
    cin >> s;
    
    string temp="";
    for(int i=0;i<s.size();i++)
    {
        if(s[i]=='!')
        {
            v.push_back(temp);
            temp="";
        }
        else temp+=s[i];
    }
    v.push_back(temp);
    
    //cout << s << endl;
    
    while(1)
    {
        int flag=0;
        for(int i=1;i+2<v.size();i++)
        {
            if(check(v[i-1],v[i+1])==1)
            {
                v.erase(v.begin()+i,v.begin()+i+2);
                flag=1;
                break;
            }
            else if(check(v[i-1],v[i])==1)
            {
                v.erase(v.begin()+i);
                flag=1;
                break;
            }
            else if(check(v[i],v[i+1])==1)
            {
                v.erase(v.begin()+i+1);
                flag=1;
                break;
            }
        }
        
        /*
        cout << v[0];
    
        for(int i=1;i<v.size();i++)
            cout << "!" << v[i];
        cout << endl;
        */
        if(flag==0)
            break;
    }
    
    cout << v[0];
    
    for(int i=1;i<v.size();i++)
        cout << "!" << v[i];
    cout << endl;

    return 0;
}
```

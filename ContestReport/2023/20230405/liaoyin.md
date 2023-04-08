### D. Busy As a Bee

[题目链接](https://qoj.ac/problem/5678)

> 已知在$m\times n$的小蜂巢群中,总的`小蜂巢个数cnt`为$m\times\frac{n+1}{2}+ (m-1)\times \frac{n}{2}$


![QQ图片20230408204704.png](https://cdn.acwing.com/media/article/image/2023/04/08/85276_8a522ffbd6-QQ图片20230408204704.png) 

**证明**:

* 奇数行有$m$个小蜂巢,共有$\frac{n+1}{2}$个奇数行(当$n$为奇数时,有$ \left \lceil \frac{n}{2} \right \rceil$个奇数行,当$n$为偶数时,有$\frac{n}{2}$个奇数行,**综上**为$\frac{n+1}{2}$个奇数行);
* 偶数行有$m-1$个小蜂巢,共有$\frac{n}{2}$个偶数行(当$n$为奇数时,有$\left \lfloor \frac{n}{2} \right \rfloor$个偶数行,当$n$为偶数时,有$\frac{n}{2}$个偶数行,**综上**为$\frac{n}{2}$个偶数行(下取整$\left \lfloor \frac{a}{b} \right \rfloor$操作在执行时同$\frac{a}{b}$操作)).

`组数cnt'`为:由于每两个一组(单个也算作一组),共有$\frac{n+1}{2}$(或者$\frac{n}{2}+n\%2$)组;

> 求该蜂巢的`边数edge`:(分**奇偶讨论**即可)

* 当$n$为奇数时,结论是$\frac{n}{2}\times m + \frac{n+1}{2}\times (6+5\times(m-1))$

![QQ图片20230408210501.png](https://cdn.acwing.com/media/article/image/2023/04/08/85276_005a015cd6-QQ图片20230408210501.png) 

在`奇数行`时,每一个奇数行中,一个小蜂巢是$6$,每增加一个小蜂巢增加$5$,故奇数行若有$k$个小蜂巢,其边数为$6+5\times (k-1)=5\times k+1$,共有$\frac{n+1}{2}$个奇数行(此处可参考**上方证明**);
在`偶数行`时,只看其竖边,每个偶数行有$m$个竖边,共有$\frac{n}{2}$个偶数行.


* 当$n$为偶数时,结论是:$\frac{n}{2}\times m + \frac{n+1}{2}\times (6+5\times(m-1))+(m-1)\times 2$

![QQ图片20230408211638.png](https://cdn.acwing.com/media/article/image/2023/04/08/85276_a188762ad6-QQ图片20230408211638.png) 

此处`奇数行`和`偶数行`与上述类似,唯一增加的是底部的边,共有$m-1$个小蜂巢,每个小蜂巢贡献两条边,即$(m-1)\times 2$


最终的结果为 $总边数-总组数+1=edge-cnt'+1$(此处$+1$表示所有组数可用$1$步完成)

```
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

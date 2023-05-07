# 题解

这里记录一下跟我的队友不同的思路

关于b题：

- 首先你要知道((ax+b)-(cx+b))%x==0;
- 所以我们只要找到N关于(b+1)的余数和 各个位数代表的值关于(b+1)的余数，就知道了减去哪个位上的数。
- 关于N的取余可以参考大数求余
- 还有你按照进制位找对应（b+1）的余数会发现一些规律  
如求B=10时：

1. 1%11=1，2%11=2.。。。。。9%11=9
2. 10%11=10，20%11=9.。。。。90%11=2；
3. 100%11就又是1了，所以就这样循环了，不用跑B*L了。

- 因为要最小的M，所以就先从大的数开始找，只要他能大于余数对应的那个值，就可以退出循环了。

```c
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;
int b[1000010];

void sovle()
{
    int bb,t;
    cin>>bb>>t;
    int yu=0;
    for(int i=1;i<=t;i++)
    {
        cin>>b[i];
        yu=((yu*bb)+b[i])%(bb+1);
    }
    if(yu==0)
    {
        cout<<"0 0"<<endl;
        return ;
    }
    int flag=0;
    int i;
    for( i=1;i<=t;i++)
    {
        int wei=t-i;
        if(wei%2==0)
        {
            if(b[i]&&b[i]>=yu)
            {
                flag=yu;
                break;
            }
        }
        else
            if(b[i]&&b[i]>=bb-yu+1)
            {
                flag=bb-yu+1;
                break;
            }
    }
    if(flag)
    {
        cout<<i<<" "<<b[i]-flag<<endl;
    }
    else
    {
        cout<<"-1 -1"<<endl;
    }
}
int main()
{
  ios::sync_with_stdio(false);
  int t=1;
  //cin >> t;
  while (t-- )
  {
    sovle();
  }
  return 0;
}
```

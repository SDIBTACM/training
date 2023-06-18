# 题解周报
## A - Divisibility by 2^n
题意： 给n个数，每个数乘上他所在的位置之后能不能整除2的n次方。

思路：先找出每个1-n的数2的数量，然后再保存1-n的数所能提供2的数量，从大到小排序跟原来相加，是否满足题意
``` 
#include<iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <cmath>
#include <cstdlib>
const int N=5e5+10;
using namespace std;
#define int  long long
int ans;
int a[N];
int cmp(int a,int b)
{
    return a>b;
}
void  sub(int n)
{

    while(1)
    {

        if(n%2==0)
        {
            ans++;
            n/=2;
        }
        else
        {
            break;
        }
    }

}
int  sub2(int n)
{
    int k=0;
    while(1)
    {

        if(n%2==0)
        {
            k++;
            n/=2;
        }
        else
        {
            break;
        }
    }
    return k;

}
void  solve()
{
    int n;
    cin>>n;
     ans=0;
    for(int i=1;i<=n;i++)
    {
        int x;
        cin>>x;
        a[i]=sub2(i);
        sub(x);
    }
    if(ans>=n)
    {
        cout<<0<<endl;
        return ;
    }
   sort(a+1,a+1+n,cmp);
   int res=0;
   for(int i=1;i<=n;i++)
   {
       ans+=a[i];
       res++;
       if(ans>=n)
       {
           cout<<res<<endl;
           return ;
       }
   }
    cout<<-1<<endl;

}
signed main()
{
    int n;
    cin>>n;
    while(n--)
    {
        solve();
    }
    return 0;
}
```
## B - Swap Game
题意：两个人对一堆数进行操作，每个人可以讲2-n的任何一个数减少一并将这个数和第一个数交换，当第一个数为0时，这个人输掉游戏

思路： 如果第一个数时最小的那么第二个人可以每次减少这个数，第一个人一定会输，反之第一个人必胜
```
#include<iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <cmath>
#include <cstdlib>
const int N=1e5+10;
using namespace std;
//#define int  long long
int ans;
int a[N],f[N];
void solve()
{
    int n;
    cin>>n;
    int x;
    cin>>x;
    int mi=1e9;
    for(int i=1;i<n;i++)
    {
        cin>>a[i];
        mi=min(a[i],mi);
    }
    if(mi>=x)
    {
        cout<<"bob"<<endl;
    }
    else
    {
        cout<<"alice"<<endl;
    }
}
int main()
{
    int t;
    cin>>t;
    while(t--)
    {
        solve();
    }

}

```
## C - Permutation Operations
 题意 ：n个数，执行n次，第i次操作对任意位置到n的数增加i，让数组变成非递减数列，输出每次选中的位置
 
 思路：最大的数（最大数小于等于n）要加的最少才最容易实现，所以开一个数组映射一下原数组的位置，从大到小输出即可
```
#include<iostream>
#include<cstdio>
#include<cstring>
using namespace std;
const int N = 2e5 +10;
int a[N], mp[N];
void solve()
{
    int n;
    cin>>n;
    for(int i=1;i<=n;i++)
    {
        cin>>a[i];
        mp[a[i]]=i;
    }
    for(int i=n;i>=1;i--)
    {
        cout<<mp[i]<<" ";
    }
    cout<<endl;
}
int main()
{
	int t;
	cin>>t;
	while(t--)
    {
        solve();
    }

}
```
## D - Make Nonzero Sum (easy version)
题意
一个由1和-1组成的数列，奇数位置相加，偶数位置相减，分段之后，每段和相加是否能为0

思路：首先观察出如果是奇数个数肯定是无法完成题目要求的。
如果相邻两个数相同，就放在同一个组；如果不相同，分别增加两个组让这两个数相互抵消掉
```
#include<iostream>
#include<cstdio>
#include<cstring>
#include<cmath>
using namespace std;
const int N = 2e5+10;
int a[N];
int cnt;
struct p{
    int x;
    int y;
}s[N];
void solve()
{
    memset(s,0,sizeof(s));
    memset(a,0,sizeof(a));
    int n;
    cin>>n;
    for(int i=1;i<=n;i++)
    {
        cin>>a[i];
    }
    if(n%2!=0)
    {
        cout<<-1<<endl;
        return ;
    }
    cnt=0;
    for(int i=1;i<=n;i+=2)
    {
        if(a[i]==a[i+1])
        {
            s[++cnt].x=i;
            s[cnt].y=i+1;
        }
        else{
            s[++cnt].x=i;
            s[cnt].y=i;
            s[++cnt].x=i+1;
            s[cnt].y=i+1;
        }

    } cout<<cnt<<endl;
    for(int i=1;i<=cnt;i++)
    {
        cout<<s[i].x<<" "<<s[i].y;
         cout<<endl;
    }

}
int main()
{
    int t;
    cin>>t;
    while(t--)
    {
        solve();
    }
}
```








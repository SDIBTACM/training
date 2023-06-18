# 本周题解
## A - Prefix Sum Addicts
题意：给一个长度为n的数组和前缀和的最后k项，问有没有一个非递减数组符合题意

思路：先判断所给前缀和是否递减,递减直接输出no；题目中前n-k+1个数最大为是s[2]-s[1]

若（n-k+1）*s[2]-s[1]>=s[1] 则一定可以构造出一个数组
```
#include <bits/stdc++.h>
using namespace std;
const int N=1e5+10;
typedef long long ll;
//#define int long long
ll s[N];
void solve()
{
    int n,k;
    scanf("%d%d",&n,&k);
   // memset(s,0,sizeof(s));
    for(int i=1;i<=k;i++)
    {
       scanf("%lld",&s[i]);
     }
     if(k==1)
    {
       puts("yes");
        return ;
    }
    for(int i=2;i<k;i++)
    {
        if(s[i]-s[i-1]>s[i+1]-s[i])
          {puts("no");
            return;
            }
    }
    ll ans=(s[2]-s[1])*(n-k+1);
      if(ans>=s[1])
    {
        puts("yes");
        return ;
    }
        else
    {
        puts("no");
        return ;
    }

}
signed main()
{
    int t;
    cin>>t;
    while(t--)
    {
        solve();
    }
}
```
## B - Good Subarrays (Easy Version)
题意：给定一个数组a，它的子数组满足b[i]>=i,为good数组，问子数组中有多少good数组

思路：因为b[i]只要大于他所在数组中的下标即符合题意，所以我们定义一个左端点l，如果a[l]>=l-i+1,就让l右移
直到不满足条件，ans+=l-i;

若前边l已经判断符合条件，后边一定符合。因为随着i的增大，l-i+1减小
```
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N=2e5+5;
int a[N];
void solve()
{
    int n;
    cin>>n;
    for(int i=1;i<=n;i++)
    {
        cin>>a[i];
    }
    int l=1;
    ll ans=0;
    for(int i=1;i<=n;i++)
    {
        while(l<=n&&a[l]>=l-i+1)
        {
            l++;
        }
        ans+=l-i;
    }
    cout<<ans<<endl;

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
## D - Removing Smallest Multiples
题意：给一个由零和1组成的字符串，1为需要保留的，0则是需要删除的

一种操作方式，选择一个数字，可以从字符串中删掉该数的最小倍数

思路:从前向后寻找，找到第一个需要删除的数，然后将它和它的倍数可以删除的全部删除，找到第一个无法删除的，跳出循环，重复操作

每次删除需要标记一下已经删除的，不然会重复
```
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N=1e6+5;
int vis[N];
char s[N];
void solve()
{
    int len;
    cin>>len;
    for(int i=1;i<=len;i++)
    {
        cin>>s[i];
    }
    ll  ans=0;
    memset(vis,0,sizeof(vis));
    for(int i=1;i<=len;i++)
    {
        for(int j=i;j<=len;j+=i)
        {
            if(s[j]=='1') break;
            if(!vis[j])
            {
            ans+=i;
            vis[j]=1;
            }

        }
    }
    cout<<ans<<endl;

}
int main()
{
    int n;
    cin>>n;
    while(n--)
    {
        solve();
    }
}
```

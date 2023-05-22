**组队赛第二周**

- 高晨轩
- 郑堂堂
- 付晓龙

**比赛地址**
https://vjudge.csgrandeur.cn/contest/557451
## 题解
### 做出来的
E:给定周长求最大面积，用高中学的均值不等式
```
#include <bits/stdc++.h>
using namespace std;
int main()
{
    int len;
    cin>>len;
    int ans;
    ans=(len/2)*(len/2)/4;
    cout<<ans;
}
```
F：给定一个数，每一位能被这个数整除加一，输出答案
```
#include <bits/stdc++.h>
using namespace std;
char s[9];
int main()
{
    
    cin>>s;
    int ans=0;
    int t=atoi(s);
    for(int i=0;i<strlen(s);i++)
    {
        int a=s[i]-48;
        if(!a)
            continue;
        else
        {
            if(t%a==0)
                ans++;
        }
    }
    cout<<ans<<endl;

}
```
### 补题
H：看了师哥代码，最后看懂一种
+ 将所给种类大米存入数组，然后搜索能够买的所有可能，将他们全部加入队列，并且标记自身价值，最后搜索大于所需购买的最小值即可
```
#include <bits/stdc++.h>
using namespace std;
const int N=1e5+10;
int vis[N];
int a[N*2];
int main()
{
    int n,m;
    cin>>n>>m;
    queue<int> q;
    for(int i=0;i<m;i++)
    {
        cin>>a[i];
        vis[a[i]]=a[i];
        q.push(a[i]);
    }
    sort(a,a+m);
    int maxx=60000;
    while(!q.empty())
    {
        int t=q.front();
        q.pop();
        for(int i=0;i<m;i++)
        {
            int ans=t+a[i];
            if(!vis[ans]&&ans<maxx)
            {
                vis[ans]=ans;
                q.push(ans);
            }
        }
    }
   for(int i=0;i<n;i++)
   {
       int p;
       cin>>p;
       int k=p;
       while(vis[k]==0)
        {
           k++;
        }
       cout<<vis[k]-p<<endl;
   }

}
```

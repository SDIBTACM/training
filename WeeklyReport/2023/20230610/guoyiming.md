# 周报
A题：没想到相同的序列经历奇数和偶数变换时和全不想同的序列的关系，导致了实现的时间复杂度过高。规律没找全就胡乱实现，以后写代码之前一定要把思路想清楚在写。
~~~

#include <bits/stdc++.h>

#define ll long long

using namespace std;
const int maxn=0x3f3f3f3f;

void solve()
{
    int n;
    cin>>n;
    string s1,s2;
    cin>>s1>>s2;
    s1=' '+s1;
    s2=' '+s2;
    int sum=0;
    for(int i=1;i<=n;++i)
        sum+=(s1[i]==s2[i]);
    if(sum!=0&&sum!=n)
    {
        cout<<"NO"<<endl;
        return ;
    }
    vector<pair<int,int>> v;
    bool u=sum==0?0:1;
    int l=1;
    while(l<=n)
    {
        if(s1[l]=='0')
        {
            int tmp=l;
            while(s1[tmp+1]=='0'&&tmp+1<=n)
                tmp++;
            v.push_back({l,tmp});
          //  cout<<l<<"ppp"<<tmp<<endl;
            u^=1;
            l=tmp+1;
        }
        else
            l++;
    }
    if(u==1)
    {
        v.push_back({1,1});
        v.push_back({2,n});
    }
    else
    {
        v.push_back({1,n});
    }
    cout<<"YES"<<endl;
    cout<<v.size()<<endl;
    for(int i=0;i<v.size();++i)
        cout<<v[i].first<<" "<<v[i].second<<endl;
}

int main()
{
    ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
    int t;
    cin>>t;
    while(t--)
        solve();
    return 0;
}
~~~
B题：前缀和，实现出问题，WA了很多发，思路不清晰，和上个题犯的错误一样，边想边实现，浪费了太多时间。
~~~

#include <bits/stdc++.h>

#define ll long long

using namespace std;
const int maxn=0x3f3f3f3f;

ll a[200010];
ll sum[200010];
int pos[200010];

map<ll,int> mp;

void solve()
{
    int n;
    cin>>n;
    int cnt=0,res=0;
    for(int i=1;i<=n;++i)
    {
        cin>>a[i];
        sum[i]=sum[i-1]+a[i];
        if(a[i]==0)
            pos[++cnt]=i;
    }
    pos[++cnt]=n+1;
    for(int i=1;i<pos[1];++i)
        if(sum[i]==0)
            res++;
    for(int i=1;i<cnt;++i)
    {
        mp.clear();
        int ma=0;
        for(int j=pos[i];j<pos[i+1];++j)
        {
            mp[sum[j]]++;
            ma=max(mp[sum[j]],ma);
        }
        res+=ma;
    }
    cout<<res<<endl;
}

int main()
{
    ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
    int t;
    cin>>t;
    while(t--)
        solve();
    return 0;
}
~~~
C题：sort排序的cmp函数还是挺好用的，虽然复杂度还是有点高，但是这个题可以过。

~~~
#include <bits/stdc++.h>

#define ll long long
#define endl '\n'

using namespace std;
const int maxn=0x3f3f3f3f;

int res=0;
int a[200010];

bool cmp(int a,int b)
{
    return (res|a)>(res|b);
}

void solve()
{
    res=0;
    int n;
    cin>>n;
    for(int i=1;i<=n;++i)
        cin>>a[i];
    int cnt=min(30,n);
    for(int i=1;i<=cnt;++i)
    {
        sort(a+i,a+n+1,cmp);
        res|=a[i];
    }
    for(int i=1;i<=n;++i)
    {
        if(i!=1)
            cout<<" ";
        cout<<a[i];
    }
    cout<<endl;
}

int main()
{
    ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
    cout<<fixed;cout.precision(18);
    int t;
    cin>>t;
    while (t--)
    {
        solve();
    }
    return 0;
}
~~~
PS：还是要多交流，多听听其他人的思路，多看看其他人都代码实现。做事情一定要专注。

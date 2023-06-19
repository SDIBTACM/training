# 周报
## 上周
补了一下个人赛的题，然后学英语，四级考试。
### A - awoo's Favorite Problem
```
#include<bits/stdc++.h>

using namespace std;
typedef long long ll;
const ll N=1e5+10;
ll i,nn,q,n;
string s,t,p;

void solve()
{
    int ss[3],tt[3];
    string a,b;
    vector <ll> k1,k2;
	cin>>n;
    cin>>s>>t;
	for(int i=0;i<n;++i)     //将s串中除了b以外的单独存起来
	{
		if(s[i]!='b')
		{
			//a+=s[i];
			a.push_back(s[i]);
			k1.push_back(i);
		}
		if(t[i]!='b')      //将t串中除了b以外的单独存起来
		{
			//b+=t[i];
			b.push_back(t[i]);
			k2.push_back(i);
		}
	}
	if(a!=b)
    {
		cout<<"NO"<<endl;
		return ;
	}
	for(int i=0;i<k1.size();++i)
	{
		if(a[i]=='a'&&k1[i]>k2[i])    //如果s串中的a在的t串后面，则无法转换成功
		{
			cout<<"NO"<<endl;
			return ;
		}
		if(b[i]=='c'&&k1[i]<k2[i])    //如果s串中的c在的t串前面，则无法转换成功
		{
			cout<<"NO"<<endl;
			return ;
		}
	}
	cout<<"YES"<<endl;
}
int main()
{
	cin>>q;
	while(q--)
	{
	    solve();
    }
	return 0;
}
```
### D - Palindromes Coloring
```
#include<iostream>
#include<cstring>
using namespace std;

typedef long long ll;
const ll N=200005;
ll i,n,k,t,ans,pa,od;
char a[N];

void slove()
{
	cin>>n>>k;
	int word[26]={0};
	for(i=0;i<n;i++)
    {
        cin>>a[i];
        word[a[i]-'a']++;
    }
	pa=0;
	od=0;
	ans=0;
	for(i=0;i<26;i++)
	{
		pa+=word[i]/2;   //计算总的回文子串的长度
		od+=word[i]%2;   //计算剩下的字符的数量
	}
	ans=(pa/k)*2;    //计算平均一个字符串的长度
	od+=(pa%k)*2;
	if(od>=k)      //计算除了分好的字符外，剩下的字符是否大于 k，若大于，那么长度可以再加 1
		ans++;
	cout<<ans<<endl;
}
int main()
{
	std::ios::sync_with_stdio(false);
	cin>>t;
	while(t--)
	{
		slove();
	}
	return 0;
}
```
### H - Sending a Sequence Over the Network
```
#include<iostream>
#include<cstring>
using namespace std;

typedef long long ll;

const int N=200005;

int i,n,t,k,m;
int a[N],f[N];

void solve()
{
    cin>>n;
    for(i=1;i<=n;i++)
        cin>>a[i];
    //每一次判断当前位置是否合法
    //如果前面都已经不合法了,后面就一定不合法
    memset(f,0,sizeof f);
    f[0]=1;
    for(i=1;i<=n;i++)
    {
        if(i+a[i]<=n && f[i-1])
            f[ i+a[i] ]=1;         //假设a[i]为在左边的数字
        if(i-a[i]-1>=0 && f[ i-a[i]-1 ])
            f[i]=1;                //假设a[i]为在右边的数字
    }
        if(f[n])
            cout<<"YES"<<endl;
        else
            cout<<"NO"<<endl;
    }

int main()
{
    std::ios::sync_with_stdio(false);
    cin>>t;
    while(t--)
    {
        solve();
    }
}
```
## 下周
主要进行专业课复习，实训和考试安排在一块时间太紧了。尽量多补几道题，先从思维练起。

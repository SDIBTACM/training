# 周报

这个周基本上没做题，一直在搞数据库的实训，结果周五比赛，好菜，连个背包dp都看不出来了，菜到不可理喻，还有一个周省赛了，加油吧

# 补题

### 比赛

## [C - Magical Rearrangement](https://vjudge.net/problem/Gym-103495C)

 题意 ：给出0-9每个数字的使用次数，拼出最小的数字。

要求：全部用完，不能有前导零（0除外），相邻的两个数字不相同。

题解：贪心，开始没想明白什么时候输出最大的，什么时候按照最小的输出，看了看大佬的题解懂了，开始的时候，判断出现次数最多的数字maxn，如果maxn为0则if(maxn>sum/2) 则输出“-1”， 如果maxn不为0，则if(maxn>(sum+1)/2) ....... 特判一下 0这种情况，其他的情况就一定有解，判断剩余的数字中maxn,如果maxn>sum/2则输出maxn所指的数字，否则输出最小的数字，注意相邻两个数字不能相同。

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<ll,ll> pll;
const ll N=2e6+5;
const ll inf=0x3f3f3f3f;
ll a[10];
map<ll,ll> ans; 
signed main(){
    int n;cin>>n;
    while(n--){
    ll maxn=-1,t,sum=0;
    for(int i=0;i<=9;i++){
        cin>>a[i];
        sum+=a[i];
        if(a[i]>maxn){
            maxn=a[i];t=i;
        }
    }
    if(sum==1&&a[0]==1){
        cout<<"0"<<endl;continue;
    }
    if((t!=0&&maxn>(sum+1)/2)||(a[0]>sum/2)){
        cout<<"-1"<<endl;continue;
    }
    ll pre=0;
    for(int i=1;i<=sum;i++){
        ll maxl=-1,tl,suml=0;
        ll pm=-1;
        for(int j=0;j<=9;j++){
            if(a[j]>maxl){
                maxl=a[j];tl=j;
            }
            if(pm==-1&&a[j]>0&&j!=pre) {pm=j,pre=j;}
            suml+=a[j]; 
        }
        if(maxl>suml/2){
          cout<<tl; a[tl]--;pre=tl;//如果选择最大的，那么pre也要是最大的数
        }
        else{
          cout<<pm;a[pm]--;
        }
    }
    cout<<endl;
    }
}  
/*orz*/
```

## [K - Longest Continuous 1](https://vjudge.net/problem/Gym-103495K)

题意： 有一个序列，si就是将0-i的所有数字的二进制去除前导零的形式相加 比如 s0=0 ,s1=01 s2=0110......,求序列的前n个数中的连续一段一的最长长度。

思路，从第三个位置开始，每次更新最大长度都是加上二进制位数*该二进制的最大数，所以，从第三个位置开始，存每次更新的位置，然后二分找位置输出答案即可。

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<ll,ll> pll;
const ll N=2e6+5;
const ll inf=0x3f3f3f3f;
vector<ll> a;
map<ll,ll> ans; 
void solve(){
   ll p=2,q=2,th=3;
   ans[1]=0;ans[2]=1;ans[3]=2;
   a.push_back(1);a.push_back(2);a.push_back(3);
   while(p<=30){
       th+=p*q;
       a.push_back(th);
       ans[th]=p+1;
       p++;q*=2;
   }
   a.push_back(1e9+5);
}
signed main(){
    solve();
    ll t;cin>>t;
    while(t--){
        ll k;cin>>k;
        cout<<ans[*(--upper_bound(a.begin(),a.end(),k))]<<endl;
    }
}  
/*orz*/
复制代码
I - Fake Walsh Transform
题意：在0-2^m-1找出k个数，让他们异或的值为n，找出k的最大值

思路：找规律，当m不为1时，0-2^m-1的所有数异或起来为0,当n为0时 答案为全部数，不为0就要将自身去掉。

复制代码
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<ll,ll> pll;
const ll N=2e6+5;
const ll inf=0x3f3f3f3f;
ll ksm(ll a,ll b){
    ll ans=1;
    while(b){
        if(b&1) ans*=a;
        a=a*a;
        b>>=1;
    }
    return ans;
}
signed main(){
    ll t;cin>>t;
    while(t--){
        ll n,m;cin>>n>>m;
        if(n==1){
            if(m==0) cout<<"1"<<endl;
            else cout<<"2"<<endl;
            continue;
        }
        ll p=ksm(2,n);
        if(m==0) cout<<p<<endl;
        else cout<<p-1<<endl;
    }
}  
/*orz*/
```

## [I - Fake Walsh Transform](https://vjudge.net/problem/Gym-103495I)

题意：在0-2^m-1找出k个数，让他们异或的值为n，找出k的最大值

思路：找规律，当m不为1时，0-2^m-1的所有数异或起来为0,当n为0时 答案为全部数，不为0就要将自身去掉。

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<ll,ll> pll;
const ll N=2e6+5;
const ll inf=0x3f3f3f3f;
ll ksm(ll a,ll b){
    ll ans=1;
    while(b){
        if(b&1) ans*=a;
        a=a*a;
        b>>=1;
    }
    return ans;
}
signed main(){
    ll t;cin>>t;
    while(t--){
        ll n,m;cin>>n>>m;
        if(n==1){
            if(m==0) cout<<"1"<<endl;
            else cout<<"2"<<endl;
            continue;
        }
        ll p=ksm(2,n);
        if(m==0) cout<<p<<endl;
        else cout<<p-1<<endl;
    }
}  
/*orz*/
```


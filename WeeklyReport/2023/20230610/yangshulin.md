# 周报
## 本周
本周做了两道每日一题，还做了之前选拔赛的两道题，练了一下思维，算法方面还需要继续学习练习。
### 每日一题
A Divisibility by 2^n

判断一个数组的乘积能否被2整除，可以进行操作：将数组中的每个数a[i]对应乘i，然后再去判断能否被2整除，输出操作数。此题我通过计算2的个数来判断能否被整除，先找出输入数组中2的个数，再找输入数组的个数1~n中2的个数，然后通过与n中2的个数相比，来计算所需的操作数。
```
#include<iostream>
#include<algorithm>
#include<cmath>
using namespace std;

typedef long long ll;
const ll N=200005;
ll i,ans,t,n,s,s0,s1,s2;
ll a[N],b[N];

ll count2(ll num)   //求输入的每个数中包含的2的个数
{
    ll cnt=0;
    while(num%2==0)
    {
        cnt++;
        num/=2;
    }
    return cnt;
}

bool cmp(ll a,ll b)
{
    return a<b;
}

int main()
{
    cin>>t;
    while(t--)
    {
        ans=0;s1=0;s2=0;
        cin>>n;
        s0=n;
        //m=pow(2,n);        //2的n次幂
        for(i=1;i<=n;i++)   //求输入的数中一共含有2的个数
        {
            cin>>a[i];
            s1+=count2(a[i]);
        }
        for(i=1;i<=n;i++)    //求1到n中含有的2的个数
        {
            b[i]=count2(i);  //每个数所含有的2的个数
            s2+=count2(i);
        }
        sort(b+1,b+1+n,cmp); //将2的个数从大到小排
        if(s0<=s1)           //将输入的n,即2的个数与输入的数中所有的2的个数比较
            ans=0;
        else
            if(s0<=s1+s2)
            {
                s=0;
                ans=0;
                for(i=n;i>=1;i--)
                {
                    ans++;
                    s+=b[i];
                    if(s>=s0-s1)
                        break;
                }
            }
            else
                ans=-1;
        cout<<ans<<endl;
    }
    return 0;
}
```

B Swap Game

本题为博弈问题，当a[1]为0的时候那个人输掉比赛。先把a[2]及以后的数从小到大排序，即a[2]为最小的数，让a[1]与a[2]相比，如果a[1]<=a[2]，则Bob胜利，反之，Alice胜利。
```
#include<iostream>
#include<algorithm>
using namespace std;

typedef long long ll;
const ll N=100005;
ll i,aa,t,n,s;
ll a[N];

bool cmp(ll x,ll y)
{
    return x<y;
}

int main()
{
    cin>>t;
    while(t--)
    {
        s=0;
        cin>>n;
        for(i=1;i<=n;i++)
            cin>>a[i];
        sort(a+2,a+1+n,cmp); //从小到大排，让a[2]每次都最小
        if(a[1]<=a[2])
            cout<<"Bob"<<endl;
        else
            cout<<"Alice"<<endl;
    }
    return 0;
}
```

## 下周
本周由于时间比较紧，没来得及做dp算法练习，下周会尽力多做算法题目训练，加强算法练习，同时多做每日一题，不会的及时补。
## 感想
集训队还是很辛苦的，压力比较大，思维和算法都得多练习，尽量早日赶上训练进度。周日的个人赛做的有点困难，思路想到了但测试数据有的不通过，很头疼，想找到问题在哪。任重道远。。。

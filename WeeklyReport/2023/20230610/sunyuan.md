## 周报
### 本周
这一周先是做了每日一题的B，看着B题短一些。一开始的思路是想着直接前缀和，碰到0若前缀和不是0，就让0赋值成前缀和，结果第五组实例就过不了。
后来思路就是，先找第一个0的位置因，然后找每两个0之间前缀和出现的最多的那个数，把零赋成这个就好。
```cpp
#include <bits/stdc++.h>
#define MAXN 1000010
using namespace std;
typedef long long ll;
ll a[MAXN];
map<ll, ll> ma;
void solve()
{    
    ma.clear();  
    int n;
    cin >> n;
    for (int i = 1; i <= n;++i)
    {
        cin >> a[i];
    }
    ll res = 0;
    int pos =-1;
    ll sum = 0;
    for(int i = 1; i <= n;++i)
    {        
        sum += a[i];
        if(a[i]==0)
        { 
            pos = i;
            break;
         }
         if(sum==0)
            res++;
    }
    if(pos==-1)
    {
        cout <<res<< endl;
        return;
    }
    ll mm = 0;
    for (int i = pos; i <= n;++i)
    {        
        if(a[i]==0)
        {
            res += mm,mm=0;            
            ma.clear();            
            sum = 0;        
        }        
        sum += a[i], ma[sum]++;       
        mm = max(mm, ma[sum]);
    }
    cout << res+mm<<"\n";
}
    
int main()
{    
    ios::sync_with_stdio(0), cin.tie(0), cout.tie(0);    
    int t;    
    cin >> t;    
    while (t--)    
    {        
        solve();   
    }   
    return 0;
}
```
然后就是c题，c题是一个n个数的或运算的前缀和最大，或运算会将所有的二进制位都置为1，因此找n个数中最大的那个补齐所有的1后或前缀和就不会再变了。这个题是暴力找的，
其实最大的ai才10的9次方，所以最外边的循环最多才30多次就够了，所以不会超时。
```cpp
#include <bits/stdc++.h>
#define MAXN 1000010
using namespace std;
typedef long long ll;
int a[MAXN], vis[MAXN];
map<int, int> ma;
int main()
{
    ios::sync_with_stdio(0), cin.tie(0), cout.tie(0);
    int t;
    cin >> t;
    while (t--)
    {
        int n;
        cin >> n;
        int mx = INT_MIN;
        for (int i = 1; i <= n; ++i)
        {
            cin >> a[i];
            if (a[i] > mx)
                mx = a[i];
            ma[a[i]]++;
        }
        cout << mx << ' ';
        ma[mx]--;
        int mm = mx;
        for (int i = 1; i <= 30; ++i)
        {
            int flag = 0;
            for (int i = 1; i <= n; ++i)
            {
                if (ma[a[i]] == 0)
                    continue;
                if ((mx | a[i]) > mm)
                {
                    mm = (mx | a[i]);
                    flag = i;
                }
            }
            if (!flag)
            {
                break;
            }
            else
            {
                cout << a[flag] << " ";
                mx= (mx|a[flag]);
                mm = mx;
                ma[a[flag]]--;
            }
            //cout << mx;
        }
        for (int i = 1; i <= n; ++i)
        {
            if (ma[a[i]] == 0)
                continue;
            ma[a[i]]--;
            cout << a[i] << " ";
        }
        cout << "\n";
    }
    return 0;
}
```
然后就是a题，给两个只有01的字符串，反转成两个只有0的串，无情的找规律，能写好几张纸就看出来了。a的每一个1，反转都会让b变成与他相同或相反交替出现，因此只有a,b完
全相同和完全相反才可以，不完全相同就会是NO。
```cpp
#include <bits/stdc++.h>
#define MAXN 100010

using namespace std;

typedef long long ll;

void solve()
{
    int n;
    string a, b;
    cin >> n >> a >> b;
    int num = 0;
    vector<int> v;
    if (a != b)
    {
        for (int i = 0; i < n; ++i)
        {
            if (a[i] == b[i])
            {
                cout << "NO\n";
                return;
            }
        }
        cout << "YES\n";
        for (int i = 0; i < n; ++i)
        {
            if (a[i] == '1')
            {
                ++num;
                v.push_back(i);
            }
        }
        if(num%2)
            cout << v.size() << "\n";
        else
            cout << v.size() + 3 << "\n";
        for (int i = 0; i < num; ++i)
        {
            cout << v[i] + 1 << ' ' << v[i] + 1 << "\n";
        }
        if (num % 2 == 0)
        {
            cout << 1 << ' ' << n << "\n";
            cout << 1 << ' ' << n - 1 << "\n";
            cout << n << ' ' << n << "\n";
        }
    }
    else
    {
        cout << "YES\n";
        for (int i = 0; i < n; ++i)
        {
            if (a[i] == '0')
            {
                ++num;
                v.push_back(i);
            }
        }
        if(num==0)
        {
            cout << "2\n";
            //cout << 1 << ' ' << n << "\n";
            cout << 1 << ' ' << n - 1 << "\n";
            cout << n << ' ' << n << "\n";
            return;
        }
        else if(num==n)
        {
            cout << 0 << "\n";
            return;
        }
        if(num%2)
        {
            cout << v.size() + 1 << "\n";
        }
        else
        {
            cout << v.size() + 4 << "\n";
        }
        cout << 1 << ' ' << n << "\n";
        for (int i = 0; i < num; ++i)
        {
            cout << v[i] + 1 << ' ' << v[i] + 1 << "\n";
        }
        if (num % 2 == 0)
        {
            cout << 1 << ' ' << n << "\n";
            cout << 1 << ' ' << n - 1 << "\n";
            cout << n << ' ' << n << "\n";
        }
    }
}

int main()
{
    ios::sync_with_stdio(0), cin.tie(0), cout.tie(0);
    int t;
    cin >> t;
    while (t--)
    {
        solve();
    }
    return 0;
}
```
### 下周计划
第一件大事就是期末考试了，真的不想考的太差，所以得复习甚至是新学。然后就是补题和每日一题了，尽量不去搜题解。还有下周就是英语四级了，这个估计得拖到下学期了。

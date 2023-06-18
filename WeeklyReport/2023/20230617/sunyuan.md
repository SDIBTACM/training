## 周报
### 本周
个人赛1补题E，这个题是二分答案，答案找的是最大的参加人数，因此二分答案从1-n,对于每一个（1+n）/2都检查是否能有这(1+n)/2个人同时参加。
```cpp
#include<bits/stdc++.h>

#define MAXN 200010
#define M 10000010

using namespace std;

typedef long long ll;

int a[MAXN],b[MAXN];
int n;

bool check(int x)
{
    int num = 0;
    for (int i = 1; i <= n;++i)
    {
        if(a[i]>=(x-num-1)&&b[i]>=num)
        {
            ++num;
        }
    }
    if(num>=x)
        return 1;
    else
        return 0;
}

void solve()
{
    cin >> n;
    for (int i = 1; i <= n;++i)
    {
        cin >> a[i] >> b[i];
    }
    int l = 1, r = n,res=0;
    while(l<r)
    {
        int mid = (l + r) >> 1;
        if(check(mid))
        {
            l = mid + 1;
            res = max(mid, res);
        }
        else
        {
            r = mid;
        }
    }
    if(check(r))
    {
        res = r;
    }
    cout << res << endl;
}

int main()
{
    ios::sync_with_stdio(0), cin.tie(0), cout.tie(0);
    int t=1;
    cin >> t;
    while(t--)
    {
        solve();
    }
    return 0;
}
```
上周的每日一题D。由-1，0和1，组成的序列，只要-1，1前边有数字那么这个就可以变成相反数，所以只需要数-1，和1，的个数就好了，如果-1，和1的总数是奇数的话怎么变都不会让他们加和为0因此构造不成。当
加和是偶数的时候，就可以让多的那个变成少的那个。这个题的区间是从前到后依次排的。

这个题交上的时候刚过截至时间，真难受。
```cpp
#include <bits/stdc++.h>

#define MAXN 1000010
#define endl '\n'

using namespace std;

typedef long long ll;

int a[MAXN];
map<int, int> ma;
int vis[MAXN];

void solve()
{
    ma.clear();
    //memset(vis, 0, sizeof(vis));
    int n;
    cin >> n;
    int num1 = 0, num = 0;
    for (int i = 1; i <= n;++i)
    {
        cin >> a[i];
        ma[a[i]]++;
    }
    if((ma[-1]+ma[1])%2)
    {
        cout << -1 << endl;
        return;
    }
    if(ma[-1]==ma[1])
    {
        cout << n << endl;
        for (int i = 1; i <= n;++i)
        {
            cout << i << ' ' << i << endl;
        }
    }
    else if(ma[-1]>ma[1])
    {
        int tem = 1;
        cout << n - ((ma[-1] - ma[1]) / 2) << endl;
        for (int i =1; i <n;++i)
        {
            if(a[i+1]==-1&&ma[-1]!=ma[1])
            {
                cout << i << ' ' << i+1 << endl;
                i += 1;
                ma[-1]--, ma[1]++;
                vis[i] = 1, vis[i - 1] = 1;
                tem = i;
            }
            else
                cout << i << ' ' << i << endl;
        }
        if (tem!=n)
            cout << n << ' ' << n << endl;
    }
    else
    {
        int tem = 1;
        cout << n - ((ma[1] - ma[-1]) / 2) << endl;
        for (int i =1; i <n; ++i)
        {
            if (a[i+1] == 1 && ma[-1] != ma[1])
            {
                cout << i<< ' ' << i+1 << endl;
                i += 1;
                ma[1]--, ma[-1]++;
                vis[i] = 1, vis[i - 1] = 1;
                tem = i;
            }
            else 
                cout << i << ' ' << i << endl;
        }
        if (tem!=n)
            cout << n << ' ' << n << endl;
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
每日一题B，这个题真的好难发现规律啊。只知道两个块所在的行和列都能到达，然后就会发现每个一行和一列就会有两个块出现的情况。完了就从中心块一直减2或加2，这样如果中点在这些中就是YES，还有就是在角
上时它无法斜着走，所以特殊处理。开始用循环来减2或加2，结果第7组超时。原来用到(a-x)%x=(a%x-x%x)%x,所以减2或加2求余结果都是相等的。
```cpp
#include <bits/stdc++.h>

#define MAXN 1000010
#define endl '\n'

using namespace std;

typedef long long ll;

void solve()
{
    map<int, int> mm1, mm2;
    int n, cx, cy;
    cin >> n;
    for (int i = 1; i <= 3; ++i)
    {
        int a, b;
        cin >> a >> b;
        mm1[a]++, mm2[b]++;
    }
    int x, y;
    cin >> x >> y;
    for (auto it1 = mm1.begin(), it2 = mm2.begin(); it1 != mm1.end(), it2 != mm2.end(); it1++, it2++)
    {
        if (it1->second == 2)
        {
            cx = it1->first;
        }
        if (it2->second == 2)
        {
            cy = it2->first;
        }
    }
    if ((cx == 1 && cy == n) || (cx == n && cy == 1) || (cx == 1 && cy == 1) || (cx == n && cy == n))
    {
        if (x == cx || y == cy)
            cout << "YES\n";
        else
            cout << "NO\n";
        return;
    }
    if((cx%2==x%2)||(cy%2==y%2))
        cout << "YES\n";
    else
        cout << "NO\n";
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
每日一题E，我开始也是想的二分，二分答案或是二分它的xi结果都无法判断，判断后l,和r该如何赋值。最后看了别人的思路，直接二分最小时间，让这个时间内所有人都能到达位置区间的最小区间，选那个最小位置
就好了。找能否都到达时，也就是区间重合时直接就是找最大的左端点是否小于最大的右端点。二分时间时二分65次就能过。
```cpp
#include<bits/stdc++.h>

#define MAXN 100010
#define ll long long

using namespace std;

double a[MAXN],b[MAXN];
int n;
double res;

bool check(double mid)
{
	double l =-1e18;
	double r = 1e18;
	for (int i = 1; i <= n;++i)
	{
		if(mid<b[i])
		{
			return 0;
		}
		double tem = mid - b[i];
		//cout << tem << endl;
		r = min(r, a[i] +tem);
		l = max(l, a[i] - tem);
	}
	res = l;
	if(l>r)
		return 0;
	else
		return 1;
}

void solve()
{
	cin >> n;
	for (int i = 1; i <= n;++i)
	{
		cin >> a[i];
	}
	for (int i = 1; i <= n;++i)
	{
		cin >> b[i];
	}
	double l = 0, r =1e18;
	for (int i = 1; i <=65;++i)
	{
		double mid = (l + r)/ 2.0;
		if(check(mid))
		{
			r = mid;
		}
		else
			l = mid;
	}
	check(r);
	if(res<0.0)
		res = -res;
	printf("%.1f\n", res);
}

int main()
{
	ios::sync_with_stdio(0), cin.tie(0), cout.tie(0);
	int t = 1;
	cin >> t;
	while(t--)
	{
		solve();
	}
	return 0;
}
```
这周的每日一题D，我的代码不知道为什么在vscode上执行到return 0，前就不执行了，但在cb上和牛客网的编译器上可以，而且交上也能过。不知到是代码的问题还是vscode的问题。我觉的vscode肯定没问题。
```cpp
#include <bits/stdc++.h>

#define MAXN 200010
#define endl '\n'

using namespace std;

typedef long long ll;

map<ll, int> ma, m;
map<ll, ll> mem;

void init()
{
    for (ll i = 1; i <= MAXN; ++i)
    {
        m[i] = 1;
    }
}

void solve()
{
    init();
    int q;
    cin >> q;
    ll mi = 1;
    ma[0] = 1;
    for (int i = 0; i < q; ++i)
    {
        char c;
        ll a;
        cin >> c >> a;
        if (c == '+')
        {
            ma[a] = 1;
            if (a <= MAXN)
                m.erase(a);
        }
        if (c == '?')
        {
            auto it = m.begin();
            if (it->first % a == 0)
            {
                cout << it->first << endl;
            }
            else
            {
                ll tem;
                if (mem[a] != 0)
                {
                    while (ma[mem[a]] == 1)
                    {
                        mem[a] += a;
                    }
                    tem = mem[a];
                }
                else
                {
                    tem = a - (it->first % a);
                    if (tem < 0)
                        tem = -tem;
                    tem += it->first;
                    while (ma[tem] == 1)
                    {
                        tem += a;
                    }
                    mem[a] = tem;
                }
                cout << tem << endl;
            }
        }
    }
}

int main()
{
    //ios::sync_with_stdio(0), cin.tie(0), cout.tie(0);
    int t = 1;
    // cin >> t;
    while (t--)
    {
        solve();
    }
    cout << 1111 << endl;//执行到这里就停了
    return 0;
}
/*
15
+ 1
+ 2
? 1
+ 4
? 2
+ 6
? 3
+ 7
+ 8
? 1
? 2
+ 5
? 1
+ 1000000000000000000
? 1000000000000000000
*/
```
### 下周计划
下周补个人赛2的题，这次也尽量全补完。复习计组，这周复习的指令系统有的题还是有点不会。等待数据库老师的重点。然后再复习数据库。概率论与数理统计也得学个几章。还有c++的实验作业，下周为社么这么多
的计划。

### 本周感想
这一周出了蓝桥杯国赛的成绩，看了一下实验室去打蓝桥杯国赛的只有我是优秀奖，也在我的意料之中，我一共才做了四个题，要是再暴力一个题的话就会是国三了，好遗憾。不过也怪我，把填空题跳过了。当时也不
知道为什么比赛的时候很困，前一天也睡得好早。我只希望期末考试可以好一些吧。下周也没有比赛和每日一题了，就好好复习。

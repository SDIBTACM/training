# [第五次选拔赛](https://vjudge.csgrandeur.cn/contest/554049)

### A
思路：寻找连续的a的个数，遇a进while循环，while内非a跳出后加在答案上即可
```
#include <bits/stdc++.h>
using namespace std;
#define mem(a, b) memset(a, b, sizeof(a));
#define IOS ios::sync_with_stdio(false);cin.tie(0);cout.tie(0); //勿混用
typedef long long ll;
typedef unsigned long long ull;
const int inf = 0x3f3f3f3f;
const int mod = 1e9 + 7;
const int N = 1e5 + 5;
const int M = 1e6 + 5;
//////////////////////////////////////////////////////////////
ll n, m;
void solve()
{
	cin >> n;
	string a;
	cin >> a;
	ll ans = 0;
	for (int i = 0; i < n; ++i)
	{
		if (a[i] == 'a')
		{
			int flog = 0;
			while (a[++i] == 'a') ++ans, flog = 1;
			if (flog) ++ans;
		} 
	}
	cout << ans << endl;
}

int main()
{
	IOS;
	//int T; cin >> T; while (T--)
		solve();

	return 0;
}
```
### C
题意：正方形内，中心有一点，每次可以选择让该点挪到与四个顶点的中点，求该店移动到目标点的步数

已知：顶点x，y轴坐标为0或2的n次方（二进制是1后面n个0），正方形中心点坐标为2的n-1次方（二进制1后面1-n个0）

推导：
- 给中点取名为点a，先只看a的x轴的点xa，xa的二进制是：1后面n-1个0，与正方形顶点只差一个0
- xa每次可以选择取 与0的中点、与2的n次方的中点
- 与0的中点等效为：xa/2，即 xa >> 1
- 与2的n次方的中点等效为：xa 等于（xa + 1后面n个0）再 xa >> 1
- xa的二进制数有n-1位，xa每次除以2的时候看做都不删除前导0，则上面那条可以视为：在 xa 的 最高位前加一个1，然后再 xa >> 1
- 综合上述三条，可以得出：xa每次移动都可以选择以下两种情况：1. 在xa的最高位前加一个0再 xa >> 1，2. 在xa的最高位前加一个1再 xa >> 1

结论：每次操作都会使 xa 最初的那个1 往右 挪一位，并且左边添加一位0/1

附加条件，因为每次1只能往右不能往左，所以：当坐标的其中一个轴到达目标值时，另一个坐标也会到达目标值，否则，因当事轴每次移动距离 h/2，坐标便永远无法第二次到达同一个位置，答案便不存在了

思路：求出目标的x坐标的最后一个1后面有几个0，然后让xa右移到最后那个1的位置，因为高位可以自由选择，那么此时的xa即是答案，即求出xa需要往右挪几位就行了

```
#include <bits/stdc++.h>
using namespace std;
#define mem(a, b) memset(a, b, sizeof(a));
#define IOS ios::sync_with_stdio(false);cin.tie(0);cout.tie(0); //勿混用
typedef long long ll;
typedef unsigned long long ull;
const int inf = 0x3f3f3f3f;
const int mod = 1e9 + 7;
const int N = 1e5 + 5;
const int M = 1e6 + 5;
//////////////////////////////////////////////////////////////
ll n, m;

void solve()
{
	ll x, y;
	cin >> n >> x >> y;
	while ((x & 1) == 0) ++m, x >>= 1; // 求出 x 后面有 m 个 0
	cout << n - 1 - m;
}

int main()
{
	IOS;
	//int T; cin >> T; while (T--)
	solve();

	return 0;
}
```
### D

思路：对于每个高度有几个子弹：开一个一维数组。从前往后遍历，如果该气球的高度没有子弹就射出去一发子弹，然后高度-1、子弹数量+1，最后输出答案

```
#include <bits/stdc++.h>
using namespace std;
#define mem(a, b) memset(a, b, sizeof(a));
#define IOS ios::sync_with_stdio(false);cin.tie(0);cout.tie(0); //勿混用
typedef long long ll;
typedef unsigned long long ull;
const int inf = 0x3f3f3f3f;
const int mod = 1e9 + 7;
const int N = 1e5 + 5;
const int M = 1e6 + 5;
//////////////////////////////////////////////////////////////
ll n, m;
int a[M];

void solve()
{
	cin >> n;
	while (n--)
	{
		int x;
		cin >> x;
		if (a[x])
		{
			--a[x];
			++a[x - 1];
		}
		else
		{
			++a[x - 1];
			++m;
		}
	}
	cout << m;
}

int main()
{
	IOS;
	//int T; cin >> T; while (T--)
	solve();

	return 0;
}
```
### E

思路： 因为只有一个*，对于每个单词都有26种答案，对于每种答案使用mp都存一遍，每个单词的26种答案没有重复的，所以直接输出最多的一种单词就行。

```
#include <bits/stdc++.h>
using namespace std;
#define mem(a, b) memset(a, b, sizeof(a));
#define IOS ios::sync_with_stdio(false);cin.tie(0);cout.tie(0); //勿混用
typedef long long ll;
typedef unsigned long long ull;
const int inf = 0x3f3f3f3f;
const int mod = 1e9 + 7;
const int N = 1e4 + 5;
const int M = 1e6 + 5;
//////////////////////////////////////////////////////////////
ll n, m;
string a[N];
map<string, int> mp;

void solve()
{
	int ans = 0;
	string uo;
	cin >> n >> m;
	for (int i = 1; i <= n; ++i)
	{
		cin >> a[i];
		for (int k = 0; k < a[i].size(); ++k)
		{
			if (a[i][k] == '*')
			{
				for (int ii = 0; ii < 26; ++ii)
				{
					string s = a[i];
					s[k] = 'a' + ii;
					++mp[s];
					int x = mp[s];
					if (x > ans)
					{
						ans = x;
						uo = s;
					}
					else if (x == ans)
					{
						if (s < uo) uo = s;
					}
				}
				break;
			}
		}
	}
	cout << uo << " " << ans;
}

int main()
{
	IOS;
	//int T; cin >> T; while (T--)
	solve();

	return 0;
}
```
### F

思路：输入有9便是F，else S

代码略

### G

思路： 打牌，对于答案的范围：保证答案还有剩余牌的情况下，如果能让对手爆掉23的同时自己不爆掉的情况下，取最小值，自己23但对手不满的情况下，有牌即答案 否则无答案

注意：大于10的牌权值是10

```
#include <bits/stdc++.h>
using namespace std;
#define mem(a, b) memset(a, b, sizeof(a));
#define IOS ios::sync_with_stdio(false);cin.tie(0);cout.tie(0); //勿混用
typedef long long ll;
typedef unsigned long long ull;
const int inf = 0x3f3f3f3f;
const int mod = 1e9 + 7;
const int N = 1e5 + 5;
const int M = 1e6 + 5;
//////////////////////////////////////////////////////////////
ll n, m;
ll siz[123];

void solve()
{
	cin >> n;
	ll a, b, x, y;
	cin >> a >> b >> x >> y;
	++siz[a], ++siz[b], ++siz[x], ++siz[y];
	if (b > 10) b = 10;
	if (a > 10) a = 10;
	if (x > 10) x = 10;
	if (y > 10) y = 10;
	a += b, x += y;
	for (int i = 1; i <= n; ++i)
	{
		ll ans;
		cin >> ans;
		++siz[ans];
		a += (ans >= 10) ? 10 : ans;
		x += (ans >= 10) ? 10 : ans;
	}
	ll p = 24 - a;
	ll c = 23 - x;
	if (p <= 10) while (siz[p] == 4) ++p;
	if (p > 10 && c > 10)
	{
		cout << -1 << endl;
		return;
	}
	if (siz[min(p, c)] == 4) cout << -1 << endl;
	else cout << min(p, c) << endl;
}

int main()
{
	IOS;
	//int T; cin >> T; while (T--)
	solve();

	return 0;
}
```


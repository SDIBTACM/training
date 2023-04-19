# 第四次选拔赛比赛报告
## [比赛链接](https://vjudge.csgrandeur.cn/contest/552923)

## 做出来的题-4题

- A - Math Trade

题目大意：n 条边，输出 n 条边连接起来顶点最多的环的顶点数，其中每个顶点最多一个前驱和一个后继

解题思路： 

前提：因为每个顶点最多一个前驱和一个后继，所以连接后的子图只有三种情况：独立的点、若干顶点组成的线、若干顶点组成的环

做法：由于顶点是字符串第一步用map把字符串换成数字，第二步以某个点为起点顺着路径一直跑，如果跑成环就用顶点数与输出答案比较max，
否则换下一个没遍历过的顶点为起点遍历，重复上述操作直到遍历过所有顶点

代码：
```
#include <bits/stdc++.h>
using namespace std;
#define mem(a, b) memset(a, b, sizeof(a));
#define IOS ios::sync_with_stdio(false);cin.tie(0);cout.tie(0); //勿混用
typedef long long ll;
typedef unsigned long long ull;
const int inf = 0x3f3f3f3f;
const int mod = 1e9 + 7;
const int N = 1e3 + 5;
const int M = 1e3 + 5;
//////////////////////////////////////////////////////////////
ll n, m;
map<string, int> mp;
int a[110][2];

bool vis[M];
int head[M];
int ed[M], nex[M], idx;
void add(int a, int b) {
    ed[idx] = b;
    nex[idx] = head[a];
    head[a] = idx++;
}

int dfs(int x, int fa, int t)
{
    vis[x] = 1;
    for (int i = head[x]; i; i = nex[i])
    {
        int d = ed[i];
        if (d == fa) return t;
        if (vis[d]) continue;
        return dfs(d, fa, t + 1);
    }
    return 0;
}
void solve()
{
    int p = 0;
    cin >> n;
    for (int i = 1; i <= n; ++i)
    {
        string s;
        cin >> s;
        cin >> s;
        if (mp[s] == 0) mp[s] = ++p;
        a[i][0] = mp[s];
        cin >> s;
        if (mp[s] == 0) mp[s] = ++p;
        a[i][1] = mp[s];
    }

    idx = 1;
    for (int i = 1; i <= n; ++i) add(a[i][0], a[i][1]);

    int ans = 0;
    for (int i = 1; i <= p; ++i)
    {
        ans = max(ans, dfs(i, i, 1));
    }

    if (ans) cout << ans;
    else cout << "No trades possible";
}

int main()
{
    IOS; 
    //int T; cin >> T; while (T--)
        solve();

    return 0;
}
```

- B - Over the Hill, Part 1

题目大意： 给一个n行n列的矩阵，和一串字符，输出：根据矩阵将字符串转换后的字符串

解题步骤：1. 第一步将字符串转换数字串
2. 对数字串分成长度为n的小串
3. 把每个小串根据矩阵进行转换
4. 把转换后的字符串合并输出

代码：

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
int u[12][12];
char s[N];
int a[N];
int b[N];
int ans[N];

int work(int t)
{
    int ans = 0;
    for (int i = 0; i < n; ++i) ans += b[i] * u[t][i+1];
    return ans % 37;
}

void solve()
{
    cin >> n;
    for (int i = 1; i <= n; ++i) for (int j = 1; j <= n; ++j) scanf("%d ", &u[i][j]);
    gets(s);

    for (int i = 0; s[i]; ++i)
    {
        if (s[i] == ' ') a[i] = 36;
        else if (s[i] >= '0' && s[i] <= '9') a[i] = s[i] - '0' + 26;
        else a[i] = s[i] - 'A';
    }
    int tp = 0;
    int len = strlen(s);
    for (int k = 1;; ++k)
    {
        int l = (k - 1) * n, r = k * n;
        while (l < r)
        {
            if (l >= len) b[l % n] = 36;
            else b[l % n] = a[l]; 
            ++l;
        } 

        for (int i = 0; i < n; ++i) ans[tp++] = work(i + 1); 
         
        if (r >= len) break;
    }

    for (int i = 0; i < tp; ++i)
    {
        if (ans[i] == 36) s[i] = ' ';
        else if (ans[i] >= 26 && ans[i] <= 35) s[i] = ans[i] + '0' - 26;
        else s[i] = ans[i] + 'A';
    }
    s[tp] = 0;
    puts(s);
}

int main()
{
    //IOS; 
    //int T; cin >> T; while (T--)
        solve();

    return 0;
}
```

- C - A Rank Problem

题目大意： n个点的排名起始是1-n，m次对决进行改变排列顺序，如果位次较后的击败位次较前的则被击败者到胜利者的后一位，中间的全部往前进一位

思路

解题前提：类似于冒泡排序的思想，按从前向后的顺序将邻近的数挨个交换，结果是第一个被挪到最后一位，其他数位置不变
解题步骤：比较对决结果，如果符合改变顺序的情况，就从胜利者一直到失败者一直swap，否则不予处理

代码

```
#include <bits/stdc++.h>
using namespace std;
#define mem(a, b) memset(a, b, sizeof(a));
#define IOS ios::sync_with_stdio(false);cin.tie(0);cout.tie(0); //勿混用
typedef long long ll;
typedef unsigned long long ull;
const int inf = 0x3f3f3f3f;
const int mod = 1e9 + 7;
const int N = 1e3 + 5;
const int M = 1e3 + 5;
//////////////////////////////////////////////////////////////
ll n, m;
int a[110];

int check(int p)
{
    for (int i = 1; i <= n; ++i) if (a[i] == p) return i;
}

void solve()
{
    cin >> n >> m;
    for (int i = 1; i <= n; ++i) a[i] = i;
    while (m--)
    {
        int u = 0, v = 0;
        string aa, b;
        cin >> aa >> b;
        for (int i = 1; aa[i]; ++i) u = u * 10 + aa[i] - '0'; 
        for (int i = 1; b[i]; ++i) v = v * 10 + b[i] - '0';  

        int rk1 = check(u);
        int rk2 = check(v);
        if (rk1 < rk2) continue;
        for (int i = rk2; i < rk1; ++i)
        {
            swap(a[i], a[i + 1]);
        }
    }

    for (int i = 1; i <= n; ++i) cout << 'T' << a[i] << ' '; 
}

int main()
{
    IOS; 
    //int T; cin >> T; while (T--)
        solve();

    return 0;
}
```

- E - Weighty Tomes

题目大意：高楼摔鸡蛋问题，输出最多扔出次数以及第一次可以选择的区间

思路： 依托基本动态规划解决摔鸡蛋问题上，对于第一次可以选择的区间做法是，在第n层遍历时，选出目标答案的最小值后，
记住每个与最小值相等的楼层，最后输出即可

代码

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
ll dp[25][5010];

void solve()
{
	cin >> m >> n;

	mem(dp, 0x3f);
	for (int i = 0; i <= n; ++i) dp[i][0] = 0;

	int a = 0, b = 0;
	for (int i = 1; i <= n; ++i)
		for (int j = 1; j <= m; ++j)
			for (int k = 1; k <= j; ++k)
				dp[i][j] = min(dp[i][j], max(dp[i - 1][k - 1], dp[i][j - k]) + 1);

	for (int k = 1; k <= m; ++k)
	{
		if ((max(dp[n - 1][k - 1], dp[n][m - k]) + 1) == dp[n][m])
		{
			if (a == 0) a = k;
			else b = k;
		}
	}

	cout << dp[n][m] << " ";
	if (b == 0) cout << a;
	else cout << a << '-' << b;
}

int main()
{
	IOS;
	//int T; cin >> T; while (T--)
		solve();

	return 0;
}
```
## 赛后补的题-1题

- D - Simply Sudoku 

题目大意：

单值规则：搜索只有一个可能值可以放在那里的格子

唯一位置规则：在任何行、列或子网格中搜索九个地点中只能放置在其中一个中的值
依据上述两个规则解决出一个9行9列的数独问题

解题思路：

对于第一个规则，选择一个空格子，遍历放置1-9的情况，如果只有一个数可以防止则满足条件。

对于第二个规则：一行格子里，1-9中如果有一个数字只能放在其中一个格子上，则符合条件。
同理，一列格子、一个子网络的格子满足上述情况也符合条件。
注意： 因为满足三者之一即说明此数只能放置在这里，所以行、列或子网格三者满足其一就说明满足条件

代码

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
int a[12][12];
int flog;
int ha[19][19], le[19][19];
int df[19][19];

void work(int i, int j, int p)
{
	a[i][j] = p;
	ha[i][p] = 1;
	le[j][p] = 1;
	df[(i / 3) * 3 + j / 3][p] = 1;
}

int check1()//单值规则
{
	for (int i = 0; i < 9; ++i)
		for (int j = 0; j < 9; ++j)
			if (a[i][j] == 0)
			{
				int cnt = 0, k = 0;
				for (int p = 1; p <= 9; ++p)
					if (ha[i][p] == 0 && le[j][p] == 0 && df[(i / 3) * 3 + j / 3][p] == 0)
						++cnt, k = p;
				if (cnt == 1)
				{
					work(i, j, k);
					return 1;
				}
			}
	return 0;
}

int check2()//唯一位置规则
{
	for (int i = 0; i < 9; ++i)
		for (int j = 0; j < 9; ++j)
		{
			if (a[i][j]) continue;
			for (int p = 1; p <= 9; ++p)
			{
				if (ha[i][p] == 0 && le[j][p] == 0 && df[(i / 3) * 3 + j / 3][p] == 0)
				{
					int flog = 0;
					for (int k = 0; k < 9; ++k)
						if (a[i][k] == 0 && k != j && le[k][p] == 0 && df[(i / 3) * 3 + k / 3][p] == 0)
							flog = 1;
					if (!flog)
					{
						work(i, j, p);
						return 1;
					}
					flog = 0;
					for (int k = 0; k < 9; ++k)
						if (a[k][j] == 0 && k != i && ha[k][p] == 0 && df[(k / 3) * 3 + j / 3][p] == 0)
							flog = 1;

					if (!flog)
					{
						work(i, j, p);
						return 1;
					}
					flog = 0;
					int vu = (i / 3) * 3 + j / 3;
					for (int ki = 0; ki < 9; ++ki)
						for (int kj = 0; kj < 9; ++kj)
						{
							if ((i == ki && j == kj) || a[ki][kj]) continue;
							if ((ki / 3) * 3 + kj / 3 == vu && ha[ki][p] == 0 && le[kj][p] == 0)
								flog = 1;
						}

					if (!flog)
					{
						work(i, j, p);
						return 1;
					}
				}
			}
		}
	return 0;
}

void solve()
{
	for (int i = 0; i < 9; ++i) 
		for (int j = 0; j < 9; ++j)
		{
			int x;
			cin >> x;
			work(i, j, x);
		}

	while (check1()|| check2());

	for (int i = 0; i < 9; ++i)
		for (int j = 0; j < 9; ++j)
			if (a[i][j] == 0) flog = 1;

	flog ? cout << "Not easy" << endl : cout << "Easy" << endl;
	for (int i = 0; i < 9; ++i)
		for (int j = 0; j < 9; ++j)
		{
			a[i][j] ? cout << a[i][j] : cout << '.';
			(j == 8) ? cout << endl : cout << " ";
		}
}

int main()
{
	IOS;
	//int T; cin >> T; while (T--)
		solve();

	return 0;
}
```

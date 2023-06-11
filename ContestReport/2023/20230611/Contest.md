## 21、22级个人赛1（2023.06.11 14:00-18:00） 
## 比赛地址
[比赛地址](https://vjudge.net/contest/562976#overview)

本次比赛出题人：唐梦淇、王传瑞、郭一鸣

# 题解
## A -awoo's Favorite Problem 
### 题意：
给出两个字符串s和t，这两个串中只有字母a、b、c。现在可以在s串中执行以下两种操作：
* 选择s串中的ab将其替换为ba
* 选择s串中的bc将其替换为cb

问通过这两种操作最终能否使字符串s成为t
### 思路：
因为s串中的操作ab可以变为ba，所以发现a是能通过b向后移动的。同理bc可以变为cb，所以c是可以通过b向前移动的。同时因为ac之间没有操作，所以ac在字符串s中的前后顺序不会改变，所以首先将s串中的a和c取出来和t串中a和c的先后顺序和个数进行比较，若顺序和个数相同，则再在s串中的a和t串中的相同a的位置进行比较，因为s串的a可以从前面向后面移动，所以s串中的a可以在t串的前面，同理s串中的c可以在t串的后面
### 代码：
~~~cpp
#include <iostream>
#include <cstring>
#include <algorithm>
#include <string>
#include <map>
#include <functional>
#include <set>
#include <vector>
#define int long long
#define MAXN 2000
#define IOS ios::sync_with_stdio(0);cin.tie(0);cout.tie(0)
using namespace std;

signed main() {
	ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);
	int t;
	string s1, s2;
	cin >> t;
	while (t--) {
		vector<pair<char, int>> vec1, vec2;//进行排序
		string ss1, ss2;//判断两串删除b后是否相等
		int n;
		cin >> n;
		cin >> s1 >> s2;
		for (int i = 0; i < s1.length(); ++i) {
			if (s1[i] != 'b') {
				ss1.push_back(s1[i]);
				vec1.push_back(make_pair(s1[i], i));
			}
		}
		for (int i = 0; i < s2.length(); ++i) {
			if (s2[i] != 'b') {
				ss2.push_back(s2[i]);
				vec2.push_back(make_pair(s2[i], i));
			}
		}
		bool ff = 0;
		if (ss1==ss2) {
			//排序后依次查找s串和t串中相同字母的位置
			sort(vec1.begin(), vec1.end());
			sort(vec2.begin(), vec2.end());
			for (int i = 0;i<ss1.length();++i) {
				if (vec1[i].first=='a'&&vec2[i].first=='a') {
					if (vec1[i].second > vec2[i].second) {
						ff = 1;
						break;
					}
				}
				else if (vec1[i].first == 'c' && vec2[i].first == 'c') {
					if (vec1[i].second < vec2[i].second) {
						ff = 1;
						break;
					}
				}
			}
			if (ff==0) {
				cout << "YES" << endl;
			}
			else {
				cout << "NO" << endl;
			}
		}
		else {
			cout << "NO" << endl;
		}
	}
	return 0;
}
~~~


## B-EKG
### 题意：
一个医院有n个人排了一些队伍，每个人编号从1~n依次排列。一个人只知道站在他前面人的编号，有一些人忘记了自己前面人的编号。现在给出n个人和队伍中一个人的编号pos，要求依次输出编号为pos的人在队伍中所有可能站的位置。（队伍中不可能有环）
### 思路：
因为明确给出队伍不会成环，则发现每个队伍的尽头总会是0，所以利用dfs搜索把每个队伍的长度搜索出来。找的过程中特别记录点pos在所在队伍的位置。因为多个队伍所构成的链的长度组成方式有多种，这刚好与01背包的原理完全相同，所以使用动态规划枚举每一种情况。最终输出即可。该题最终时间复杂度为O(n^2^)
### 代码：
~~~cpp
#include <iostream>
#include <cstring>
#include <algorithm>
#include <map>
#include <functional>
#include <set>
#define int long long
#define MAXN 2000
#define IOS ios::sync_with_stdio(0);cin.tie(0);cout.tie(0)
using namespace std;
int arr[MAXN];
int arr2[MAXN];
bool vis[MAXN];
int flag[MAXN];
int val;
bool ff = 0;
int cnt;
//深搜查找链的长度
int dfs(int n,int pos,int cnt1) {
	//若找到了点pos所在的位置则不需要继续向下寻找，直接记录点pos在这个链到第一个人的长度即可
	if (n == pos) {
		ff = 1;
		val = cnt1 + 1;
		return 0;
	}
	else if (n==0) {
		cnt = cnt1;
		return 0;
	}
	++cnt1;
	dfs(arr[n],pos,cnt1);
	return 0;
}
signed main() {
	ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);
	int n, pos;
	int k = 0;
	int kk = 0;
	cin >> n >> pos;
	for (int i = 1; i <= n; ++i) {
		cin >> k;
		if (k!=0) {//记录第k个人前面的人i
			arr[k] = i;
			vis[i] = 1;
		}
	}//遍历一遍0的链，并找到链的长度。
	for (int i = 1;i<=n;++i) {
		ff = 0;
		if (vis[i]==0) {
			dfs(i, pos, 0);
			if (ff==0) {
				arr2[++kk] = cnt;
			}
		}
	}
	//找到所有链的长度后，找到所有链的组合方式，记录每种组合下链的长度
	//此方法和01背包方法完全相同
	flag[0] = 1;
	for (int i = 1;i<=kk;++i) {//一维空间压缩
		for (int j = n;j>=arr2[i];--j) {
			flag[j] = max(flag[j - arr2[i]], flag[j]);
		}
	}
	for (int i = 0;i<=n;++i) {//遍历所有情况
		if (flag[i]==1) {
			cout << i+val << endl;
		}
	}
	return 0;
}
~~~
## C-Save the Nature
### 题意：
有n张票需要卖出，你可以选择任意数字，做任意序列。
* 第i张能被a整除会拿出x%来捐赠。
* 第i张能被b整除会拿出y%来捐赠。
* 如果这张票的位置能同时被a和b整除则拿出(x+y)%来捐赠。
* 给出票价均是100的倍数，不需要任何舍入。

现在需要找到用最小的门票数量n来至少赚取k数量的钱
### 思路：
因为票的位置可以随意排列，可以选择利用贪心策略来找到用更少的门票数量来赚取更多的钱，但我们不知道如何找到如何使用最小的门票数量来至少赚取k数量的钱，所以需要对每次贪心后得到的钱进行遍历找到最小门票数量，为了降低时间复杂度，因为门票价格是有序的，所以可以使用二分查找。最终时间复杂度是排序+二分的时间复杂度O(nlog^2^n)
### 代码：
~~~cpp
#include <iostream>
#include <cstring>
#include <algorithm>
#include <map>
#include <functional>
#include <set>
#define int long long
#define MAXN 200010
#define IOS ios::sync_with_stdio(0);cin.tie(0);cout.tie(0)
using namespace std;
int arr[MAXN];
int arr2[MAXN];

signed main() {
	ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);
	int n;
	int t;
	cin >> t;
	while (t--) {
		int x, y, a, b, k;
		int ans = -1;
		cin >> n;
		for (int i = 1; i <= n; ++i) {
			cin >> arr[i];
			arr[i] /= 100;//因为题目给出票价是100的倍数，所以这里直接处理
		}
		cin >> x >> a;
		cin >> y >> b;
		cin >> k;
		auto search = [=](int n) {//查找票价最高是多少
			int sum = 0;
			for (int i = 1; i <= n; ++i) {
				arr2[i] = 0;
				if (i % a == 0) {
					arr2[i] += x;
				}
				if (i % b == 0) {
					arr2[i] += y;
				}
			}
			sort(arr2 + 1, arr2 + 1 + n, greater<int>());//从大到小排序
			for (int i = 1; i <= n; ++i) {
				sum += arr[i] * arr2[i];
			}
			return sum >= k;
		};
		sort(arr + 1, arr + 1 + n, greater<int>());
		int l=1, r=n;
		while (l<=r) {
			int mid = (l + r) / 2;
			if (search(mid)==true) {
				ans = mid;
				r = mid - 1;
			}
			else {
				l = mid + 1;
			}
		}
		cout << ans << endl;
	}
	return 0;
}
~~~
## D-Palindromes Coloring
### 题意：
*题意读起来稍微有点迷糊，实际上和涂色一点关系没有，就是看能不能拆成k个回文序列，
要求这些回文序列长度最小值最大
### 思路：
我们先看有多少对字符，再看剩下多少字符。成对的字符除k再乘2就是最小长度，如果所有字符串都能再插一个单独的那最小数再加一。注意，单独的可以是之前不成对的，也可以是除k之后剩下的
### 代码：
~~~cpp
#include<iostream>
using namespace std;
void slove()
{
	int n, k;
	cin >> n >> k;
	char a[200010];
	int zm[26]={0};
	for (int i = 1; i <= n; i++)
	{
		cin >> a[i];
		zm[a[i] - 'a']++;
	}
	int pair = 0, odd = 0;
	for (int i = 0; i < 26; i++)
	{
		pair += zm[i] / 2;
		odd += zm[i] % 2;
	}
	int ans = pair / k * 2;//平均一个字符串的长度
	odd += 2 * (pair % k);
	if (odd >= k)
		ans++;
	cout << ans << endl;
}
int main()
{
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	int t;
	cin >> t;
	while (t--)
	{
		slove();
	}
	return 0;
}
~~~
## E-Keshi Is Throwing a Party
### 题意：
这题是一道二分（二分人数）加贪心（越往后的人越有钱），
意思就是男主有一堆朋友，如果有a个人比他富，或者b个人比他穷他就不来了，看最多来几个人。
### 思路：
二分选择的人数。
然后遍历，带入下列条件：如果比他有钱的小于他的b[i]，同时比他穷的也小于他的a[i]，这个人就可以邀请
### 代码：
~~~cpp
#include<iostream>
using namespace std;
#define maxn 200010
int a[maxn], b[maxn];
int n;
int check(int mid)//假设mid个可以
{
	int cnt = 0;//0个人比他有钱，mid-1-cnt个人比他穷
	for (int i = 1; i <= n; i++)
	{
		if (mid - cnt - 1 <= a[i] && cnt <= b[i])//比他有钱的人数量要小于他能忍受的数
		//同时穷人数也要小于忍受数
			cnt++;
		if (cnt >= mid)
			return 1;//mid小了，可以有更多
	}
	return 0;//
}
void  slove()
{
	cin >> n;
	for (int i = 1; i <= n; i++)
		cin >> a[i] >> b[i];
	int l = 1, r = n, mid;
	while (l < r)
	{
		mid = (l + r+1) / 2;
		if (check(mid))
			l = mid;
		else
			r = mid - 1;
	}
	cout << l << endl;
}
int main()
{
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	int t;
	cin >> t;
	while (t--)
	{
		slove();
	}
	return 0;
}
~~~~~
## F-Easy Assembly
### 题意：
有许多积木，按堆竖直存放，可以进行两个操作：
* 1.分裂：从将一堆积木（数量大于 1）的顶端拿出若干块，按原来的顺序放在地上形成一堆新的积木。操作后积木堆数加一；
* 2.合并：将一堆积木全部按原来的顺序全部堆到另一堆积木的顶端。操作后积木堆数减一。
让所有木块形成一堆且积木上的数字由顶端到底端升序排列。求出最小操作次数。
### 思路：
最后合成的积木的情况是唯一的（因为题中要求了排序），将原来的积木状态与最后的积木状态进行遍历对应，对于某一个积木，如果当前元素大于下一个元素或者下一个元素不是比他大的最小的元素的话，需要进行分裂。最后将所有的积木堆进行合并操作即可。
### 代码
~~~cpp
#include <bits/stdc++.h>

#define ll long long

using namespace std;
const int maxn=0x3f3f3f3f;

vector<int> q[100010];

int a[100010];

map<int,int> mp;

void solve()
{
    int n;
    cin>>n;
    int cnt=0;
    for(int i=0;i<=n-1;++i)
    {
        int len;
        cin>>len;
        for(int j=1;j<=len;++j)
        {
            int x;
            cin>>x;
            q[i].push_back(x);
            a[++cnt]=x;
        }
    }
    sort(a+1,a+cnt+1);
    for(int i=1;i<=cnt;++i)
        mp[a[i]]=i;
    int w=0,u=0;
    for(int i=0;i<=n-1;++i)
    {
        w++;
        for(int j=0;j<q[i].size()-1;++j)
        {
            if(a[mp[q[i][j]]+1]!=q[i][j+1]||q[i][j]>q[i][j+1])
            {
                w++;
                u++;
            }
        }
    }
    cout<<u<<" "<<w-1<<endl;
}

int main()
{
    ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
        solve();
    return 0;
}
~~~
## G-Make It Round
### 题意
给定一个商品的价格，再给一个价格提高到的倍数，求使得商品扩大到多少倍时使得商品价格最后的0最多，如果有多个价格0一样多，输出最大的那一个。如果不可能得到一个后缀0多的价格，输出扩大系数最多时的价格（n*m）。
### 思路
找后缀0，可以分析得出2*5是对后缀0有贡献的。所以先找出有多少个2，有多少个5，然后利用倍数补充没有配好对的2或者5，之后再把10的个数考虑进来，最后利用一个简单的除法即可找到最大的价格。
## 代码
~~~cpp

#include <bits/stdc++.h>

#define ll long long

using namespace std;
const int maxn=0x3f3f3f3f;

void solve()
{
    ll n,m;
    cin>>n>>m;
    ll cnt2=0,cnt5=0;
    ll tmp=n;
    while (tmp%2==0)
    {
        cnt2++;
        tmp>>=1;
    }
    while(tmp%5==0)
    {
        cnt5++;
        tmp/=5;
    }
    ll res=1;
    while(cnt5>cnt2&&m/2)
    {
        cnt5--;
        n<<=1;
        m>>=1;
    }
    while(cnt2>cnt5&&m/5)
    {
        cnt2--;
        n*=5;
        m/=5;
    }
    while(res*10<=m)
    {
        res*=10;
    }
    res=m/res*res;
    cout<<res*n<<endl;
    
}

int main()
{
    ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
    int t;
    cin>>t;
    while (t--)
    {
        solve();
    }
    return 0;
}
~~~
## H-Sending a Sequence Over the Network
### 题意
给定一个数组，判断是否能够满足下列条件：
* 1.分块（连续，完全覆盖）
* 2.对于每块，序列长度被写在左边或右边
### 思路
此题为动态规划题。用dp[i]代表以i结尾的前缀方案是否合法，合法为1，反之为0。
状态转移：
* 如果以第i个数结尾，这个数为a[i],先考虑它是左侧a[i]个数的长度，并且dp[i-a[i]-1]即这a[i]+1个数之前的所有数必须要已经是合法方案了，那么以第i个数结尾的这个数才可以是一个合法方案。
* 考虑右侧a[i]个数的长度，那么说明dp[i-1]需要已经是一个合法方案了，这样右侧的a[i]+1个数才是合法的，即第a[i]+1个数之前都是合法。
### 代码
~~~cpp

#include <bits/stdc++.h>

#define ll long long

using namespace std;
const int maxn=0x3f3f3f3f;

int dp[200010];
int a[200010];

void solve()
{
    int n;
    cin>>n;
    for(int i=1;i<=n;++i)
        cin>>a[i];
    memset(dp,0,sizeof dp);
    dp[0]=1;
    for(int i=1;i<=n;++i)
    {
        if(i-a[i]>=1&&dp[i-a[i]-1])
            dp[i]=1;
        if(i+a[i]<=n&&dp[i-1])
            dp[i+a[i]]=1;
    }
    cout<<(dp[n]?"YES":"NO")<<endl;
}

int main()
{
    ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
    int t;
    cin>>t;
    while (t--)
    {
        solve();
    }
    return 0;
}
~~~
## 比赛排名
| Rank | Team                  | Score | Penalty  |      
|:----:|:---------------------:|:-----:|:--------:|
| 1    | SDTBU_LX(李旭)          | 5     | 10:35:29 |
| 2    | SDTBU_ZLK(康康不想秃头)     | 5     | 10:53:41 |
| 3    | SDTBU_CWL(陈文龙)        | 5     | 11:52:07 |
| 4    | SDTBU_WCK(王成魁)        | 5     | 16:03:36 |
| 5    | jk22214603(栗子旭)       | 4     | 8:47:18  |
| 6    | SDTBU_LTY(刘统誉)        | 3     | 4:05:52  |
| 7    | szyaizd(周东)           | 3     | 6:10:55  |
| 8    | js2021230777(田佳璇)     | 3     | 6:20:50  |
| 9    | SDTBU_GYM(郭一鸣)        | 2     | 4:29:36  |
| 10   | jk21213389(李业昊)       | 2     | 5:27:13  |
| 11   | SDTBU_CH(崔涵)          | 2     | 5:46:53  |
| 12   | SDTBU_LT(林涛)          | 2     | 5:57:53  |
| 13   | SDTBU_ZRT(赵若彤)        | 2     | 6:22:03  |
| 14   | SDTBU_TMQ(唐梦淇)        | 2     | 7:06:34  |
| 15   | SDTBU_SY(孙源)          | 2     | 7:13:15  |
| 16   | SDTBU_CJZ(成嘉泽)        | 1     | 1:14:54  |
| 17   | SDTBU_wzw(王智炜)        | 1     | 1:20:39  |
| 18   | SDTBU_LSS(李双双)        | 1     | 1:54:23  |
| 19   | jk22214600(赵潼)        | 1     | 2:01:07  |
| 20   | wl22214941(郑堂堂)       | 1     | 2:21:03  |
| 21   | SDTBU_GGCX(高晨轩)       | 1     | 2:35:35  |
| 22   | rg2021213527(王传瑞)     | 1     | 2:47:55  |
| 23   | jk22214598(祝增圆)       | 1     | 2:57:03  |
| 24   | 13626422329(SDTBU_HS) | 1     | 3:41:33  |
| 25   | SDTBU_MBY(马宝运)        | 1     | 3:48:29  |
| 26   | wl222121943(付晓龙)      | 1     | 6:08:45  |
| 27   | SDTBU_XYH             | 0     | 0:00:00  |
| 28   | SDTBU_xjw(肖俊伟)        | 0     | 0:00:00  |
| 29   | SDTBU_XJJ(谢俊杰)        | 0     | 0:00:00  |
| 30   | rj2022214683(赵丽萍)     | 0     | 0:00:00  |
| 31   | SDTBU_YSL(杨舒林)        | 0     | 0:00:00  |

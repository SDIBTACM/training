## 周报
### 本周完成
#### 数据库内容
这个其实也可以看作笔记，嘿嘿！！  
1、数据 数据库 数据库管理系统 数据库系统  
2、数据模型——概念模型：面相用户，E-R模型；逻辑模型：面向计算机，包括层次 、网状、关系  
3、数据库模式概念：外模式-局部数据逻辑结构；模式-全部数据逻辑结构；内模式-数据物理结构及其存储方式  
#### 第四次选拔赛
写这个主要是查漏补缺
##### E题
这是一个典型的字典树，就是一堆字符串，问每个字符串与其他字符串的最长公共前缀，  
结果比赛的时候忘了字典树咋写了，用哈希过的，哎！!   
这里附上字典树补题代码  
```C++
#include<map>
#include<queue>
#include<stack>
#include<math.h>
#include<cstdio>
#include<vector>
#include<cstring>
#include<stdio.h>
#include<iostream>
#include<string.h>
#include<algorithm>
#define INF 999999999
#define M 200005
using namespace std;
typedef long long ll;
typedef pair<long long, long long >PII;

int son[500005][26], cnt[500005], idx;
string s[500005];
/*
建立原理
每个节点建立26个子节点位置标记
若没有则空着不管
若有在该子节点位置标记出记录其子节点
子节点位置由idx递增记录，子节点数值由son第一个角标表示
后一个角标表示其子节点位置
*/
void insert(int t, int v)//建立
{
    int p = 0, i, u;
    for (i = 0; i < s[t].size(); i++)
    {
        cnt[p] += v;//当前节点经过几次，即前缀一样的有几个
        u = s[t][i] - 'a';
        if (!son[p][u])
            son[p][u] = ++idx;
        p = son[p][u];
    }
    cnt[p] += v;
}

int query(int t)//查询
{
    int p = 0, ans = 0, i, u;
    for (i = 0; i < s[t].size(); i++)
    {
        u = s[t][i] - 'a';
        //if (!son[p][u]) 
            //return ans;
        p = son[p][u];
        if (cnt[p] == 0)
            return ans;
        ans++;
    }
    return ans;
}

void slove() 
{
    int n, i;
    scanf("%d", &n);
    for (i = 1; i <= n; i++) 
    {
        cin >> s[i];
        insert(i, 1);
    }
    for (i = 1; i <= n; i++)
    {
        insert(i, -1);
        printf("%d\n", query(i));
        insert(i, 1);
    }
}

int main()
{
	int t;
	//scanf("%d", &t);
	//while (t--)
		slove();
	return 0;
}
```
这是原来的哈希做法，时间多了一堆（72——1305）
```C++
#include<map>
#include<queue>
#include<stack>
#include<math.h>
#include<cstdio>
#include<vector>
#include<cstring>
#include<stdio.h>
#include<iostream>
#include<string.h>
#include<algorithm>
#define INF 999999999
#define M 200005
using namespace std;
typedef long long ll;
typedef pair<long long, long long >PII;

map<ll, int> mp1, mp2, mp3;
string x[500005];

void slove()
{
	int n;
	scanf("%d", &n);
	int i, j;
	for (i = 1; i <= n; i++)
	{
		cin >> x[i];
		ll sum1 = 0, sum2 = 0, sum3 = 0;
		for (j = 0; j < x[i].size(); j++)
		{
			sum1 = (sum1 * 26 + x[i][j] - 'a' + 1) % 10000005;
			mp1[sum1]++;
			sum2 = (sum2 * 26 + x[i][j] - 'a' + 1) % 999999997;
			mp2[sum2]++;
			sum3 = (sum3 * 26 + x[i][j] - 'a' + 1) % 99999997;
			mp3[sum3]++;
		}
	}
	for (i = 1; i <= n; i++)
	{
		ll sum1 = 0, sum2 = 0, sum3 = 0;
		for (j = 0; j < x[i].size(); j++)
		{
			sum1 = (sum1 * 26 + x[i][j] - 'a' + 1) % 10000005;
			sum2 = (sum2 * 26 + x[i][j] - 'a' + 1) % 999999997;
			sum3 = (sum3 * 26 + x[i][j] - 'a' + 1) % 99999997;
			if (mp1[sum1] < 2 || mp2[sum2] < 2 || mp3[sum3] < 2)
				break;
		}
		printf("%d\n", j);
	}
}

int main()
{
	int t;
	//scanf("%d", &t);
	//while (t--)
		slove();
	return 0;
}
```
##### C题
这题主要是优化一下代码结构，比赛的时候思路混乱，写的东一下西一下   
思路则是每次比较都有重复的部分，利用其重复部分不多次比较  
只顺着比较一次，逆着比较一次   
代码在这里，俩次思路、时间没有区别，但优化后更好理解   
优化前  
```C++
#include<map>
#include<queue>
#include<stack>
#include<math.h>
#include<cstdio>
#include<vector>
#include<cstring>
#include<stdio.h>
#include<iostream>
#include<string.h>
#include<algorithm>
#define INF 999999999
#define M 200005
using namespace std;
typedef long long ll;
typedef pair<long long, long long >PII;

string s, t;

void slove()
{
	cin >> s >> t;
	int i, j, k;
	int flot1 = 1, flot2 = 1;
	for (j = s.size() - 1, k = t.size() - 1; k >= 0; j--, k--)
		if (s[j] != t[k] && s[j] != '?' && t[k] != '?')
			break;
	if (k >= 0)
	{
		printf("No\n");
		flot1 = 0;
	}
	else
		printf("Yes\n");
	int vis = k;
	for (i = 0; i < t.size(); i++)
	{
		if (i >= k)
			flot1 = 1;
		if (s[i] != t[i] && s[i] != '?' && t[i] != '?')
			flot2 = 0;
		if (flot2 == 1 && flot1 == 1)
			printf("Yes\n");
		else
			printf("No\n");
	}
}

int main()
{
	int t;
	//scanf("%d", &t);
	//while (t--)
		slove();
	return 0;
}
```
优化后   
```C++
#include<map>
#include<queue>
#include<stack>
#include<math.h>
#include<cstdio>
#include<vector>
#include<cstring>
#include<stdio.h>
#include<iostream>
#include<string.h>
#include<algorithm>
#define INF 999999999
#define M 200005
using namespace std;
typedef long long ll;
typedef pair<long long, long long >PII;

string s, t;
int vis1[300005];
int vis2[300005];

void slove()
{
	cin >> s >> t;
	int i, j;
	for (i = 0; i < t.size(); i++)
		if (s[i] == t[i] || s[i] == '?' || t[i] == '?')
			vis1[i] = 1;
		else
			break;
	for (i = s.size() - 1, j = t.size() - 1; j >= 0; i--, j--)
		if (s[i] == t[j] || s[i] == '?' || t[j] == '?')
			vis2[j] = 1;
		else
			break;
	if (vis2[0] == 1)
		printf("Yes\n");
	else
		printf("No\n");
	for (i = 1; i < t.size(); i++)
		if (vis1[i - 1] == 1 && vis2[i] == 1)
			printf("Yes\n");
		else
			printf("No\n");
	if (vis1[t.size() - 1] == 1)
		printf("Yes\n");
	else
		printf("No\n");
}

int main()
{
	int t;
	//scanf("%d", &t);
	//while (t--)
	slove();
	return 0;
}
```
### 下周完成
嗯、没有目标，继续数据库算不算，期待假期中！！！  

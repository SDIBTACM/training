# 选拔赛3翻译
## F - ABCBAC
对于长度为 N 的字符串 S 和整数 i （0≤i≤N），我们将字符串 fi（S）定义为以下项的串联： 

    S的前i个字符，
    S的逆转，以及
    S的最后（N−i） 字符，
按此顺序。例如，如果 S= abc 且 i=2，则 fi（S）= abcbac。

您将获得一个长度为 2N 的字符串 T。求一对长度为 N 的字符串 S 和整数 i （0≤i≤N），使得 fi（S）=T。如果不存在这样的 S 和 i 对，请报告该事实。
## G - Excitation of Atoms
Chanek先生目前正在参加一个在镇上很受欢迎的科学博览会。他在博览会上发现了一个令人兴奋的谜题，并想解决它。
有 N原子编号从 1 到 N。这些原子特别古怪。最初，每个原子都处于正常状态。每个原子都可以处于激发状态。激发原子 i 需要 Di 能量。当原子 i 兴奋时，它会给 Ai能源。您可以激发任意数量的原子（包括零）。这些原子也形成一种特殊的单向键。对于每个 i，（1≤i<N），如果原子i被激发，原子Ei也将被无成本激发。最初，Ei = i+1。请注意，原子 N不能与任何原子形成键。

Chanek 先生必须完全改变 K债券。正好是 K 次，Chanek 先生选择了一个原子 i （1≤i<N），并将 Ei 更改为 i 和当前 Ei 以外的其他值.请注意，原子的键可以保持不变或多次更改。帮助Chanek先生确定他可以达到的最大能量！

注意：您必须首先将 K 完全更改在你开始激发原子之前键合。
# 选拔赛3补题
## D - Happy New Year 2023 
求素数p，q使得 N=p^2*q 
```c++
#include <iostream>
#include <string.h>
#include <algorithm>
#include<stdio.h>
#include<math.h>
using namespace std;
typedef long long ll;

#define MAXN 10000000
ll  primes[MAXN];
int cnt, vis[MAXN];
void get_primes(int n)//线性(欧拉)筛O(n)
{
	for (ll i = 2; i <= n; i++)
	{
		if (vis[i] == 0)
			primes[cnt++] = i;
		for (ll j = 0; primes[j] <= n/i; j++)
		{
			vis[primes[j] * i] = 1;
			if (i % primes[j] == 0)
				break;
		}
	}
}
int main()
{
	int t;
	ll n, a, b;
	get_primes(10000000);
	cin >> t;
	while (t--)
	{
		cin >> n;
		for (int i = 0; i < cnt; i++)
		{
			if (n % primes[i]==0)
			{
				n /= primes[i];
				if (n % primes[i] == 0)
				{
					a = primes[i];
					b = n / primes[i];
				}
				else
				{
					a = sqrt(n);
					b = primes[i];
				}
				printf("%lld %lld\n", a, b);
				break;
			}
		}
	}
	return 0;
}
```
## E - Count Simple Paths
在图中求从顶点1开始的简单路径（没有重复顶点的路径）的数量。打印 min（K，106）。
思路：构建图后进行DFS查找.
注意：当K>10^6的时候，break;
# 感想
选拔赛题目很难，任重道远；英语很重要.

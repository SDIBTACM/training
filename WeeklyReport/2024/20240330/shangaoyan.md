# 周报  
## C题
题意：这个游戏是两个人回合制玩，查克先生打先手，这有一个装着 N个金币的宝箱，当这个宝箱里面没有金币的时候结束，每个回合，玩家可以采取以下行动之一  
1.从宝箱中拿取一个金币2.拿去当前金币数的一半，这个行动只有在宝箱金币数是偶数的时候可用  
每个玩家都将尝试拿最多的金币，查克显示请求你帮他如果他和对手每回合都采取最贪心的策略时它可以得到的最大金币数  
第一个输入一个单个数字T，代表输出测试用例的数量，接下来的T行每个包括一个单个数字N  
## 题解  
双方均采取最贪心的策略->自己拿得最多=对方拿到的少，而第二种行动无疑拿的最多，所以我们要让对方尽可能不能采取第二种行动，即让金币数偶数的情况最小，即使自己拿少一点，也要让对方不能采取第二种行动  
所以每次拿金币前要判断，如果拿一半导致对方也能拿一半，那么自己宁愿拿一个也要使对方不能拿一半  
```c++
#include <stdio.h>
#include <iostream>
using namespace std;
typedef long long ll;
#include <queue>
using namespace std;
int main()
{
	ll n;
	cin >> n;
	while (n--) 
	{
		ll myturn = 1;
		long long  x;
		ll ans = 0;
		ll sum = 0;
		cin >> x;
		while (x)
		{
			if(( x % 2 == 0 && ( x / 2 ) % 2 == 1) || x == 4)
			{
				x /= 2;
				sum = x;
			}
			else
			{
				x -= 1;
				sum=1;
			}
			if (myturn)
			{
				ans += sum;
				myturn = 0;
			}
			else
				myturn = 1;
		}
		cout << ans << endl;
	}
	return 0;
}
```
## D题  
题意：你被给予一个积极数字N，据说N可以被用两个不同的素数p和q表示为N=p^2*q,找到p和q，你有T个测试用例需要解决， 
## 题解  
本题需要先用一个素数筛找到1000000以内的素数（别问为什么，问就是听师哥讲的），在此顺便复习一下欧拉筛  
```cpp
int primes[MAXN],int vis[MAXN],cnt=0;
memset(vis,0,sizeof(vis));
void get_primes(int n)
{
   for(int i=2;i<=n;i++)
     {
        if(vis[i]==0)
           primes[cnt++]=i;
        for(int j=0;primes[j]<=n/i;j++)
             {
                 vis[primes[j] *i]=1;
                 if(i %primes[j] ==0)
                      break;
             }
      }
}
```
然后就是遍历，找到p和q，但是这里有一个细节：需要考虑q可不可以当p
```cpp
if (n % primes[i] == 0)
			{
				if (n / primes[i] % primes[i] != 0)
					printf("%lld %lld\n", (ll)sqrt(n / primes[i]), primes[i]);
				else
					printf("%lld %lld\n", primes[i], n / primes[i] / primes[i]);
				break;
			}
```
## E题  
题意：你被给予一个有着N个从1~N的顶点的无向图和M条从1~M的边，边i连接着顶点ui和vi，每个顶点的度不超过10，输出从顶点1开始的简单路径（没有重复顶点的路径）的数量  
# 本周总结  
复习了一点大学物理，离散还没开始。。  
把欧拉筛看了看，补了几道选拔赛的题  
把这周讲的算法彻底理解了，但是还没有实际编过  
# 下周计划  
把这周讲的算法题尽可能写一写，有点好奇链式前向星到底谁发明的  
把大学物理和离散复习完，马上测试了  
正在思考如何从选拔赛中获得提升，求教

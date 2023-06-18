# 题解周报
## A - Divisibility by 2^n
题意：给你一串长度为n的数，每个数可以变成它乘以它的位置，如a[i]=a[i]*i,问这个操作进行几次可以让它们乘起来整除2的n次方。

思路：一个可以整除2的n次方就是可以除n个2，而一堆数乘起来除2和分开可以除2的次数相同，所以遍历每个数可以除几个2，
并遍历每个位置可以除几个2（记得排序，因为4可以除的比6的多）。

```
#include<stdio.h>
#include<algorithm>
using namespace std;

int s[200005];

bool comp(int x,int y)
{
	return x>y;
}

int main()
{
	int T;
	scanf("%d",&T);
	while(T--)
	{
		memset(s,0,sizeof(s));
		int n;
		scanf("%d",&n);
		int i;
		int x;
		int t=0;
		for(i=1;i<=n;i++)
		{
			scanf("%d",&x);
			while(x%2==0)
			{
				t++;
				x=x/2;
			}
			x=i;
			while(x%2==0)
			{
				s[i]++;
				x=x/2;
			}
		}
		if(t>=n)
			printf("0\n");
		else
		{
			sort(s+1,s+n+1,comp);
			for(i=1;i<=n;i++)
			{
				if(s[i]==0)
				{
					printf("-1\n");
					break;
				}
				t+=s[i];
				if(t>=n)
				{
					printf("%d\n",i);
					break;
				}
			}
		}
	}
	return 0;
}
```
## B - Swap Game
题意：一串数字，俩个人来回操作，将第一个数减1并与任意一个后面的数交换，当此人进行操作时第一个数为0，此人输。

思路：因为每次操作时都必须让第一个数减一，所以每个人操作时都会将后面最小的那个数换到第一，从而让对方先输。所以我们只需要
判断第一个数和后面最小的数那个先减到0。
```
#include<stdio.h>

int main()
{
	int t;
	scanf("%d",&t);
	while(t--)
	{
		int n;
		scanf("%d",&n);
		int x,y;
		int t,i;
		y=0x7fffffff;
		scanf("%d",&x);
		for(i=1;i<n;i++)
		{
			scanf("%d",&t);
			if(t<y)
				y=t;
		}
		if(x>y)
			printf("Alice\n");
		else
			printf("Bob\n");
	}
	return 0;
}
```
## C - Permutation Operations
题意：有一个长度为n的排列，需要进行n次操作第i次操作需要选择一个位置j，使这个位置和这个位置后面的数都+i，
问每次操作选择哪个位置可以使逆序对的数量最少。

思路：逆序对最少就是该排列单调递增，假设我们让1去加n，2去加n-1，n去加1，这样每个数都是相等的，而多加的数都是往后面加，
所以依旧是单调的，也就是将数据按从大到小的顺序依次输出它们的位置就是答案。
```
#include<stdio.h>

int a[200005];

int main()
{
	int t;
	scanf("%d",&t);
	while(t--)
	{
		int n;
		scanf("%d",&n);
		int i,x;
		for(i=1;i<=n;i++)
		{
			scanf("%d",&x);
			a[x]=i;
		}
		for(i=n;i>1;i--)
			printf("%d ",a[i]);
		printf("%d\n",a[1]);
	}
	return 0;
}
```
## D - Make Nonzero Sum (easy version)
题意:给定一个取值只有1，-1的数组,将它分为多个子集，每个子集的结果是a[1]-a[2]+a[3]-....,问如何使这些子集加起来等于0.

思路：分为子集，而子集可以为1，所以将其分1和2的子集最好判断，当子集内俩个数据相等则放到一起，反之，分开放。

```
#include<stdio.h>

int a[200005][2];

int main()
{
	int t;
	scanf("%d",&t);
	while(t--)
	{
		int n;
		scanf("%d",&n);
		int i;
		int x,y;
		if(n%2==1)
		{
			for(i=0;i<n;i++)
				scanf("%d",&x);
			printf("-1\n");
			continue;
		}
		int k=1;
		for(i=1;i<=n;i=i+2)
		{
			scanf("%d%d",&x,&y);
			if(x==y)
			{
				a[k][0]=i;
				a[k][1]=i+1;
				k++;
			}
			else
			{
				a[k][0]=a[k][1]=i;
				k++;
				a[k][0]=a[k][1]=i+1;
				k++;
			}
		}
		printf("%d\n",k-1);
		for(i=1;i<k;i++)
			printf("%d %d\n",a[i][0],a[i][1]);
	}
	return 0;
}
```
## E - Complementary XOR
题意：给定两个01字符串，每次操作选择一个连续区间，第一个字符串这个区间所有的数字取反，
第二个字符串除这个区间外所有的字符取反，问能否使两个字符串都变为0，并给出这些区间。

思路：简单判断后，我们发现如果俩个字符串完全相同或者完全不同，那么就可以，反之不可以。

在考虑可以情况下，方便思考我们先将a字符串全部变为0，这个时候我们发现，

如果b和a相同的时候，此时操作次数为偶，则b此时也全部是0，此时操作次数为奇，则全为1。

如果b和a不同的时候，此时操作次数为偶，则b此时也全部是1，此时操作次数为奇，则全为0。

如果此时b为0，则不管，反之，则进行（1，n），（1，1），（2，n）可以将b也变为0（方法不唯一）。
```
#include<stdio.h>

int a[200005],b[200005];

int main()
{
	int t;
	scanf("%d",&t);
	while(t--)
	{
		int n;
		scanf("%d",&n);
		int i;
		int p=0,q=0;
		int aa=0;
		for(i=1;i<=n;i++)
		{
			scanf("%1d",&a[i]);
			if(a[i]==1)
				aa++;
		}
		for(i=1;i<=n;i++)
		{
			scanf("%1d",&b[i]);
			if(a[i]==b[i])
				p=1;
			else
				q=1;
		}
		if(p==1&&q==1)
		{
			printf("NO\n");
			continue;
		}
		else
		{
			printf("YES\n");
			if(p==1)
			{
				if(aa%2==0)
				{
					printf("%d\n",aa);
					for(i=1;i<=n;i++)
						if(a[i]==1)
							printf("%d %d\n",i,i);
				}
				else
				{
					printf("%d\n",aa+3);
					for(i=1;i<=n;i++)
						if(a[i]==1)
							printf("%d %d\n",i,i);
					printf("1 %d\n1 1\n2 %d\n",n,n);
				}
			}
			if(q==1)
			{
				if(aa%2==1)
				{
					printf("%d\n",aa);
					for(i=1;i<=n;i++)
						if(a[i]==1)
							printf("%d %d\n",i,i);
				}
				else
				{
					printf("%d\n",aa+3);
					for(i=1;i<=n;i++)
						if(a[i]==1)
							printf("%d %d\n",i,i);
					printf("1 %d\n1 1\n2 %d\n",n,n);
				}
			}
		}
	}
	return 0;
}
```
## F - Number Game
题意：有一个序列a，两个人对它进行操作，一共进行k个回合。在第i个回合中，第一个人必须在序列a不为空的情况下删除序列a任意一个小于等于k-i+1的数。
接着，第二个人必须在序列a不为空的情况下将k-i+1加到序列a中任意一个数上。当k个回合结束时，如果第一个人每一回合都操作过了，第一个人就赢了，否则就输了。
求保证让第一个人赢的情况下 k 最大是多少。

思路：第一个人操作时一定是删除当前可以删除的数中最大的，第二个人操作时一定在最小的数上加上这个数，所以我们可以模拟这个过程，看第一个人是否可以赢，
从小到大遍历就可以得到答案，为了减少时间复杂度采用二分（0~a的长度的一半+1）。
```
#include<stdio.h>
#include<algorithm>
using namespace std;

int s[110];
int n;

int f(int x)
{
	int i=0,j=n-1;
	while(x)
	{
		for(j;j>=i;j--)
			if(s[j]<=x)
				break;
		if(j>=i)
		{
			j--;
			i++;
			x--;
		}
		else
			break;
	}
	if(x==0)
		return 1;
	else
		return 0;
}

int main()
{
	int t;
	scanf("%d",&t);
	while(t--)
	{
		scanf("%d",&n);
		int i;
		memset(s,0,sizeof(s));
		for(i=0;i<n;i++)
			scanf("%d",&s[i]);
		sort(s,s+n);
		int r=0,l=(n+1)/2+1;
		int x;
		while(r+1!=l)
		{
			x=(r+l)/2;
			x=f(x);
			if(x==0)
				l=(r+l)/2;
			else
				r=(r+l)/2;
		}
		printf("%d\n",r);
	}
	return 0;
}
```
## G - Scuza
题意：有一个n阶台阶，每一阶比前一阶高a米，问一个一次最多上k米的人可以上多高。

思路：一个人如果可以上n阶台阶，则他一定可以上n-1阶台阶，也就是说他上到n阶台阶的条件是他可以上1~n阶台阶之间最大的差值。
所以我们可以用一个数组将上第n阶台阶需要的最大差值（即k）是多少（就是一个dp)。为了减少时间复杂度可以采用二分+前缀和（将上n阶台阶后的高度存下来）。
```
#include<stdio.h>

int s[200005],p[200005];
long long dp[200005];

int max(int x,int y)
{
	if(x>y)
		return x;
	else
		return y;
}

int main()
{
	int t;
	scanf("%d",&t);
	while(t--)
	{
		int n,q;
		scanf("%d%d",&n,&q);
		int i;
		for(i=1;i<=n;i++)
		{
			scanf("%d",&s[i]);
			dp[i]=dp[i-1]+s[i];
			p[i]=max(p[i-1],s[i]);
		}
		while(q--)
		{
			int k;
			scanf("%d",&k);
			int r=0,l=n+1;
			int x;
			while(r+1!=l)
			{
				x=(l+r)/2;
				if(p[x]<=k)
					r=x;
				else
					l=x;
			}
			printf("%lld",dp[r]);
			if(q!=0)
				printf(" ");
			else
				printf("\n");
		}
	}
	return 0;
}
```

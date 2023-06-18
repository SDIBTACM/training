# 本周题解
## A - Prefix Sum Addicts
题意：有一个含有n个数的数组，现给出其前缀和si的后k个数，问这个数组是否可以成为递增数组。

思路：先根据后k个数的前缀和算出后k-1个数进行判断，当成立时，我们判断前n-k+1个数。
它们每个数最大都是si[2]-si[1],看这些数加起来有没有si[1]大，有就可以，没有就不可以。
因为它们最大都不够第一个前缀和，在大就不符合递增了。
注意特判k=1的时候一定成立，此时没有上限。
```
#include<stdio.h>
#include<iostream>
using namespace std;

long long a[100005];

int main()
{
	int t;
	scanf("%d",&t);
	while (t--)
	{
		int n,k;
		scanf("%d%d",&n,&k);
		int i;
		for(i=1;i<=k;i++)
			scanf("%lld",&a[i]);
		for(i=k;i>1;i--)
			a[i]=a[i]-a[i-1];
		if(k==1)
		{
			printf("YES\n");
			continue;
		}
		for(i=2;i<k;i++)
			if(a[i]>a[i+1])
			{
				printf("NO\n");
				break;
			}
		if(i<k)
			continue;
		else
		{
			long long y;
			y=(n-k+1)*a[2];
			if(y>=a[1])
				printf("YES\n");
			else
				printf("NO\n");
		}
	}
	return 0;
}
```

## B - Good Subarrays (Easy Version)
题意：定义good数组，对于1<=i<=n,有a[i]>=i。现给你一个长度为n的数组a，问a的子数组b有几个是good数组。

思路：定义起点i，终点j属于i~n，aj]>=j-i+1时加一，否则i+1，再次判断。这样会超时，所以我们简化。
每次碰到a[j]<j-i+1时总数加j-i，并在i+1后，a[j]<j-i+1,在加j-i。
即让起点和终点同时循环
```
for(i=1,j=1;j<=n;j++)
while(j<=n&&a[j]<j-i+1)
{
sum+=j-i;
i++;
}
```
最后补充终点一定成立后的所有数。
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
		int i;
		for(i=1;i<=n;i++)
			scanf("%d",&a[i]);
		long long sum=0;
		int j;
		for(i=1,j=1;j<=n;j++)
			while(j<=n&&a[j]<j-i+1)
			{
				sum+=j-i;
				i++;
			}
		while(i<j)
		{
			sum+=j-i;
			i++;
		}
		printf("%lld\n",sum);
	}
	return 0;
}
```
## C - Playing with GCD
题意：给你一个n长度的数组a，问是否可以构造一个长度为n+1的数组b，使得a[i]是b[i]和b[i+1]的最大公因数。

思路：反向思考，因为a[i]是b[i]的因数，a[i-1]也是，所以b[i]是a[i-1]和a[i]的最小公倍数。最后判断题意就好。
```
#include<stdio.h>
#define MAXN 100005

int a[MAXN],b[MAXN];

int gcd(int a,int b) 
{
	int temp;  
	if(a<b)                    
	{
		temp=a;
		a=b;
		b=temp;
	}
	while(b!=0)
	{
		temp=a%b;   
		a=b;
		b=temp;
	}
	return a;   
}

int main()
{
	int t;
	scanf("%d",&t);
	while(t--)
	{
		int n;
		scanf("%d",&n);
		int i;
		for(i=1;i<=n;i++)
			scanf("%d",&a[i]);
		a[0]=1;
		a[n+1]=1;
		for(i=1;i<=n+1;i++)
			b[i]=a[i]*a[i-1]/gcd(a[i],a[i-1]);
		for(i=1;i<=n;i++)
			if(a[i]!=gcd(b[i],b[i+1]))
			{
				printf("NO\n");
				break;
			}
		if(i>n)
			printf("YES\n");
	}
	return 0;
}
```
## D - Removing Smallest Multiples
题意：给由0和1组成的字符串 T，字符串长度为 n，0表示该数字被删去，1表示存在。
定义该字符串是由一种操作得到：每操作一次，就删去一个数字（该数字是字符串中k的最小公倍数。）
将每次操作的k值相加求和的最小值。

思路：遍历就好，从1~n，当第i个数需要删除时，就将它和它的倍数全部删除，
从小到大，碰到第一个不需要删除的数时跳出，再次遍历i。已经删除过的数要标记，
但不能改变它需要删除的条件（后面的数还有用到这个条件）。
```
#include<stdio.h>
#define MAXN 1000005

char s[MAXN];
int ss[MAXN];

int main()
{
	int t;
	scanf("%d",&t);
	while(t--)
	{
		int n;
		scanf("%d",&n);
		s[0]='0';
		scanf("%s",s+1);
		long long sum=0;
		int i,j;
		for(i=0;i<=n;i++)
			ss[i]=0;
		for(i=1;i<=n;i++)
			if(s[i]=='0')
				for(j=i;j<=n;j=j+i)
					if(s[j]=='0')
					{
						if(ss[j]==0)
						{
							sum+=i;
							ss[j]=1;
						}
					}
					else
						break;
		printf("%lld\n",sum);
	}
	return 0;
}
```
## E - Zero-One (Easy Version)
题意：给你俩个长度为n的数组a,b。每次操作可以让俩个数取反，问是否可以经过若干次操作使它们相等。
每次操作时，若俩个数不相邻，代价为y，反之，代价为x，若可以找到最小代价。（x>y)

思路：先判断有几个不相等数，若为奇数个，则不可能。
若为偶数个，判断是否是2个，若不是一定可以选取几次不相邻得到结果。
若是2个，则判断n是否大于2，若不大于，则只能为x。
若大于，则判断这俩个数是否相邻，若不相邻，则为y。
若相邻，则选择x和2y中小的那个。（记得开long long）
```
#include<stdio.h>
#define MAXN 3005

char a[MAXN];
char b[MAXN];

long long min(long long x,long long y)
{
	if(x<y)
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
		int n;
		long long x,y;
		scanf("%d%lld%lld",&n,&x,&y);
		scanf("%s%s",a,b);
		int i;
		int k=0;
		for(i=0;i<n;i++)
			if(a[i]!=b[i])
				k++;
		if(k%2==1)
			printf("-1\n");
		else
		{
			long long sum=0;
			if(k!=2)
				sum=k/2*y;
			else
			{
				if(n>2)
				{
					int f=0;
					for(i=0;i<n-1;i++)
						if(a[i]!=b[i]&&a[i+1]!=b[i+1])
							f=1;
					if(f==1)
						sum=min(x,2*y);
					else
						sum=y;
				}
				else
					sum=x;
			}
			printf("%lld\n",sum);
		}
	}
	return 0;
}
```
## F - Parity Shuffle Sorting
题意：给定一个长度为n的数组，可以操作n次。
每次操作：选择俩个数，若加起来为奇数，则后面的数等于前面的，反之，前面的等于后面的。
问一种方案，使其不递减。

思路：因为它没有限制最小操作几次，所以我们考虑最简单的情况，所有数都相等。
那木我们先让第一个和最后一个相等为a，之后对其他数b依次判断，
若b+a为奇数，选第一个，反之，选最后一个。操作总数为n-1次。
```
#include<stdio.h>
#include<algorithm>
using namespace std;
 
long long a[100005],b[100005];
 
int main()
{
	int t;
	scanf("%d",&t);
	while(t--)
	{
		int n;
		scanf("%d",&n);
		int i;
		for(i=1;i<=n;i++)
			scanf("%lld",&a[i]);
		if(n==1)
		{
			printf("0\n");
			continue;
		}
		if((a[1]+a[n])%2==1)
			a[n]=a[1];
		else	
			a[1]=a[n];
		printf("%d\n",n-1);
		printf("%d %d\n",1,n);
		for(i=2;i<n;i++)
		{
			if((a[1]+a[i])%2==1)
				printf("%d %d\n",1,i);
			else
				printf("%d %d\n",i,n);
		}
	}
	return 0;
}
```

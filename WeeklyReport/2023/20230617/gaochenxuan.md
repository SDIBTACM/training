# 周报  
## A - Prefix Sum Addicts  
  题意：给你一段从第k个到第n个的前缀和，判断原数列是否满足一个递增序列   
  思路：如果k只有一个，那么肯定符合题意，判断第k个到第n个的前缀和是否递增  因为  
  a[1] + a[2] + .... + a[j] <= j * a[j] , 所以s[j+1] <= (j+1) * a[j+1]  
  最后判断s[n-k+1] <= (n-k+1) * a[n-k+1] 是否成立即可   
### 代码  
```
#include<iostream>

using namespace std;

long long a[100005];
int main()
{
	int t;
	cin>>t;
	while(t--)
	{
		int m,n,i;
		cin>>m>>n;
		int idn=1;
		for(i=1;i<=n;i++)
			cin>>a[i];
		for(i=2;i<n;i++)
			if(a[i+1]-a[i]<a[i]-a[i-1])
			{
				idn=0;
				break;
			}
		if((a[2]-a[1])*(m-n+1)<a[1])
			idn=0;
		if(n==1)
			idn=1;
		if(idn)
			cout<<"Yes"<<endl;
		else
			cout<<"No"<<endl;
	}
	return 0;
}
``` 
## C - Playing with GCD   
  题意：有一个长度为n的数组，问是否能构造出一个b数组，其中b[i]和b[i+1]的最大公因数时a[i]  
  思路：a[i]是b[i],b[i+1]的最大公因数，a[i+1]是b[i+1],b[i+2]的最大公因数  
  所以b[i+1]既是a[i]的倍数也是a[i+1]的倍数，构造数组是，只要判断b[i+1],b[i+2]的最大公因数是不是a[i+1]就行  
  一项不是就no    
### 代码  
```  
#include<iostream>
using namespace std;
int gb(int x,int y)
{
	int n=x*y,m=0;
	while(m=x%y)
	{
		x=y;
		y=m;
	}
	return n/y;
}
int gy(int x,int y)
{
	return y==0?x:gy(y,x%y);
}
int main()
{
	int t;
	cin>>t;
	while(t--)
	{
		int n;
		cin>>n;
		int a[n+2];
		for(int i=1;i<=n;i++)
			cin>>a[i];
		int k=1;
		int b,b1,a1;
		for(int i=1;i<n-1;i++)
		{
			b1=gb(a[i],a[i+1]);
			b=gb(a[i+1],a[i+2]);
			a1=gy(b,b1);
			if(a[i+1]!=a1)
			{
				k=0;
				break;
			}
		}
		if(k)
			cout<<"YES"<<endl;
		else
			cout<<"NO"<<endl;
	}
}
```  

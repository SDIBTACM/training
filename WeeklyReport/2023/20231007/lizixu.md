## 本周感想
说实话，唯一的感想就是回家之后直接放开了，一点也不想看题。
最后努力看了几道就直接摆烂了，以后还是多在学校熬一熬吧。

另外一点是关于每日一题的，这个题越写越写得少，可能是盼着放假的心情，也可能是因为其他，后面争取在前几天做完，也不老是保持一天一道了

## 训练题部分补题
没有参加训练赛，把补了的题写一写
### a题
给你一堆数，要第一大的位置和第二大的大小，用sort排一下输出就好。
```
#include<stdio.h>
#include<algorithm>
using namespace std;

int t;
struct node{
	int x;
	int i;
}s[105];

bool comp(struct node a,struct node b)
{
	return a.x>b.x;
}

int main()
{
	int t;
	scanf("%d",&t);
	while(t--)
	{
		int n;
		scanf("%d",&n);
		int j;
		for(j=1;j<=n;j++)
		{
			scanf("%d",&s[j].x);
			s[j].i=j;
		}
		sort(s+1,s+1+n,comp);
		printf("%d %d\n",s[1].i,s[2].x);
	}
	return 0;
}
```
### b题
数学计算题，给灯高H，人高h，墙距离灯的长度D，求最大影子L

我们设人距离灯x，计算后得到L=D+H-(x+D(H-h)/x),（设墙上的影子长度y，墙上影子如果照到地上的长度x1，经过变量替换化简可得）
可以看见函数括号里面是一个对勾函数，最小值是x=D（H-h)/X时，算出x后，判断x的合理性（是否超出D，是否有影子照到了墙上）。
```
#include<stdio.h>
#include<math.h>

int main()
{
	int t;
	scanf("%d",&t);
	while(t--)
	{
		double H,h,D;
		scanf("%lf%lf%lf",&H,&h,&D);
		double x0=D-h*D/H;
		double x=sqrt(D*(H-h));
		if(x>D)
			printf("%.3f\n",h);
		else if(x<x0)
			printf("%.3f\n",h*D/H);
		else
			printf("%.3f\n",D+H-2*x);
	}
	return 0;
}
```
### f题
计算出现了几次关键词，统计就好，注意输入，为了省事直接用cin读入
```
#include<iostream>
#include<algorithm>
#include<string>
using namespace std;

int main()
{
	int t;
	int i,j;
	string str[1000],s;
	cin >> t;
	for(i=0;i<t;i++)
		cin >> str[i];
	int n,m;
	cin >> n;
	while (n--)
	{
		int ans=0;
		cin >> m;
		for(i=0;i<m;i++)
		{
			cin >> s;
			for(j=0;j<t;j++)
				if(s==str[j])
					ans++;
		}
		cout << ans << endl;
	}
	return 0;
}
```
### i题
还是统计，取俩个标记数，暴力判断是否符合条件。
```
#include<iostream>
#include<algorithm>
#include<stdio.h>
using namespace std;

int a[102],b[102];

int main()
{
	int t;
	scanf("%d",&t);
	while(t--)
	{
		int n;
		scanf("%d",&n);
		int i,j;
		for(i=1;i<=n;i++)
			scanf("%d",&a[i]);
		for(i=1;i<=n;i++)
			scanf("%d",&b[i]);
		int p=1,q=1;
		for(i=1,j=1;i<=n;i++,j++)
			if(a[i]!=b[i])
			{
				p=0;
				break;
			}
		for(i=1,j=n;i<=n;i++,j--)
			if(a[i]!=b[j])
			{
				q=0;
				break;
			}
		if(p==0&&q==0)
			printf("neither\n");
		else if(p==0&&q==1)
			printf("stack\n");
		else if(p==1&&q==0)
			printf("queue\n");
		else
			printf("both\n");
	}
	return 0;
}
```
### k题
给一个n*m的矩阵，以及有k个四周加起来的数等于中间那个数的位置，输出一个这样的矩阵

简单来看，我们知道全部为0后，符合条件的个数是（n-2）*（m-2），所以我们一直输出0，直到符合题目所要求的个数后，换位输出其他数字（要一样）。
```
#include<stdio.h>

int main()
{
    int t;
    scanf("%d",&t);
    while(t--)
    {
        int n,m,k;
        scanf("%d%d%d",&n,&m,&k);
		int i,j;
        if(k==(n-2)*(m-2))
            for(i=0;i<n;i++)
            {
                for(j=0;j<m-1;j++)
                    printf("0 ");
                printf("0\n");
            }
        else
        {
            int vis=1;
            int sum=(k/(m-2)+2)*m+k%(m-2)+1;
            for(i=0;i<n;i++)
                for(j=0;j<m;j++)
                {
                    if(vis==0)
                        printf("1");
                    else
					{
						printf("0");
						sum--;
						if(sum==0)
							vis=0;
					}
                    if(j==m-1)
                        printf("\n");
                    else
                        printf(" ");
                }
		}
	}
    return 0;
}
```

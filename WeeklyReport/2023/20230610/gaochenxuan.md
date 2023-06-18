# 周报  
## A - Divisibility by 2^n  
    题意：给你一个n和长度为n的数组，要求他们的乘积%（2^n）==0         
        不足的可以让第i位的数乘以i            
        如果满足：输出最少操作次数              
        如果不满足：输出-1      
          
           
    思路：先看这个数组中有多少个2的因数                
      如果这些因数大于等于n，这个数组的乘积肯定满足，直接输出0         
      如果小于等于n，先排序，优先对i包含2的因数多的位置进行操作   

### 代码  
 ``` 
 #include <iostream>
#include<algorithm>

using namespace std;

int a[200005];
int k[200005];

int main()
{
    int t,n;
    cin>>t;
    while(t--)
    {
        int b;
        int sum1=0,sum2=0;
        cin>>n;
        for(int i=1;i<=n;i++)
        {
            sum2=0;
            cin>>a[i];
            b=a[i];
            while(b%2==0)
            {
                sum1++;
                b/=2;
            }
            b=i;
            while(b%2==0)
            {
                sum2++;
                b/=2;
            }
            k[i]=sum2;
        }

        b=0;
        sort(k+1,k+n+1);
        for(int i=n;i>0;i--)
        {
            if(sum1>=n)
            {
                cout<<b<<endl;
                break;
            }
            sum1+=k[i];
            b++;
        }
        if(sum1<n)
            cout<<-1<<endl;
    }
    return 0;
}  
```  
  
  
 ## B - Swap Game    
     题意: 两个人玩游戏，给n个正整数，两人轮流操作  
       每回合开始时，先将a1-1，再将a1与任意一个数交换  
       当某一个玩家回合开始时，a1==0  则该玩家失败  
       问alice先手且两人都使用最优策略时，谁胜利  
     
     思路：假设数组最小值为x，则当a1==x时，先手必败  
       否则，先手必胜。因此，只需判断a1的大小位置   
 ### 代码  
 ```
 #include<iostream>

using namespace std;

int a[100004];

int main()
{
    int t,n;
    int k;
    cin>>t;
    while(t--)
    {
        k=1;
        cin>>n;
        for(int i=1;i<=n;i++)
         {
             cin>>a[i];
             if(a[i]<a[k])
                k=i;
         }
         if(k==1)
            cout<<"Bob"<<endl;
         else
            cout<<"Alice"<<endl;
    }
    return 0;
}
```

## C - Permutation Operations  
       题意：对于某个长度为n的排列的数组，每一轮i，都为某一后缀的元素+i  
          求每轮操作选择那个位置可以使逆序对最少  
          
        思路：让逆序对少，便让大数加小数，小数加大数  
        让该数列变为一个递增数列，此时逆序对为0；
  ### 代码  
  ```
  
#include<iostream>
 
using namespace std;
int main()
{
	int n,t;
	cin>>t;
	while(t--)
	{
		cin>>n;
		int a[n+1],b[n+1];
		for(int i=1;i<=n;i++)
		{	
			cin>>a[i];
			b[a[i]]=i;
		}
		for(int i=1;i<=n;i++)
		{
			if(b[i]==n)
				cout<<1%n+1<<" ";
			else
				cout<<b[i]+1<<" ";
		}
		cout<<endl;
	}
	return 0;
}
```

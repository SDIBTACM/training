**第一周组队赛**    
* 谢俊杰  
* 赵潼  
* 栗子旭  
    
**E题**    
**题意：** 数字都有颜色，能否通过交换相同颜色的数字使得所以数字升序排列  
**思路：** 输入时用哈希表存好每个数字的颜色，因为如果最终数字能升序排列，数组中数字肯定在对应数组下标，  
因此只要遍历数组，判断下标值对应的颜色与数组该下标存的数字颜色是否一致就可以啦，时间复杂度O(n)  
```ruby
#include <stdio.h>
#include <string.h>
#include <algorithm>
#include <vector>
#include <iostream>
#include <map>
#include <set>
#include <queue>
#include <stack>
#include <stdlib.h>
#include <math.h>
struct ELE
{
    int n,c;
}ele[100100];
int hx[100100];
using namespace std;
int main()
{

    int n,k;
    cin>>n>>k;
    for(int i=1;i!=n+1;i++)
    {
        scanf("%d%d",&ele[i].n,&ele[i].c);
        hx[ele[i].n]=ele[i].c;
    }
    int fl=1;
    for(int i=1;i!=n+1;i++)
    {
        if(ele[i].c!=hx[i])
        {
            fl=0;
            break;
        }
    }
    if(fl)
        cout<<"Y";
    else
        cout<<"N";
    return 0;
}
```

**F题**  
**水题**  
```ruby
#include <stdio.h>
#include <string.h>
#include <algorithm>
#include <vector>
#include <iostream>
#include <map>
#include <set>
#include <queue>
#include <stack>
#include <stdlib.h>
#include <math.h>
struct ELE
{
    int n,c;
}ele[100100];
int hx[100100];
using namespace std;
int main()
{
    int t,d,m;
    cin>>t>>d>>m;
    int s[1010]={0};
    int de[1010]={0};
    s[m+1]=d;
    for(int i=1;i!=m+1;i++)
    {
        cin>>s[i];
    }
    int fl = 0;
    for(int i=1;i!=m+2;i++)
    {
        de[i]=s[i]-s[i-1];
        if(de[i]>=t)
            fl=1;
    }
    if(fl)
        cout<<"Y";
    else
        cout<<"N";
    return 0;
}

```  
  
**C题**    
**题意：** 双头扶梯，来人即动，方向为人去方向，两头不断来人，人看方向不同就等待，方向同就上扶梯，扶梯运行要10秒，问最终结束时间  
**思路：** 这道题可以使用模拟来解决，可以遍历所有人并模拟他们在双向自动扶梯上的行为。  
记录下当前自动扶梯的状态（停止或运动），方向和下一次停止的时间。  
如果扶梯停止，将它打开并运动到该人要去的方向，然后将自动扶梯的下一次停止时间应该更新为当前时间加上 10 秒。  
如果扶梯正在运动，并且该人要去的方向与当前方向相同，那么该人可以立即进入自动扶梯并更新下一次停止时间。否则，该人需要等待自动扶梯停止并更新下一次停止时间。  
最后，我们可以通过添加 10 秒到最后一个人下车的时间来计算自动扶梯的最后停止时间。  
```ruby
#include<stdio.h>
#include<iostream>
using namespace std;
struct ELE
{
    int t,d;
};
int main()
{
    int n;
    cin>>n;
    int st=0;
    ELE e[10010]={0};
    for(int i=1;i!=n+1;i++)
    {
        int x,y;
        scanf("%d%d",&x,&y);
        e[i].t=x;
        if(y==0)
            e[i].d=-1;
        else
            e[i].d=1;
    }
    int t=e[1].t+10,d=e[1].d;
    for(int i=2;i!=n+1;i++)
    {
        if(e[i].t<=t)
        {
            if(e[i].d==d)
            {
                t=e[i].t+10;
            }
            else
            {
                st=e[i].d;
            }
        }
        else
        {
            if(st!=0)
            {
                t=t+10,d=st;
                i--;
                st=0;
            }
            else
            {
                t=e[i].t+10,d=e[i].d;
            }

        }
    }
    if(st!=0)
    {
        t+=10;
    }
    cout<<t;
    return 0;
}

```
**b题**    
**题意：** 给一个n位b进制数，可改动一个位，使其成为最小的可以整除b+1的M  
**思路：** 为了使结果m可以整除b+1，所以我们将其化为同一种进制，而将m化为10进制是比较困难与麻烦的，  
所以我们反向思考，将b+1化为b进制可以得到，所有b+1化完都是1 1，  
而任何可以整除1 1的数，它的奇位和偶位上的数分别加和相等，而且当奇减1，偶就加b；同理偶减1，奇加b。  
（所以采用换算所得到的奇偶加和相等也可以成立）  
所以我们可以分为奇偶相等（直接输出0 0），奇大于偶，偶大于奇  
这里讨论奇大于偶，采用上述奇减1，偶加b后，使其相差不大于一个进制，这里得到的差值就是我们需要某个位上减少的数，  
由于取最小M，所以我们从最高位（即第一位）开始遍历，直到找到位上数比差值大的位输出  
若遍历完依旧未找到输出-1 -1。  
这里提供特殊情况  
1：所给数只有2位，这时候偶不可以减1，然而这时候奇偶和的差值也不会大于一个进制，所以直接使大数变为小数即可，此时符合以上算法  
2：采用奇减1，偶加b后，所得差值刚好等于b，此时奇位上找不到可以减去该差值的位，所以换位找偶位可以减1的位置，依旧从最高位遍历  
```ruby
#include<stdio.h>
#include<iostream>
using namespace std;

int main()
{
    int n,m;
    scanf("%d%d",&n,&m);
    int a[200010];
    int i,b1=0,b2=0;
    for(i=0;i<m;i++)
    {
        scanf("%d",&a[i]);
        if(i%2==0)
            b1+=a[i];
        else
            b2+=a[i];
    }
    if(b1==b2)
    {
        printf("0 0\n");
        return 0;
    }
    if(b1>b2)
    {
        while(b1-b2>n)
        {
            b1-=1;
            b2+=n;
        }
        if(b1==b2)
        {
            printf("0 0\n");
            return 0;
        }
        if(b1>b2)
        {
   if(b1-b2==n)
   {
    for(i=1;i<m;i+=2)
    if(a[i]>=1)
    {
     printf("%d %d\n",i+1,a[i]-1);
     return 0;
    }
   }
   else
   {
    for(i=0;i<m;i+=2)
     if(a[i]>=b1-b2)
     {
      printf("%d %d\n",i+1,a[i]-b1+b2);
      return 0;
     } 
   }
  }
  printf("-1 -1\n");
  return 0;
    }
    if(b2>b1)
    {
        while(b2-b1>n)
        {
            b2-=1;
            b1+=n;
        }
        if(b1==b2)
        {
            printf("0 0\n");
            return 0;
        }
        if(b2>b1)
        {
   if(b2-b1==n)
   {
    for(i=0;i<m;i+=2)
     if(a[i]>=1)
     {
      printf("%d %d",i+1,a[i]-1);
      return 0;
     }  
   }
   else
            {
    for(i=1;i<m;i+=2)
     if(a[i]>b2-b1)
     {
      printf("%d %d\n",i+1,a[i]-b2+b1);
      return 0;
     }
   }
        }
  printf("-1 -1\n");
  return 0;
 }
}
```

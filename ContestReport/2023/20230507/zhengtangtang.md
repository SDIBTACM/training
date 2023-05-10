**组队赛第一周**


-郑堂堂
-高晨轩
-付晓龙

**比赛地址**

https://vjudge.net/contest/556687#overview

**做出题目**

E
**题意**：输入地的序号和的颜色，交换相同颜色地的序号，判断能否使所有地的序号为升序
**思路**：根据地的颜色，存入地的序号和输入的序号，遍历判断输入的序号是否全部存在于地的序号集合中，遍历过程中，如果存在，弹出该地的序号以减少时间复杂度
```c++
#include<string.h>
#include<iostream>
#include<stdio.h>
#include<math.h>
#include<stdlib.h>
#include<vector>
using namespace std;
int b[7]={1,2,2,4,5,6,7};
vector<int> nums[100005];
vector<int> num1[100005];
int main()
 {
     int n,m;
     cin>>n>>m;
     for(int i=1;i<=n;i++)
     {
         int a,b;
         cin>>a>>b;
         nums[b].push_back(a);
         num1[b].push_back(i);
     }

    for(int k=1;k<=m;k++)
     for(int i=0;i<num1[k].size();i++)
        {
            int x=0;


            for (vector<int>::iterator it = nums[k].begin(); it != nums[k].end();) {
    if (*it == num1[k][i]) {
        it = nums[k].erase(it);
        x=1;
    } else {
        ++it;
    }
}


            if(x==0)
    {
        printf("N\n");
        return 0;
    }


        }


      printf("Y\n");
 return 0;
}

```


F
**题意**：注意两个点就行
**思路**：y必须是吃完饭并且睡够
```
#include<cstdlib>
#include<iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <cmath>
#include <cstdlib>
#include<queue>
using namespace std;
#define ll unsigned long long
int main()
{
   int t,d,m;
   cin>>t>>d>>m;
   int b;
   int a=0;
   int flag=0;
   if(m==0&&t<=d)
   {
       cout<<"Y";
       return 0;
   }
    if(t>d)
   {

       cout<<"N";
       return 0;
   }
   while(m--)
   {
       cin>>b;
       int k=b-a;
       a=b;
       if(k>=t)
       {
           flag=1;
       }

   }
   if(d-b>=t)
   {
       flag=1;
   }
   printf("%s",flag==1?"Y":"N");





}
```
补题 b

**题意**：提供一个数字b，和m，缩小m某个数位的值，如果存在m%（b+1）==0，输出m的最小值，否则输出0，0

**思路**：原思路是字符串换成数字进行运算，然后从高位往地位遍历，逐渐递增逐渐找到符合条件的m，结果在test 3就wa了，可能是数字位数改变的原因吧，，
学习了师哥的新思路，1.首先是求余数的算法，res=(res*b%(b+1)+a[i])%(b+1),以前看李师哥好像也用过，现在印象更深刻了。2.然后需要倒序对数位标记，虽然不知道为什么，但是确实必须这样做3.根据标记进行大小判断，比res大或者比b+1-res大就行，然后就能找到答案了，也是挺神奇的

师哥ac代码
```
#include<bits/stdc++.h>
using namespace std;
#define endl '\n'
const int maxn =2e5+5;
int B,L;int vis[maxn],a[maxn];  int res=0;
int main()
 {
    cin>>B>>L;

    for(int i=1;i<=L;i++)
    {
        cin>>a[i];
        res=(res*B%(B+1)+a[i])%(B+1);
    }
    if(!res){
        cout<<"0 0"<<endl;
        return 0;}
    bool flag=1;
    for(int i=L;i;i--)
    {
        vis[i]=flag?1:B;
        flag=!flag;
    }
    for(int i=1;i<=L;i++)
    {
        if(vis[i]==1)
        {
            if(a[i]>=res){
                cout<<i<<" "<<a[i]-res<<endl;
            return 0;}
        }
        else
        {
            if(a[i]>=B+1-res)
            {
                cout<<i<<" "<<a[i]+res-B-1<<endl;
                return 0;
            }

        }
    }

cout<<-1<<" "<<-1;

 return 0;
}
```
原思路代码
```
#include<string.h>
#include<iostream>
#include<stdio.h>
#include<math.h>
#include<stdlib.h>
#include<vector>
#include<string>
using namespace std;
const int maxn=2e5+3;

int B,L,M;
   string A;
string D[maxn];

bool hh()
{ string A;
    for(int i=1;i<=L;i++)
        A+=D[i];//cout<<"uhiu<"<<endl;
    M=stoi(A.c_str());
//cout<<"uhiu<"<<endl;
    if(M%(B+1)==0)
        return 1;
      return 0;

}
int main()
 {

     cin>>B>>L;
     for(int i=1;i<=L;i++){
        cin>>D[i];
        A+=D[i];
     }
      M=stoi(A.c_str());
      if(M%(B+1)==0)
            cout<<"0 0"<<endl;
    for(int i=1;i<=L;i++)
    {
        for(int j=1;j<=L;j++)
        {//cout<<"0 0"<<endl;
       // cout<<"D["<<j<<"]="<<D[j]<<endl;
            int m=stoi(D[j].c_str());
            for(int k=0;k<=m;k++)
            {

            D[j]=to_string(k);

               // cout<<D[j];
       // cout<<"D["<<j<<"]="<<D[j]<<endl;
         if(hh()){
                 cout<<j<<" "<<k<<endl;return 0;}

        }

        }

    }
cout<<-1<<" "<<-1;

 return 0;
}

```







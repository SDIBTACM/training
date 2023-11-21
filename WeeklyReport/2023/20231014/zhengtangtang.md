## 本周感想

一回到家消费降级，一下子敢花钱了，原本想烫个头去见见人的，后来见不上了突然就不知道自己烫头的意义是什么了。
家里那个汉堡，挺奇怪的。我姑买给我吃的时候超级好吃，从来没吃过这么好吃的汉堡。我第二天下午就去那店里买了两个吃了，又变得一点不好吃了。
在家里忙的事情比较碎，比较想说的是助教培训真是压榨我的自由，我的假期还要压榨。
别的按照优先级来排一下：1.陪伴家人，我是真的相家了。2。好好休息调整调整，freedom，和朋友聊天玩耍感受一下走过的路。3.琐碎业务，跟着算法宝典练习紫书题目，国励表，讲读比赛的读书报告，ppt，知识竞赛准备。
还有奋斗的理由：1.一回到家看到那爷爷奶奶住的穷破老年房，我在想都这样了以后不全都得靠我来。
2.我来的时候坐火车来的，就在我们县城那里出发，全家人大半夜11点多的都来送我去车站，我爷爷最后还在围栏去看着我检票过去。真的，那感觉哭死，以后一定带他们出来看看外面的时间。呜呜~（找不到表情）
## 每日一题```https://vjudge.csgrandeur.cn/contest/586655#problem```
没有参加训练赛，把补了的题写一写，看着rank的情况，题目应该是简单的。跳了3道就去做每日一题去了。
### a题 
/*
4 10 9        //n a b
2 3            //x1 y1
1 1
5 10
9 11          //xn yn
/*
56           //可存放的两块地的面积和的最大值
题：给你个矩形纸片，面积=a*b，在可选择的1-n个纸片中选择两个纸片进行无缝拼接，要求拼接后长和宽不超过原有纸片，拼接时候纸片可以任意旋转。
找到能够拼接中的最大的那两个纸片，输出面积和的最大。
解：暴力枚举计算，涉及半个细节
i: 1-n-1;//两两一组，所以最多到倒数第二个。
j:i+1-n;//第i+1组到最后一组。
```
#include <bits/stdc++.h>
#define rep(i, n) for (int i = 1; i <= (n); ++i)
#define debug(c) cout << #c << " = " << c << endl;
using namespace std;
typedef long long ll;
int a[110],b[110];
/*
4 10 9
2 3
1 1
5 10
9 11

/*
56

*/
//
int main(void)
{
    int n,a,b;cin>>n>>a>>b;
    ll x[110],y[110];
    for(int i=1;i<=n;i++){cin>>x[i]>>y[i];}
    ll ans=0;
    for(int i=1;i<=n;i++)
        for(int j=i+1;j<=n;j++)
    {

        int x1=a-x[i],y1=b-y[i];
        if(x1>=0&&y1>=0)
        {
            if((x[j]<=x1&&y[j]<=b)||(y[j]<=x1&&x[j]<=b))//1.注意规范书写逻辑，防止自己写重。，即合理控制变量的变化。
                ans=max(ans,(x[i]*y[i]+x[j]*y[j]));
            else if((x[j]<=y1&&y[j]<=a)||(y[j]<=y1&&x[j]<=a))
            {
                ans=max(ans,x[i]*y[i]+x[j]*y[j]);
            }

        }
        x1= a-y[i],y1=b-x[i];
        if(x1>=0&&y1>=0)
        {
          if((x[j]<=x1&&y[j]<=b)||(y[j]<=x1&&x[j]<=b))
                ans=max(ans,(x[i]*y[i]+x[j]*y[j]));
            else if((x[j]<=y1&&y[j]<=a)||(y[j]<=y1&&x[j]<=a))
            {
                ans=max(ans,x[i]*y[i]+x[j]*y[j]);
            }
        }

    }
    cout<<ans<<endl;
	return 0;
}

```
### b
/*
?aa?   //字符串s,含有'?'
ab     //字符串t
/*
baab    // s中的？变成字母后，与t重复率最高的结果。
*/
题：s中的？会变成任意字母，求与t重复率最高的结果/
解：
1.需要考虑？的数量与t长度和s中原有字符含有t子串的综合情况。
2.基本量可以是字符匹配，cnt(？的数量)，其余字符的数量。
3.先计算出最大可以是多少个t,设为mid个
mid *t = cnt + nums[a]+num[b]+...+nums[t[i]]
4.首先想到的是暴力枚举mid，后来看到是mid影响的结果单调递增，故用二分加速。
5.知道mid就把sumt都翻番，然后利用'a'-'z'遍历筛查一遍就行。
```
#include<math.h>
#include <bits/stdc++.h>
#define rep(i, n) for (int i = 1; i <= (n); ++i)
#define debug(c) cout << #c << " = " << c << endl;
using namespace std;
typedef long long ll;
/*
??b?
za
/*
azbz
*/
#include<map>
ll cnt=0;
map<char,int> sums,sumt;
int check(int mid)
{
    int rnt=cnt;
    for(char a='a';a<='z';a++)
    {

        if(sumt[a]*mid>sums[a])rnt-=abs(sums[a]-sumt[a]*mid);//解3.（返回条件，很巧妙）
        if(rnt<0)return 0;
    }
    return 1;

}
int main()
{
    string s,t;cin>>s>>t;
    //int sums[30],sumt[30];
    int l1=s.size(),l2=t.size();
    for(int i=0;i<l1;i++)sums[s[i]]++,cnt+=(s[i]=='?');
    for(int i=0;i<l2;i++)sumt[t[i]]++;
    ll ans=0;
    int l=0;
    int r=ceil(1e6/t.size());
    while(l<=r)//1.二分加速找到mid
    {

        int mid=(l+r)/2;
        if(check(mid)){
            ans=mid,l=mid+1;
        }
        else
            r=mid-1;
    }
    for(char a='a';a<='z';a++)sumt[a]*=ans;
   // for(int i=0;i<l2;i++)sumt[t[i]]*=ans;
    for(int i=0;i<l1;i++)
    {

        if(s[i]=='?')
        {
            for(char a='a';a<='z';a++)//2.遍历输出，直接a-z简化实现，而不用t中的具体内容。
            {

                if(sumt[a]>sums[a])
                {

                    sumt[a]--;
                    cout<<a;
                    goto end;//新的写法，可以当个小知识点用，跳行功能，goto end；    另一行  end：；
                }
            }
            cout<<'l';
            end:;

        }
        else
            cout<<s[i];
    }
	return 0;
}
```
c
/*
7    // n
add 3   //3压入栈
add 2
add 1
remove    //应丢掉栈顶元素1
add 4
remove    //应丢掉栈顶元素2
remove    //应丢掉栈顶元素3
remove    //应丢掉栈顶元素4
add 6
add 7
add 5
remove    //应丢掉栈顶元素5
remove    //应丢掉栈顶元素6
remove    //应丢掉栈顶元素7
/*
2     //调整顺序的数量
*/
题：n次add，n次remove，按提示进行操作，要求第几次remove就丢掉n，如果不是，你可以在这之前进行一次排序，记录ans++。
注意：It is guaranteed that every box is added before it is required to be removed.
解：
模拟数组栈操作，正是因为上面那个注意，所以我们remove n 的时候 ，可以把 1-n的数全部视为remove，代码表现就是：当
出现一个不匹配的remove时，我们可以使用把栈置零的方法，把k（1-n)的值屏蔽掉
如果正好就正常remove
```
#include<math.h>
#include <bits/stdc++.h>
#define rep(i, n) for (int i = 1; i <= (n); ++i)
#define debug(c) cout << #c << " = " << c << endl;
using namespace std;
typedef long long ll;
/*
7
add 3
add 2
add 1
remove
add 4
remove
remove
remove
add 6
add 7
add 5
remove
remove
remove

/*
2
*/
const int maxn=3e5+10;
ll s[maxn];
int main()
{
    // int s[100],top=0;
    int n;cin>>n;int m=2*n;int k=0,ans=0,top=0;
    while(m--)
    {

        string a;cin>>a;
        if(a[0]=='a')
        {

            int x;cin>>x;
            s[++top]=x;
        }
        else
        {
            k++;//1-当前该remove的值
            if(!top)continue;//2.屏蔽执行
            if(s[top]!=k)
            {
                top=0;//1.屏蔽标志
                ans++;

            }
            else top--;

        }
    }
    cout<<ans<<endl;

	return 0;
}
```
d
/*
5           //  n   3-105 The first line of input contains a positive integer number n (3 ≤ n ≤ 105) — the number of elements in array a.
1 3 2 3 4   //  ai-an  ai:1-1e9 The second line contains n positive integer numbers ai (1 ≤ ai ≤ 109) — the elements of a given array.
/*
2  i,j,k    //这组数可能的数量。Print one number — the quantity of triples (i,  j,  k) such that i,  j and k are pairwise distinct and ai·aj·ak is minimum possible.
*/
题：计算这样三元组的数量，三元组的ijk各不相同并且ai*aj*ak最小
an array a consisting of n positive integer numbers. 
how many triples of indices (i,  j,  k) (i < j < k), such that ai·aj·ak is minimum possible, are there in the array?
思：ai>0,最小的就得是在所有数中最小的那三个中选。
一共需要三个位置ijk，我们需要计算出最小那三个数分别有多少个。
如果第一小的数的个数就>=3,那我们只能选择第一小的数，否则就得看第二小和第三小的数量。
总：mp计数-> sort找前三小->if分类判断
```
#include<math.h>
#include <bits/stdc++.h>
#define rep(i, n) for (int i = 1; i <= (n); ++i)
#define debug(c) cout << #c << " = " << c << endl;
using namespace std;
typedef long long ll;
/*
5
1 3 2 3 4
/*
2
*/
const int maxn=1e5+10;
#include<map>
map<ll,ll>mp;
ll a[maxn];
int main()
{
    int n;
    cin>>n;
    for(int i=1;i<=n;i++)cin>>a[i],mp[a[i]]++;//1.每个数的数量统计
    sort(a+1,a+1+n);//3.排序直接锁定前三个最小的
    if(mp[a[1]]>=3){cout<<mp[a[1]]*(mp[a[1]]-1)*(mp[a[1]]-2)/6<<endl;}//2.分类枚举
    else if(mp[a[1]]==2){cout<<mp[a[3]]<<endl;}
    else if(mp[a[1]]==1){
            if(mp[a[2]]>1)
                {cout<<mp[a[2]]*(mp[a[2]]-1)/2<<endl;}
            else if(mp[a[2]]==1)
                {cout<<mp[a[3]]<<endl;}

    }

	return 0;
}
```
e:
/*
3 11   // n  s  n个数可以选，s是预算；
2 3 5  // ai 每个东西要花费的金钱
/*
2 11   // 买东西的个数，最后花费
*/
题：
思:
总：二分枚举加速找最大数量，直接输出
```
#include <bits/stdc++.h>
#define rep(i, n) for (int i = 1; i <= (n); ++i)
#define debug(c) cout << #c << " = " << c << endl;
using namespace std;
typedef long long ll;
const int maxn =1e5+10;
int a[maxn];
ll b[maxn];
/*
3 11
2 3 5
/*
/*
2 11
*/
int main(void)
{
	int n;cin>>n;
	int k;cin>>k;
	for(int i=1;i<=n;i++)cin>>a[i];
	ll l=1,r=n;
	ll res=0,sum=0,mid;
	while(l<=r)//
    {

         mid=(l+r)/2;
        for(int i=1;i<=n;i++)
        {

            b[i]=a[i]+i*mid;
        }
        sort(b+1,b+1+n);
        ll ans=0;
        for(int i=1;i<=mid;i++)
        {

            ans+=b[i];
        }
        if(ans<=k)
        {

            l=mid+1;
            res=mid;
            sum=ans;
        }
        else
        {
            r=mid-1;
        }
    }
    cout<<res<<" "<<sum<<endl;
	return 0;
}
```
F
/*
3
4 3 1
/*
9
*/
题：
思：
总：
```
#include <bits/stdc++.h>
using namespace std;
const int MAX = 3*1e5 + 7;
const int MOD = 1e9 + 7;
int a[MAX];
typedef long long ll;
long long b[MAX];
int main()
{
    int n;
    cin >> n;
    for(int i=0;i<n;i++)cin>>a[i];
    b[0]=1;
    sort(a,a+n);
    for(int i=1;i<n;i++)b[i]=(b[i-1]*2)%MOD;
    ll ans=0;
    for(int i=0;i<n;i++){ans+=((a[i]*(b[i]-b[n-1-i]))%MOD);ans%=MOD;}
    cout<<ans%MOD<<endl;
    return 0;
}
```
g
/*
3 1 2  //n l r   (1 ≤ n ≤ 105, 1 ≤ l ≤ r ≤ n) — the number of Vasya's cubes（立方体） and the positions told by Stepan.
1 2 3  // a1 a2 a3  the sequence（序列） of integers written on cubes in（按照） the Vasya's order.
3 1 2  // b1 b2 b3  

/*
LIE   //TRUTH or LIE
*/
题：判断:能否只更换 i: l-r 位置之间的数a[i],使a变成b，能输出TRUTH，不能输出LIE；
思：逆向思维，直接判断在 1 -  l-1 和 r+1 - n 区间中，如果有a[i]!=b[i],输出LIE。反之亦然。
总：a,b存数，遍历判断就行。
```
#include <bits/stdc++.h>
#define INF 0x3f3f3f3f
#define maxn 100100
#define N 1111
#define eps 1e-6
#define pi acos(-1.0)
#define e exp(1.0)
using namespace std;
const int mod = 1e9 + 7;
typedef long long ll;
typedef unsigned long long ull;
int vis[maxn], a[maxn], b[maxn];
int main()
{

	int n, l, r;
	while (cin>>n>>l>>r)
	{
		memset(vis, 0, sizeof(vis));
		for (int i = 1; i <= n; i++)
		{
			scanf("%d", &a[i]);
			vis[a[i]]++;
		}
		bool flag = 1;
		for (int i = 1; i <= n; i++)
		{
			scanf("%d", &b[i]);
			vis[b[i]]++;
		}

		for (int i = 1; i <= n; i++)
			if ((i < l || i > r) && a[i] != b[i])
			{
				flag = 0;
				break;
			}
		if (!flag)
			puts("LIE");
		else
			puts("TRUTH");
	}
	return 0;
}
```

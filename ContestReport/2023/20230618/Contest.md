# 21、22级个人赛2（2023.06.18 14:00-18:00）  
## 比赛地址  
https://vjudge.net/contest/563912#overview
## 本次比赛出题人
谢俊杰、栗子旭、赵潼  
# 题解
## A - Points on Line
### 题意
线上有n个点，通过多少种方式选择三个不同的点，以便其中两个最远点之间的距离不超过d。


### 思路
遍历原数组下标 i 从1到n-2，每次二分查找符合的点，并记录下标 L ，如果i到L有三个数字以上，那么每次把答案变量 cnt 加上下标为 L-i-1 的前缀和，遍历之后最终的cnt就是答案，时间复杂度O(nlogn)。

### 代码
```ruby
#include <stdio.h>
#include <string.h>
#include <string>
#include <vector>
#include <stack>
#include <iostream>
#include <map>
#include <set>
#include <queue>
#include <algorithm>
#include <math.h>
//#include <unordered_set>
//#include <unordered_map>
using namespace std;
#define N 5000    
int a[100010];
long long res[100010];
int main()
{
    int n, d;
    cin >> n >> d;
    for (int i = 1; i != n + 1; i++)
        cin >> a[i];
    long long cnt = 0;
    if (n < 3)
    {
        cout << 0 << endl;
        return 0;
    }
    for (int i = 1; i != 100001; i++)
        res[i] = res[i - 1] + i;
    for (int i = 1; i != n - 2 + 1 ; i++)
    {
        int l = i + 1, r = n + 1;
        int m = (l + r) >> 1;
        while (l + 1 != r )
        {
            
            if (a[m] > a[i] + d)
                r = m;
            else
                l = m;
            m = (l + r) >> 1;
        }
        if (i + 2 <= l)
            cnt += res[l - i - 1];
    }
    cout << cnt << endl;
    return 0;
}


```

## B - The Two Routes
### 题意
给出n个点m条边的无向图，为铁路，其补图为公路，火车与公交车同时出发从点1到点n，速度一致，不能同时经过同一顶点，问两种到达方式中 经过边数多的那个 最小值。


### 思路
画出图和补图结果发现点1到n必然存在公路或铁路，而且不受到另一种交通方式的干扰，则必然是该种交通方式的最短路，那么就只用考虑另一种交通方式的最短路就行，于是广搜图或补图就能找到答案，搜不到就说明不可能。

### 代码
```ruby
#include <stdio.h>
#include <string.h>
#include <string>
#include <vector>
#include <stack>
#include <iostream>
#include <map>
#include <set>
#include <queue>
#include <algorithm>
#include <math.h>
//#include <unordered_set>
//#include <unordered_map>
using namespace std;
#define N 5000      
struct Head
{
    int n, q, len;
}head[410];
struct Edge
{
    int to, next;
}edge[160010];
int main()
{
    int n, m;
    cin >> n >> m;
    int cnt = 1;
    for (int i = 1; i != n + 1; i++)
        head[i].n = i, head[i].len = -1;
    for (int i = 1; i != m + 1; i++)
    {
        int x, y;
        cin >> x >> y;
        edge[cnt].to = y;
        edge[cnt].next = head[x].q;
        head[x].q = cnt++;
        edge[cnt].to = x;
        edge[cnt].next = head[y].q;
        head[y].q = cnt++;
    }
    int k = head[1].q;
    int fl = 0;
    while (k)
    {
        int tls=edge[k].to;
        if (tls == n)
        {
            fl = 1;
            break;
        }
        k = edge[k].next;
    }

    queue <Head> que;
    head[1].len = 0;
    que.push(head[1]);
    int flag = 0;
    while (!que.empty())
    {
        Head qf = que.front();
        que.pop();
        int kk = qf.q;
        if (qf.n == n)
        {
            flag = 1;
            break;
        }
        if (fl)
        {
            char hx[410] = { 0 };
            while (kk)
            {
                Edge key = edge[kk];
                hx[key.to] = 1;
                kk = key.next;
            }
            for (int i = 1; i != n + 1; i++)
            {
                if (hx[i] == 0)
                {
                    if (head[i].len == -1)
                    {
                        head[i].len = qf.len + 1;
                        que.push(head[i]);
                    }
                }
            }
        }
        else
        {
            while (kk)
            {
                Edge key = edge[kk];
                if (head[key.to].len == -1)
                {
                    head[key.to].len = qf.len + 1;
                    que.push(head[key.to]);
                }
                kk = key.next;
            }
        }
    }
    if (flag)
        cout << head[n].len << endl;
    else
        cout << -1 << endl;
    return 0;
}


```

## C - I Hate 1111
### 题意
给定整数 x ，问能否用11,111,1111,11111,…组成，可以不用也可以用多次。


### 思路
首先发现111，1111，11111等 mod 11 等于 1或0 。接着发现1111，11111等其实可以用11和111组成，于是问题简化成x是不是由11和111组成的，即x==11*n+111*m？如果是x mod 11==m mod 11，(x-111*(x%11))%11==0 成立。判断时要考虑特殊情况，如(x-111*(x%11))<0时。

### 代码
```ruby
#include <stdio.h>
#include <string.h>
#include <string>
#include <vector>
#include <stack>
#include <iostream>
#include <map>
#include <set>
#include <queue>
#include <algorithm>
#include <math.h>
//#include <unordered_set>
//#include <unordered_map>
using namespace std;
#define N 5000      
int main()
{
    int n;
    cin >> n;
    while (n--)
    {
        int x;
        cin >> x;
        int kk = x % 11;
        int s = x - kk * 111;
        if (s >= 0 && s % 11 == 0)
            cout << "YES" << endl;
        else
            cout << "NO" << endl;
    }

    return 0;
}


```


## D - Coins and Queries
### 题意
n个硬币，q次询问。第二行给你n个硬币的面值（保证都是2的次幂）。每次询问组成b块钱，最少需要多少个硬币？


### 思路
其实就是贪心，每次取可以取最大的币值。
证明（当你缺一个10时，下一个你应该选取8，假设你不选，因为给的数据都是2的幂，所以剩下的只有2和4，而用2和4凑10的过程一定会出现8，所以我们最后还是选择了8.）
这个时候时间复杂度是qn=4*10的10次方，可能会超时，所以我们不存数，而是存它是2的几次方，这样存完我们就得到了2的几次方有几个，此时再去贪心，时间复杂度就是62*10的5次方。
（因为它的数据最大是2*10的9次方，也就是最大是2的31次方）


### 代码
```ruby
#include<stdio.h>
#include<algorithm>
using namespace std;

int s[32];

int log(int x)
{
	int y=0; 
	while(x%2==0)
	{
		x=x/2;
		y++;
	}
	return y;
}

int min(int x,int y)
{
	if(x<y)
		return x;
	else
		return y;
}

int main()
{
	int n,q;
	scanf("%d%d",&n,&q);
	int i,j;
	for(i=0;i<n;i++)
	{
		int x;
		scanf("%d",&x);
		int y=log(x);
		s[y]=s[y]+1;
	}
	for(i=0;i<q;i++)
	{
		int ans=0,k;
		scanf("%d",&k);
		for(j=31;j>=0;j--)
		{
			int t;
			t=min(s[j],k/(1<<j));
			ans+=t;
			k-=(1<<j)*t;
		}
		if(k!=0)
			printf("-1\n");
		else
			printf("%d\n",ans);
	}
	return 0;
}
```

## E - Add Modulo 10
### 题意
题意：给你n个数，可以对其进行任意次a[i]=a[i]+a[i]%10,问最后是否可以使它们都相等。
相等输出Yes，不相等输出No。


### 思路
对个位分别是0到9的数模拟，我们会发现各位是0和5的最后都变成了0。
个位是其他的最后会按照2 4 8 6循环。
看2->4->8->16->22,所以上述循环是以20为一个循环。
也就是将每个数变为个数是2或者0，如果是2就在对20求余，最后依次比较是否相等就可以了。


### 代码
```ruby
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
		{
			scanf("%d",&a[i]);
			while(a[i]%10!=2&&a[i]%10!=0)
				a[i]=a[i]+a[i]%10;
			if(a[i]%10==2)
				a[i]=a[i]%20;
		}
		if(n==1)
			printf("Yes\n");
		else
		{
			for(i=2;i<=n;i++)
				if(a[i]!=a[1])
					break;
			if(i<=n)
				printf("No\n");
			else
				printf("Yes\n");
		}
	}
	return 0;
}
```

## F - Long Number
### 题意
选择一个连续区间，将区间内的数根据所给的函数将其进行替换，求替换后能得到的最大值。


### 思路
将字符数组转换成整数数组，从头开始依次比较该位置的数的大小和所给函数值的大小，如果是该数大就跳过，如果是函数值大就从当前位置开始进行替换，直到遇到比比函数值小的数。

### 代码
```ruby
#include <iostream>
#include <vector>
#include <algorithm>
#include <map>
using namespace std;
int main() {
	int n;
	while (cin >> n) {
		string str;
		cin >> str;
		map<int, int> f;
		for (int i = 1; i <= 9; i++) {
			int t;
			cin >> t;
			f[i] = t;
		}
		for (int i = 0; i <= n; i++) {
			str[i] = str[i] - '0';
		}
		bool flag = 0;
		for (int i = 0; i <= n; i++) {
			if (flag == 0) {
				if (f[str[i]] > str[i]) {
					str[i] = f[str[i]];
					flag = 1;
				}
			}
			else {
				if (f[str[i]] >= str[i]) {
					str[i] = f[str[i]];
				}
				else
					break;
			}
		}
		for (int i = 0; i <= n; i++) {
			str[i] = str[i] + '0';
		}
		cout << str << endl;
	}
	return 0;
}
```

## G - Sagheer and Nubian Market
### 题意
有n件不同纪念品，第i件纪念品的原价是a[i], 如果购买k件纪念品，那么第i件纪念品的价格为原价加上k*i，即a[i] + k*i，求最多能买几件纪念品。


### 思路
用二分法寻找可能买的件数，然后判断是否有足够的钱够买，具体做法是将购买k件纪念品时每个纪念品的价格计算出来，然后由小到大排序，然后将前k件纪念品的价格加起来，看总和是否等于S(所带的钱)。如果大于S，更新右边界，如果小于S，更新左边界。


### 代码
```ruby
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;
int main() {
	long long n, S;
	cin >> n >> S;
	long long low = 0, high = n, mid;
	vector<int> souv(n + 1);
	for (int i = 1; i <= n; i++) {
		cin >> souv[i];
	}
	long long cnt = 0, ans = 0;
	while (low <= high) {
		mid = (low + high) / 2;
		vector<long long> t(n + 1);
		for (int i = 1; i <= n; i++) {
			t[i] = souv[i] + mid * i;
		}
		sort(t.begin(), t.end());
		long long sum = 0;
		for (int i = 1; i <= mid; i++) {
			sum += t[i];
		}
		if (sum > S) {
			high = mid - 1;
		}
		else {
			low = mid + 1;
			if (mid > cnt) {
				cnt = mid, ans = sum;
			}
		}
	}
	cout << cnt << " " << ans;
	return 0;
}
```

## I - Bishwock
### 题意
给一个2 * n的矩阵，‘0’代表空白，‘X’代表被占用，现在可以在空白处放‘L’型，'L‘型由3个单位组成，可以旋转，问最多能放几个'L'型；


### 思路
除了2 * 3的一个矩阵里面全是零的情况下2个’L‘型在中间部分会出现交集，其他时候’L‘型和’L‘型之间都是可以看成一个2 * 2的独立的块的，块和块之间互不影响，那么在dp的时候看当前位置能否构成一个'L‘型，即一个独立的块，然后比较除该块之外的最大值 + 1。当第i和i + 1列构成一个块时dp[i + 1] = max(dp[i-1] + 1,dp[i]),当第i和i - 1列构成一个块时dp[i] = max(dp[i - 2] + 1, dp[i])。


### 代码
```ruby
#include <vector>
#include <algorithm>
#include <iostream>
using namespace std;
int main() {
	string str1, str2;
	cin >> str1 >> str2;
	int len = str1.size();
	str1 = '1' + str1;
	str2 = '1' + str2;
	vector<int> dp(len + 1, 0);
	for (int i = 1; i <= len; i++) {
		dp[i] = max(dp[i], dp[i - 1]);
		if (str1[i] == '0' || str2[i] == '0') {
			if (str1[i - 1] == '0' && str2[i - 1] == '0') {
				dp[i] = max(dp[i - 2] + 1, dp[i]);
			}
			if (str1[i + 1] == '0' && str2[i + 1] == '0') {
				dp[i + 1] = max(dp[i - 1] + 1, dp[i]);
			}
			if (i >= 3 && str1[i] == '0' && str2[i] == '0' &&
				str1[i - 1] == '0' && str2[i - 1] == '0' &&
				str1[i - 2] == '0' && str2[i - 2] == '0') {
				dp[i] = max(dp[i - 3] + 2, dp[i]);
			}
		}
	}

	cout << dp[len] << endl;

	return 0;
}
```

#  比赛排名 

| Rank | Team                  | Score | 
|:----:|:---------------------:|:-----:|
| 1    | SDTBU_WCK(王成魁)        | 6     |
| 2    | SDTBU_ZLK(康康不想秃头)     | 6     |
| 3    | SDTBU_LX(李旭)          | 6     |
| 4    | SDTBU_CWL(陈文龙)        | 6     |
| 5    | js2021230777(田佳璇)     | 5     |
| 6    | SDTBU_LTY(刘统誉)        | 4     |
| 7    | jk22214603(栗子旭)       | 4     |
| 8    | SDTBU_TMQ(唐梦淇)        | 4     |
| 9    | jk21213389(李业昊)       | 4     |
| 10   | szyaizd(周东)           | 3     |
| 11   | jk22214600(赵潼)        | 3     |
| 12   | SDTBU_LT(林涛)          | 3     |
| 13   | SDTBU_XJJ(谢俊杰)        | 3     |
| 14   | SDTBU_GGCX(高晨轩)       | 3     |
| 15   | SDTBU_ZRT(赵若彤)        | 3     |
| 16   | 13626422329(SDTBU_HS) | 3     |
| 17   | SDTBU_MBY(马宝运)        | 3     |
| 18   | SDTBU_SY(孙源)          | 2     |
| 19   | SDTBU_CH(崔涵)          | 2     |
| 20   | wl222121943(付晓龙)      | 2     |
| 21   | SDTBU_SYY(宋玥)         | 1     |
| 22   | SDTBU_wzw(王智炜)        | 1     |
| 23   | SDTBU_GYM(郭一鸣)        | 1     |
| 24   | SDTBU_YSL(杨舒林)        | 1     |
| 25   | SDTBU_LSS(李双双)        | 1     |
| 26   | rg2021213527(王传瑞)     | 1     |
| 27   | SDTBU_xjw(肖俊伟)        | 0     |
| 28   | SDTBU_CJZ(成嘉泽)        | 0     |
| 29   | wl22214941(郑堂堂)       | 0     |
| 30   | jk22214598(祝增圆)       | 0     |
| 31   | jk22214550(刘贵瑜)       | 0     |

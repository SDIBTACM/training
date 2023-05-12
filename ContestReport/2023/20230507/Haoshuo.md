**组队赛 1**

**队员**
- 郝硕
- 田佳璇
- 赵若彤

**比赛地址**
地址：https://vjudge.net/contest/556687#overview

**做出题目**

E

思路一：如果每个数字都能和她应到的位置颜色一样则可以排成升序。

思路二：将n种颜色分为n组，如果每组分别排序后，n组一起还能是升序则Y。
```
#include<stdio.h>
const int maxn=1e5+5;
int main()
{
    int n,k;
    int color[maxn];
    int id[maxn],co[maxn];
    scanf("%d%d",&n,&k);
    for(int i=1;i<=n;i++)
    {
        scanf("%d%d",&id[i],&co[i]);
        color[i]=co[i];
    }
    int i;
    for(i=1;i<=n;i++)
    {
        if(color[id[i]]!=co[i])
            break;
    }
    if(i>n)
        printf("Y\n");
    else printf("N\n");
    return 0;
}
```

F

思路：判断差值，暴力即可

```
#include<stdio.h>
const int maxn=1e5+5;
int main()
{
    int T,D,M;
    scanf("%d%d%d",&T,&D,&M);
    int s[M];
    s[0]=0;
    s[M+1]=D;
    for(int i=1;i<=M;i++)
        scanf("%d",s+i);
    int i;
    for(i=1;i<=M+1;i++)
    {
        if(s[i]-s[i-1]>=T)
            break;
    }
    if(i<=M+1) printf("Y\n");
    else printf("N\n");
    return 0;
}
```

**有思路但没做出的题**

C

思路：使用两个优先队列，比较两个方向的最早到达时间，最早到达的先运行，若此方向第i个人在第I-1个到达时间+10s内，则总用时为第i个人的时间+10s.
否则总时间直接+10s。
```
#include<iostream>
#include<algorithm>
#include<vector>
#include<queue>
using namespace std;
priority_queue<int, vector<int>, greater<int> >a;
priority_queue<int, vector<int>, greater<int> >b;
int main()
{
 int n;
 cin >> n;
 for (int i = 1;i <= n;i++)
 {
  int x, m;
  cin >> x >> m;
  if (m == 0)
   a.push(x);
  else if (m == 1)
   b.push(x);
 }
 int summ = 0;
 int xa, xb;
 while (!a.empty() && !b.empty())
 {
  xa = a.top();
  xb = b.top();
  if (xa < xb)
  {
   summ = max(summ, xa) + 10;
   a.pop();
   while (!a.empty() && a.top() < summ)
   {
    summ = max(summ, a.top() + 10);
    a.pop();
   }
  }
  else
  {
   summ = max(summ, xb) + 10;
   b.pop();
   while (!b.empty() && b.top() < summ)
   {
    summ = max(summ, b.top() + 10);
    b.pop();
   }
  }

 }
 if (!a.empty())
 {
  xa = a.top();
  summ = max(summ, xa) + 10;
  a.pop();
  while (!a.empty())
  {
   summ = max(summ,a.top() + 10);
   a.pop();
  }
 }
 if(!b.empty())
 {
  xb = b.top();
  summ = max(summ, xb) + 10;
  b.pop();
  while (!b.empty())
  {
   summ = max(summ, b.top() + 10);
   b.pop();
  }
 }
 cout << summ << endl;
}
```

D

思路：A有两个解决方案，每个A只对其后面第一个或第二个有影响，所求方案要求最小字典序，对于连续的A来讲,f[i] = f[i - 1] + f[i - 2]，
B对后续活动无影响，只起到分割序列的作用，分割后就是多个斐波那契数列相乘。

```
#include <iostream>
#include <algorithm>
using namespace std;
#define ll long long
int flag;
ll a[100], fi[100];
void dfs(ll x, int ans, ll cnt) 
{
 if (x == 1) 
 {
  flag = 1;
  for (ll i = 1; i <= ans - 1; i++)
  {
   for (ll j = 1; j <= a[i]; j++) 
    cout << 'A';
   cout << 'B';
  }
  return;
 }
 for (ll i = cnt; i >= 2; i--) 
 {
  if (x % fi[i] == 0) 
  {
   a[ans] = i - 1;
   dfs(x / fi[i], ans + 1, i);
   if (flag) 
    return;
  }
 }
}
int main() 
{
 ll n;
 cin >> n;
 fi[0] = 1;
 fi[1] = 1;
 for (int i = 2; i <= 73; i++) 
  fi[i] = fi[i - 1] + fi[i - 2]; 
 dfs(n, 1, 73);
 if (!flag)
  cout << "IMPOSSIBLE" << endl;
 return 0;
}
```

G

思路：使用不相交集数据结构来跟踪每个人的祖先。 最初，我们将每个人的父节点设置为它本身。

每当有新孩子出生时，我们创建一个具有下一个最低唯一标识符的新节点，并使用 merge 函数将其与其父节点的集合合并。 

每当有人死亡时，我们将其标记为死亡，并遍历其孩子的集合，如果没孩子则找兄弟，判断其有无孩子，有则数字小的孩子继承，无则重复。

```
#include <stdio.h>
#define MAXN 100005

int p[MAXN];
int cnt;
int alive[MAXN];

int find(int x) {
    if (p[x] == x) return x;
    return p[x] = find(p[x]);
}

void merge(int x, int y) {
    int px = find(x), py = find(y);
    p[px] = py;
}

int main() {
    int q;
    scanf("%d", &q);
    for (int i = 1; i <= MAXN - 5; i++) {
        p[i] = i;
    }
    cnt = 1;
    alive[1] = 1;
    while (q--) {
        int type, x;
        scanf("%d%d", &type, &x);
        if (type == 1) {
            cnt++;
            p[cnt] = cnt;
            alive[cnt] = 1;
            merge(cnt, x);
        } else {
            alive[x] = 0;
            int curr = find(x);
            while (++curr <= cnt && !alive[curr]) {}
            if (curr > cnt) {
                printf("0\n");
            } else {
                printf("%d\n", curr);
            }
        }
    }
    return 0;
}
```

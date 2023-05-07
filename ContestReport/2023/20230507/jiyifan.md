**组队赛第一周**

- jyf
- 刘骋羽
- 廖银

**比赛地址**

https://vjudge.net/contest/556687#overview

**做出题目**

F

**题意**: 签到题 略
**思路**: 略

```C++
#include <bits/stdc++.h>

using namespace std;

#define endl '\n'

const int N = 1e5 + 5;
const int M = 1e3 + 5;

int a[M];
int t, d, m;

int main()
{
    cin >> t >> d >> m;
    for (int i = 1; i <= m; i++) cin >> a[i];
    a[m + 1] = d;
    for (int i = 0; i <= m; i++) {
        if (a[i + 1] - a[i] >= t) {
            cout << "Y" << endl;
            return 0;
        }
    }
    cout << "N" << endl;
    return 0;
}

```

E
**题意**: 数组中所有数字都有一个颜色, 并且可以交换任意两个颜色相同数字无数次, 问最终能否使整个数组为升序 **(数组中所有数字均唯一)**

**思路**: 将数字按照颜色分组, 并在内部进行排序, 再将排好序的各个颜色数组中的数字放回对应的位置, 最后遍历整个数组验证是否有序. 由于对每一种颜色, 只有其内部有数字时, 才对其进行排序, 所以整体时间复杂度为 O(nlogn)

```c++
#include <bits/stdc++.h>

using namespace std;

const int N = 1e5 + 5;

vector<int>v[N];
vector<int>p[N];

map<int, int> pos;

int n, k;

vector<int> a(N);

#define endl '\n'

int main()
{
    cin >> n >> k;
    for (int i = 1; i <= n; i++) {
        int x, c; cin >> x >> c;
        v[c].push_back(x);
        p[c].push_back(i);
        pos[x] = i;
    }
    for (int i = 1; i <= k; i++) {
        if (v[i].empty()) continue;
        sort(v[i].begin(), v[i].end());
        for (int j = 0; j < p[i].size(); j++) {
            a[p[i][j]] = v[i][j];
        }

    }

    if (is_sorted(a.begin() + 1, a.begin() + n + 1)) {
        cout << "Y" << endl;
    } else {
        cout << "N" << endl;
    }
	return 0;
}
```

C

**题意**: 电梯两端有乘客, 电梯一次只能向一个方向运行, 并且在当乘客登上电梯后, 若是在 10s 内依旧有同方向乘客登上电梯, 10s 倒计时重新计时, 否则就按照两端首位乘客的到达时间决定电梯先向哪个方向运行. 计算两方向乘客都互相做过电梯后的消耗时间.

**思路**: 模拟即可, 使用两个队列, 分别记录不同方向的乘客到达时间, 当电梯未运行时, 比较两方向上最先到达的乘客的到达时间, 选择其中到达时间最早的乘客登上电梯, 然后在考虑其之后的乘客是否能在 10s 内登上电梯, 考虑到会有乘客先到达的情况, 所以如果有多个乘客提前到达则将其视为在同一秒内可以登上电梯.

```c++
#include <bits/stdc++.h>

using namespace std;

#define endl '\n'

const int N = 1e4 + 5;

int n, a[N], b[N];
queue <int> q[2];
bool vis[N];

int main()
{
    int n;
    cin >> n;
    for(int i = 1; i <= n; i++)
    {
        int t, d;
        cin >> t >> d;
        q[d].push(t);
    }
    int ed = 0;
    while(!q[0].empty() || !q[1].empty())
    {
        if(q[0].empty())
        {
            ed = max(ed, q[1].front()) + 10;
            q[1].pop();
            while(!q[1].empty())
            {
                ed = max(ed, q[1].front() + 10);
                q[1].pop();
            }
            break;
        }
        if(q[1].empty())
        {
            ed = max(ed, q[0].front()) + 10;
            q[0].pop();
            while(!q[0].empty())
            {
                ed = max(ed, q[0].front() + 10);
                q[0].pop();
            }
            break;
        }
        if(q[0].front() < q[1].front())
        {
            ed = max(ed, q[0].front()) + 10;
            q[0].pop();
            while(!q[0].empty() && q[0].front() <= ed)
            {
                ed = max(ed, q[0].front() + 10);
                q[0].pop();
            }
        }
        else
        {
            ed = max(ed, q[1].front()) + 10;
            q[1].pop();
            while(!q[1].empty() && q[1].front() <= ed)
            {
                ed = max(ed, q[1].front() + 10);
                q[1].pop();
            }
        }
        // cout << ed << endl;
    }
    cout << ed << endl;
    return 0;
}
```

B

**题意**: 给定 B 进制下的位数为 L 的数字, 并给出这 L 位上每一位数字, 组成的数字 M, 要求你可以最多减少 M 上的某一位数字(不能小于 0), 使得修改后的数字可以整除 (B + 1)

**思路**: 首先可以发现, 对于在 B 进制,  数字 M 为 $a_l, a_{l - 1}, a_i, ... a_2, a_1, a_0$ 时, 某一位减少 1 后 M 在模 (B + 1) 意义下的值会减少 1 **(当 i 为偶数时)**, 或者 B **(当 i 为奇数时)**. 随后对求出 $M mod (B + 1)$ 下的 res, 该 res 表示 M 需要的某一位需要减少在 $mod(B + 1)$ 下的值. 若选择 i 为奇数位的位置来减小, 则需要保证 $res \le a_i$ , 否则当前位置不能满足条件, 若选择 i 为偶数位时的位置来进行减小, 则要保证 $a_i * B = res + k(B + 1) (K\ge 0)$ , 对这 L 位数字进行遍历, 若存在 $a_i$ 满足上述条件则说明可以构造出, 反之则不可以构造出.

```c++
#include <bits/stdc++.h>

using namespace std;

#define endl '\n'

const int N = 2e5 + 5;
const int M = 1e4 + 5;


int b, l;

int a[N], v[N];

int main()
{
    cin >> b >> l;
    int res = 0;
    for (int i = 1; i <= l; i++) {cin >> a[i]; res = (res * b % (b + 1) + a[i]) % (b + 1); }
    if (!res) {
        cout << "0 0" << endl;
        return 0;
    }
    bool flag = 1;
    for (int i = l; i; i--) {
        v[i] = flag ? 1 : b;
        flag = !flag;
    }
    int pos = -1, value = -1;
    for (int i = 1; i <= l; i++) {
        if (!a[i]) continue;
        if (v[i] == 1) {
            if (a[i] >= res) {
                cout << i << " " << a[i] - res << endl;
                return 0;
            }
        } else {
            if (a[i] >= b - res + 1) {
                cout << i << " " << a[i] - b + res - 1 << endl;
                return 0;
            }
        }

    }
    cout << "-1 -1" << endl;
    return 0;
}
```

G:

**题意**: 王位继承问题，要求确认每次人去世后的继承人编号。规则如下

- 遵循直系优先于旁系规则，也就是当支系的祖先去世后
- 同一支系下，优先选择编号较小的孩子

现在有两种操作:

1. $op = 1, x_i$ 表示编号为$x_i$ 的节点下新增一个儿子
2. $op = 2, x_i$ 表示编号为$x_i$ 节点去世, 不能继承王位, 只能由其嫡系或者支系继承王位

**思路**:

分为两种思路:

1. 离线算法

   可以将这棵树先构建出来, 然后可以发现, 所谓的王位继承顺序本质上就是在这棵树的 `dfs` 序上找到当前未死亡的首节点, 所以只需要进行离线后, 将所有 1 操作构建出树后进行 `dfs` 序, 再按照 2 操作的顺序维护当前 `dfs` 序下未死亡的首节点即可.

   ```c++
   #include <bits/stdc++.h>
   
   using namespace std;
   
   const int N = 1e6 + 5;
   
   vector<int>e[N];
   
   int d[N];
   int vis[N], cnt = 0;
   
   queue<pair<int, int>> q;
   
   void dfs(int u) {
       for (auto v: e[u]) {
           q.push({v, ++cnt});
           dfs(v);
       }
   }
   
   int n;
   
   int main()
   {
       scanf("%d", &n);
       int tot = 1, ct = 0;
       for (int i = 1; i <= n; i++) {
           int t, x; scanf("%d %d", &t, &x);
           if (t == 1) e[x].push_back(++tot);
           else d[++ct] = x;
       }
       q.push({1, ++cnt});
       dfs(1);
       int now = 1;
       for (int i = 1; i <= ct; i++) {
           vis[d[i]] = 1;
           if (!vis[now]) printf("%d\n", now);
           else {
               auto t = q.front();
               while (vis[t.first]) { q.pop(); t = q.front(); }
               now = t.first;
               printf("%d\n", now);
           }
       }
       return 0;
   }
   ```

   

2. 在线算法

   在线算法的话就比较麻烦, 首先是要寻找自己这一棵子数下满足条件的继承人. 若是没有的话就要回溯到父节点进行查询, 比较绕, 但是实际复杂度大致是 O(n + klogE) (E 是边数, k是常数). 具体实现就是 O(n) 查询当前国王的子树, 是否有合适的继承人. 若没有则返回到父节点去查找兄弟支系中是否有合适的继承人. 在查找兄弟支系的继承人时, 可以发现, 我们只需要检查当前节点的右侧兄弟就可以, 如果左侧兄弟中有合适继承人的话, 那么当前死掉的国王就不会是当前节点, 而是左侧兄弟中合适的继承人. 那么这样就保证了每一个节点都会只搜它右侧的兄弟节点, 而且每一个子树都只会进入一次, 也就保证了总遍历的次数为 O(n) 的, 只不过在找它最右侧兄弟节点时需要二分它父节点的子节点序列, 所以会有 k 次(不好估计, 不过不会很大) logE 的二分查询消耗

   PS: 不建议在线, 因为会很绕, 当然如果有更优雅的代码, 欢迎提出

   ```c++
   #include <bits/stdc++.h>
   
   using namespace std;
   
   #define endl '\n'
   
   const int N = 1e5 + 5;
   const int M = 1e3 + 5;
   
   int fa[N], flag[N];
   vector <int> edge[N];
   
   int dfs2(int, int);
   
   int dfs1(int u, int type)
   {
       if(flag[u])
           return u;
       for(int i = 0; i < edge[u].size(); i++)
       {
           int v = edge[u][i];
           int res = dfs1(v, 0);
           if(res)
               return res;
       }
       if(type == 1)
           return dfs2(fa[u], u);
       else
           return 0;
   }
   
   int dfs2(int f, int u)
   {
       int pos = lower_bound(edge[f].begin(), edge[f].end(), u) - edge[f].begin() + 1;
       for(int i = pos; i < edge[f].size(); i++)
       {
           int v = edge[f][i];
           int res = dfs1(v, 0);
           if(res)
               return res;
       }
       return dfs2(fa[f], f);
   }
   
   int main()
   {
       int _;
       scanf("%d", &_);
       int tot = 1, now = 1;
       flag[1] = 1;
       while(_--)
       {
           int t, x;
           scanf("%d %d", &t, &x);
           if(t == 1)
           {
               tot++;
               edge[x].push_back(tot);
               fa[tot] = x;
               flag[tot] = 1;
           }
           else
           {
               flag[x] = 0;
               int ans = dfs1(now, 1);
               now = ans;
               printf("%d\n", ans);
           }
       }
       return 0;
   }
   ```

   

# 比赛总结

---

感觉这场比赛榜带的有点歪，今天下午做题的时候又有两道题犯了很多不该犯的错误，导致调bug用了很多时间，后面没时间切题了……犯错误不是坏事，能从错误中学到东西，何尝不是一件好事？

### 比赛地址：[sdtbu选拔赛5 - Virtual Judge (vjudge.net)](https://vjudge.net/contest/554049)

### 题解：

##### A

思路：

签到题，我的做法是统计出有多少个a，再统计出有多少个单独的a，两者的差就是答案。

代码：

```cpp
#include <iostream>
using namespace std;
#define N 100005
char s[N];
int main()
{
    int n;
    cin >> n;
    cin >> s + 1;
    int ans = 0, cnt = 0;
    for(int i = 1; i <= n; i++)
    {
        if(s[i] == 'a')
        {
            ans++;
            if(s[i - 1] != 'a' && s[i + 1] != 'a')
                cnt++;
        }
    }
    ans -= cnt;
    cout << ans << endl;
    return 0;
}

```

##### B

思路：

这个题也是挺麻烦的，我敲了160行，首先，要能抽象出这题是一道广搜求最大连通块的题，能抽象出题目考点的话剩下的就是实现了。实现也不简单，将所有切割的线都当作标记存起来，因为横着切的线使得上下没法走，竖着切的线使得左右没法走。注意要开vector存，因为在某一行或者某一列切的线可能不唯一，可能有很多条不连续的切割线，所以不能单纯记下某一行或者某一列切割线的最大值最小值。之后先从(1, 1)开始搜，这次搜索的结果不记录，因为这不是切割留下的区域，从(1, 1)开搜是因为题目中说了最外一圈没有切割线。之后就顺序遍历每一个格子，如果没搜过就从这个点开始广搜，求这个连通块的大小。答案就是所有切割得到的连通块中最大的那一个。

代码：

```cpp
#include <iostream>
#include <queue>
#include <vector>
using namespace std;
#define N 1005
int flag[N][N];
int dir[4][2] = {{0, 1}, {0, -1}, {-1, 0}, {1, 0}};
struct F1
{
    int l;
    int r;
};
struct F2
{
    int d;
    int u;
};
vector <F1> f1[N];
vector <F2> f2[N];
int bfs(int x, int y)
{
    int res = 0;
    queue <pair <int, int> > q;
    q.push({x, y});
    flag[x][y] = 1;
    res++;
    while(!q.empty())
    {
        pair <int, int> p = q.front();
        q.pop();
        for(int i = 0; i < 4; i++)
        {
            int xx = p.first + dir[i][0];
            int yy = p.second + dir[i][1];
            if(xx < 1 || xx > 1000 || yy < 1 || yy > 1000)
                continue;
            if(i == 0)
            {
                int f = 1;
                for(int j = 0; j < f1[p.second].size(); j++)
                {
                    if(p.first >= f1[p.second][j].l && p.first <= f1[p.second][j].r)
                    {
                        f = 0;
                        break;
                    }
                }
                if(!f)
                    continue;
                else
                {
                    if(flag[xx][yy])
                        continue;
                    q.push({xx, yy});
                    flag[xx][yy] = 1;
                    res++;
                }
            }
            if(i == 1)
            {
                int f = 1;
                for(int j = 0; j < f1[yy].size(); j++)
                {
                    if(p.first >= f1[yy][j].l && p.first <= f1[yy][j].r)
                    {
                        f = 0;
                        break;
                    }
                }
                if(!f)
                    continue;
                else
                {
                    if(flag[xx][yy])
                        continue;
                    q.push({xx, yy});
                    flag[xx][yy] = 1;
                    res++;
                }
            }
            if(i == 2)
            {
                int f = 1;
                for(int j = 0; j < f2[xx].size(); j++)
                {
                    if(p.second >= f2[xx][j].d && p.second <= f2[xx][j].u)
                    {
                        f = 0;
                        break;
                    }
                }
                if(!f)
                    continue;
                else
                {
                    if(flag[xx][yy])
                        continue;
                    q.push({xx, yy});
                    flag[xx][yy] = 1;
                    res++;
                }
            }
            if(i == 3)
            {
                int f = 1;
                for(int j = 0; j < f2[p.first].size(); j++)
                {
                    if(p.second >= f2[p.first][j].d && p.second <= f2[p.first][j].u)
                    {
                        f = 0;
                        break;
                    }
                }
                if(!f)
                    continue;
                else
                {
                    if(flag[xx][yy])
                        continue;
                    q.push({xx, yy});
                    flag[xx][yy] = 1;
                    res++;
                }
            }
        }
    }
    return res;
}
int main()
{
    ios::sync_with_stdio(0);
    cin.tie(0), cout.tie(0);
    int n;
    cin >> n;
    int x, y;
    cin >> x >> y;
    for(int i = 1; i <= n; i++)
    {
        int xx, yy;
        cin >> xx >> yy;
        if(xx != x)
            f1[y].push_back({min(x, xx) + 1, max(x, xx)});
        if(yy != y)
            f2[x].push_back({min(y, yy) + 1, max(y, yy)});
        x = xx, y = yy;
    }
    bfs(1, 1);
    int ans = 0;
    for(int i = 1; i <= 1000; i++)
    {
        for(int j = 1; j <= 1000; j++)
        {
            if(flag[i][j])
                continue;
            ans = max(ans, bfs(i, j));
        }
    }
    cout << ans << endl;
    return 0;
}

```

##### C

思路：

这个题值得深思，晚上我又跟好几个人一起讨论了下这个题，先说结论：题目中的中点是指真正的中点，坐标不整除不下取整；有的点到不了，题目中给的点都是可以到达的点；能到达的点绝对是两个坐标同时到达，不存在其中一个坐标到达而另外一个坐标不到达的情况；不能到达的点应该是由于题目中的中点是真正的中点，不存在下取整的情况而到达不了，不能到达的点只能通过震荡无限接近而不能到达。例如4 12 8这个样例，(12, 8)这个点无法到达，只能通过各种方法无限接近这个点。但是代码是对的，因为题目中说了给出的点都是可以到达的，根据之前的分析能到达一个点一定是两个坐标同时到达，现在只需要把题目视为一个一维的数轴，从2^(n - 1)开始搜索，每次可以+0除以2或者+2^n除以2，遇到奇数则不处理，因为无法得到整数。这样搜到的到每一个数的所需操作数都是最少的，之后只需输出到x或者到y任意一个点的操作次数即可，因为两者相等。

代码：

```cpp
#include <iostream>
#include <queue>
using namespace std;
#define N 2000005
int flag[N], cnt[N];
void bfs(int x, int number)
{
    queue <pair <int, int> > q;
    q.push({x, 0});
    flag[x] = 1;
    cnt[x] = 0;
    while(!q.empty())
    {
        pair <int, int> p = q.front();
        q.pop();
        if(p.first % 2)
            continue;
        if(!flag[p.first / 2])
        {
            q.push({p.first / 2, p.second + 1});
            flag[p.first / 2] = 1;
            cnt[p.first / 2] = p.second + 1;
        }
        if(!flag[(p.first + number) / 2])
        {
            q.push({(p.first + number) / 2, p.second + 1});
            flag[(p.first + number) / 2] = 1;
            cnt[(p.first + number) / 2] = p.second + 1;
        }
    }
}
int main()
{
    int n, x, y;
    cin >> n >> x >> y;
    bfs(1 << (n - 1), 1 << n);
    int ans = max(cnt[x], cnt[y]);
    cout << ans << endl;
    return 0;
}

```

##### D

思路：

从后向前考虑，将遇到的每一个数的下标都放进行相应的队列中，因为我们是从后向前跑的，所以队列中的顺序是这个数从后向前出现的顺序。再来考虑箭扎爆一个高度为H的气球后，接下来会去扎高度为H - 1的气球，如果当前高度为H - 1的队列不为空，则取出队首，队首即为下次会扎爆的气球的下标，由此遍历，可得到一个nx数组，nx[i]代表扎爆下标为i的气球后下一次要扎爆的气球的下标。虽然从后向前遍历和题目中射箭的顺序可能不一样，但是最终得到的最优解是一样的，从后向前跑更符合我们的认知以及方便实现。

代码：

```cpp
#include <iostream>
#include <queue>
using namespace std;
#define N 500005
int a[N], nx[N], flag[N];
queue <int> q[N << 1];
int main()
{
    int n;
    cin >> n;
    for(int i = 1; i <= n; i++)
        cin >> a[i];
    for(int i = n; i >= 1; i--)
    {
        q[a[i]].push(i);
        if(!q[a[i] - 1].empty())
        {
            nx[i] = q[a[i] - 1].front();
            q[a[i] - 1].pop();
        }
    }
    int ans = 0, l = 1;
    while(1)
    {
        while(flag[l])
            l++;
        if(l > n)
            break;
        ans++;
        int t = l;
        do {
            flag[t] = 1;
            t = nx[t];
        } while(t);
    }
    cout << ans << endl;
    return 0;
}

```

##### E

思路：

一道简单的模拟题，对于任意一个字符串，遍历它的每一个字符，如果当前字符是*，则暴力地枚举将这个*变成a~z中的任意一个字符而得到的字符串，并将这个字符串放到map中统计出现次数。由于map底层实现的特性，我们顺序遍历map其实就是按照字典序遍历map，所以只需输出顺序遍历map所得到的出现次数最多的字符串即可。

代码：

```cpp
#include <iostream>
#include <map>
using namespace std;
#define N 10005
string s[N];
map <string, int> mp;
int main()
{
    int n, c;
    cin >> n >> c;
    for(int i = 1; i <= n; i++)
        cin >> s[i];
    for(int i = 1; i <= n; i++)
    {
        for(int j = 0; j < s[i].length(); j++)
        {
            if(s[i][j] != '*')
                continue;
            for(char k = 'a'; k <= 'z'; k++)
            {
                s[i][j] = k;
                mp[s[i]]++;
            }
        }
    }
    int mx = 0;
    string ans;
    for(auto it : mp)
    {
        if(it.second > mx)
        {
            mx = it.second;
            ans = it.first;
        }
    }
    cout << ans << " " << mx << endl;
    return 0;
}

```

##### F

思路：

这也是一道签到题，如果输入的数字中有9就输出”F“，否则输出”S“。

代码：

```cpp
#include <iostream>
using namespace std;
int main()
{
    int flag = 1, a[10];
    for(int i = 1; i <= 8; i++)
        cin >> a[i];
    for(int i = 1; i <= 8; i++)
    {
        if(a[i] == 9)
        {
            flag = 0;
            break;
        }
    }
    if(flag)
        cout << "S" << endl;
    else
        cout << "F" << endl;
    return 0;
}

```

##### G

思路：

又是一道模拟题，首先要能读懂题，题目中的细节还是挺多的。首先J、Q、K分别用11、12、13表示，但是它们的点数只算10。我们可以开一个数组来存当前这13种牌各剩多少张，题目中一旦出现11、12、13，张数按11、12、13减，但是价值按10加。之后我们就可以从小到大遍历了，在当前牌有剩余的情况下，如果加上当前点数Mary的点数正好是23或者加上当前点数John爆掉了但是Mary没爆掉，就满足条件，输出答案。注意题目让输出的是最小价值，不是最小点数，如果是11、12、13的话输出10，如果无解的话输出-1。

代码：

```cpp
#include <iostream>
using namespace std;
int main()
{
    int j[5], m[5], a[10], cnt[15];
    for(int i = 1; i <= 13; i++)
        cnt[i] = 4;
    int n;
    cin >> n;
    cin >> j[1] >> j[2];
    cin >> m[1] >> m[2];
    cnt[j[1]]--, cnt[j[2]]--;
    cnt[m[1]]--, cnt[m[2]]--;
    for(int i = 1; i <= n; i++)
    {
        cin >> a[i];
        cnt[a[i]]--;
    }
    int sum1 = (j[1] > 10 ? 10 : j[1]) + (j[2] > 10 ? 10 : j[2]);
    int sum2 = (m[1] > 10 ? 10 : m[1]) + (m[2] > 10 ? 10 : m[2]);
    for(int i = 1; i <= n; i++)
    {
        sum1 += a[i] > 10 ? 10 : a[i];
        sum2 += a[i] > 10 ? 10 : a[i];
    }
    int ans = 0, flag = 0;
    for(int i = 1; i <= 10; i++)
    {
        if(!cnt[i])
            continue;
        if(sum2 + i == 23 || sum1 + i > 23 && sum2 + i <= 23)
        {
            flag = 1;
            ans = i;
            break;
        }
    }
    if(!flag)
    {
        for(int i = 11; i <= 13; i++)
        {
            if(!cnt[i])
                continue;
            if(sum2 + 10 == 23 || sum1 + 10 > 23 && sum2 + 10 <= 23)
            {
                flag = 1;
                ans = 10;
                break;
            }
        }
    }
    if(!flag)
        cout << -1 << endl;
    else
        cout << ans << endl;
    return 0;
}

```

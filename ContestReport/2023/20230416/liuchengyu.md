# 比赛总结

---

### 比赛地址：[sdtbu选拔赛4 - Virtual Judge (vjudge.net)](https://vjudge.net/contest/552923)

### 题解：

##### A

思路：

很简单的一道深搜题，刚开始看题的时候以为可能会有很多出边，但是仔细看题目的输入，上面写着”All trader names will be unique, and no object will be wanted by more than one person and owned by more than one person.“，说明一个点最多有一条出边，这样的话题目就更简单了，从第一个点到最后一个点暴力判断能形成的最长的环是多少即可。

代码：

```cpp
#include <iostream>
#include <cstring>
#include <vector>
#include <map>
using namespace std;
#define N 105
int flag[N];
string s1[N], s2[N];
vector <int> edge[N];
map <string, int> has, wants;
int dfs(int u, int cnt)
{
    if(flag[u])
        return cnt;
    flag[u] = 1;
    for(int i = 0; i < edge[u].size(); i++)
    {
        int v = edge[u][i];
        return dfs(v, cnt + 1);
    }
    return 0;
}
int main()
{
    int n;
    cin >> n;
    for(int i = 1; i <= n; i++)
    {
        string name;
        cin >> name >> s1[i] >> s2[i];
        has[s1[i]] = i;
        wants[s2[i]] = i;
    }
    for(int i = 1; i <= n; i++)
        if(wants[s1[i]])
            edge[i].push_back(wants[s1[i]]);
    int ans = 0;
    for(int i = 1; i <= n; i++)
    {
        memset(flag, 0, sizeof(flag));
        ans = max(ans, dfs(i, 0));
    }
    if(ans)
        cout << ans << endl;
    else
        cout << "No trades possible" << endl;
    return 0;
}

```

##### B

思路：

纯模拟题，没啥好说的，按着题目的意思走呗。有不少人不会读一整行带空格的字符串，其实直接用getline()函数就可以了啊，记得在getline()之前别忘了用getchar()取走上一行最后的换行，要不然读不进去这行字符串。另外提醒一下大家如果用ios::sync_with_stdio(0)关同步流之后千万别把C的输入输出流跟C++的输入输出流混用，会出错（至于为什么我还真没深究过）。

代码：

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <map>
using namespace std;
#define N 15
int m[N][N];
vector <int> v1[1005], v2[1005];
map <char, int> mp1;
map <int, char> mp2;
void init()
{
    int tot = 0;
    for(char i = 'A'; i <= 'Z'; i++)
        mp1[i] = tot++;
    for(char i = '0'; i <= '9'; i++)
        mp1[i] = tot++;
    mp1[' '] = tot++;
    for(auto it : mp1)
        mp2[it.second] = it.first;
}
int main()
{
    init();
    int n;
    cin >> n;
    for(int i = 1; i <= n; i++)
        for(int j = 1; j <= n; j++)
            cin >> m[i][j];
    string s;
    getchar();
    getline(cin, s);
    for(int i = 0; i < s.length(); i++)
        v1[i / n].push_back(mp1[s[i]]);
    for(int i = v1[(s.length() - 1) / n].size() + 1; i <= n; i++)
        v1[(s.length() - 1) / n].push_back(mp1[' ']);
    for(int i = 0; i <= (s.length() - 1) / n; i++)
    {
        for(int j = 1; j <= n; j++)
        {
            int t = 0;
            for(int k = 1; k <= n; k++)
                t = (t + m[j][k] * v1[i][k - 1]) % 37;
            v2[i].push_back(t);
        }
    }
    for(int i = 0; i <= (s.length() - 1) / n; i++)
        for(int j = 0; j < v2[i].size(); j++)
            cout << mp2[v2[i][j]];
    cout << endl;
    return 0;
}

```

##### C

思路：

这是道签到题，但是我不能理解为什么这题的正确率连三分之一都不到……题意也很直接啊，如果a击败了b并且a的排名比b的排名肯后，那么就把b + 1,b + 2,...,a这些人的排名向前移动一个，并将b放到a的位置即可。移动元素的话大一C语言讲冒泡排序的时候就讲过，这个应该都会吧。

代码：

```cpp
#include <iostream>
using namespace std;
#define N 105
int a[N];
int main()
{
    int n, m;
    cin >> n >> m;
    for(int i = 1; i <= n; i++)
        a[i] = i;
    while(m--)
    {
        char c1, c2;
        int t1, t2;
        cin >> c1 >> t1 >> c2 >> t2;
        int pos1 = 0, pos2 = 0;
        for(int i = 1; i <= n; i++)
        {
            if(a[i] == t1)
                pos1 = i;
            if(a[i] == t2)
                pos2 = i;
        }
        if(pos1 > pos2)
        {
            for(int i = pos2; i <= pos1 - 1; i++)
                a[i] = a[i + 1];
            a[pos1] = t2;
        }
    }
    for(int i = 1; i <= n; i++)
    {
        if(i == 1)
            cout << "T" << a[i];
        else
            cout << " " << "T" << a[i];
    }
    cout << endl;
    return 0;
}

```

##### D

思路：

怎么感觉我写的题解跟我平时在实验室讲题一样呢，我总感觉只要点下核心思路就行，但是每次讲题感觉底下都是一脸懵逼，都跟不上我的思路……这个D题也是个模拟题，算是个不小的模拟了，我写了175行。思路也是很直接，在这里简单放上我的处理方法。判断某个位置是否有唯一可行放置（详见check1()函数），直接拿一个set去除掉那些已经在这行这列这个子矩阵中出现过的数，看看是否只剩下一个数。判断某行的某个数是否有唯一可行放置（详见check2()函数），直接判断这一行的9个位置中是否有位置可放，当一个位置的列、子矩阵均没出现过这个数时为可放，如果这一行有且仅有一个位置可放，则满足条件。判断某列的某个数是否有唯一可行放置（详见check3()函数），直接判断这一列的9个位置中是否有位置可放，当一个位置的行、子矩阵均没出现过这个数时为可放，如果这一列有且仅有一个位置可放，则满足条件。判断某个子矩阵的某个数是否有唯一可行放置（详见check4()函数），直接判断这一子矩阵的9个位置中是否有位置可放，当一个位置的行、列均没出现过这个数时为可放，如果这一子矩阵有且仅有一个位置可放，则满足条件。循环跑以上四个check函数，直到没有更新为止。

代码：

```cpp
#include <iostream>
#include <vector>
#include <set>
using namespace std;
#define N 10
int number[N][N], flag1[N][N], flag2[N][N], flag3[N][N];
bool check1(int x, int y)
{
    set <int> st;
    for(int i = 1; i <= 9; i++)
        st.insert(i);
    for(int i = 1; i <= 9; i++)
        if(flag1[x][i] || flag2[y][i] || flag3[x / 3 * 3 + y / 3][i])
            st.erase(i);
    if(st.size() == 1)
    {
        int t = *st.begin();
        number[x][y] = t;
        flag1[x][t] = 1;
        flag2[y][t] = 1;
        flag3[x / 3 * 3 + y / 3][t] = 1;
        return true;
    }
    else
        return false;
}
bool check2(int x, int t)
{
    vector <int> v;
    for(int i = 0; i < 9; i++)
    {
        if(number[x][i])
            continue;
        if(!flag2[i][t] && !flag3[x / 3 * 3 + i / 3][t])
            v.push_back(i);
    }
    if(v.size() == 1)
    {
        number[x][v[0]] = t;
        flag1[x][t] = 1;
        flag2[v[0]][t] = 1;
        flag3[x / 3 * 3 + v[0] / 3][t] = 1;
        return true;
    }
    else
        return false;
}
bool check3(int y, int t)
{
    vector <int> v;
    for(int i = 0; i < 9; i++)
    {
        if(number[i][y])
            continue;
        if(!flag1[i][t] && !flag3[i / 3 * 3 + y / 3][t])
            v.push_back(i);
    }
    if(v.size() == 1)
    {
        number[v[0]][y] = t;
        flag1[v[0]][t] = 1;
        flag2[y][t] = 1;
        flag3[v[0] / 3 * 3 + y / 3][t] = 1;
        return true;
    }
    else
        return false;
}
bool check4(int id, int t)
{
    vector <pair <int, int> > v;
    for(int i = id / 3 * 3; i <= id / 3 * 3 + 2; i++)
    {
        for(int j = id % 3 * 3; j <= id % 3 * 3 + 2; j++)
        {
            if(number[i][j])
                continue;
            if(!flag1[i][t] && !flag2[j][t])
                v.push_back({i, j});
        }
    }
    if(v.size() == 1)
    {
        number[v[0].first][v[0].second] = t;
        flag1[v[0].first][t] = 1;
        flag2[v[0].second][t] = 1;
        flag3[id][t] = 1;
        return true;
    }
    else
        return false;
}
bool cal()
{
    int flag = 0;
    for(int i = 0; i < 9; i++)
    {
        for(int j = 0; j < 9; j++)
        {
            if(number[i][j])
                continue;
            if(check1(i, j))
                flag = 1;
        }
    }
    for(int i = 0; i < 9; i++)
    {
        for(int j = 1; j <= 9; j++)
        {
            if(flag1[i][j])
                continue;
            if(check2(i, j))
                flag = 1;
        }
    }
    for(int i = 0; i < 9; i++)
    {
        for(int j = 1; j <= 9; j++)
        {
            if(flag2[i][j])
                continue;
            if(check3(i, j))
                flag = 1;
        }
    }
    for(int i = 0; i < 9; i++)
    {
        for(int j = 1; j <= 9; j++)
        {
            if(flag3[i][j])
                continue;
            if(check4(i, j))
                flag = 1;
        }
    }
    if(flag)
        return true;
    else
        return false;
}
int main()
{
    for(int i = 0; i < 9; i++)
    {
        for(int j = 0; j < 9; j++)
        {
            cin >> number[i][j];
            flag1[i][number[i][j]] = 1;
            flag2[j][number[i][j]] = 1;
            flag3[i / 3 * 3 + j / 3][number[i][j]] = 1;
        }
    }
    while(cal()) ;
    int flag = 1;
    for(int i = 0; i < 9 && flag; i++)
        for(int j = 0; j < 9 && flag; j++)
            if(!number[i][j])
                flag = 0;
    if(flag)
        cout << "Easy" << endl;
    else
        cout << "Not easy" << endl;
    for(int i = 0; i < 9; i++)
    {
        for(int j = 0; j < 9; j++)
        {
            if(number[i][j])
                cout << number[i][j] << " ";
            else
                cout << "." << " ";
        }
        cout << endl;
    }
    return 0;
}

```

##### E

思路：

没想到吧，这题还是道摔鸡蛋问题，如果去掉后面的输出，只看前面的最少实验次数的话，这题把数据范围改一下直接交之前的摔鸡蛋的代码也能过。现在题目要求我们输出最少的实验次数以及在满足最少试验次数的前提下第一次放置的箱子个数的范围，这里的箱子个数相当于摔鸡蛋问题第一次摔的层数，问从哪些层摔都能得到最优解，我们可以想到在摔鸡蛋问题中第三层dp枚举的是第一次摔的层数，这下只要我们算出dp[n][m]是多少之后再反向计算一遍就能知道范围了，反向计算的转移公式跟正向跑的时候一样，不需要变，我也是WA了五发之后才发现这回事……

代码：

```cpp
#include <iostream>
using namespace std;
#define N 25
#define M 5005
int dp[N][M];
int main()
{
    int n, m;
    cin >> m >> n;
    for(int i = 1; i <= m; i++)
        dp[1][i] = i;
    for(int i = 1; i <= n; i++)
        dp[i][1] = 1;
    for(int i = 2; i <= n; i++)
    {
        for(int j = 2; j <= m; j++)
        {
            int mn = 1e9;
            for(int k = 1; k <= j; k++)
                mn = min(mn, max(dp[i - 1][k - 1], dp[i][j - k]) + 1);
            dp[i][j] = mn;
        }
    }
    int l = 1e9, r = 0;
    for(int i = 1; i <= m; i++)
        if(max(dp[n - 1][i - 1], dp[n][m - i]) == dp[n][m] - 1)
            l = min(l, i), r = max(r, i);
    if(l == r)
        cout << dp[n][m] << " " << l << endl;
    else
        cout << dp[n][m] << " " << l << "-" << r << endl;
    return 0;
}

```

##### F

下周再搞，上周的E题搞出来了，原本以为是道模拟，结果是道搜索，不做不知道啊。

**组队赛第一周**

- 仝令辉
- 孟孜文
- 唐梦淇

**比赛地址**

https://vjudge.net/contest/556687#overview

**总结**
第一次组队赛，好久都没有打组队赛了，今年第一次打组队赛，然后和一个大二的师弟打比赛，其实前面打的时候还挺顺利的，做完了EF两题，但是一开始开A题浪费了很长时间，一开始是题目读错了，感觉A挺简单的，但是仔细读完题目之后直接不会了，然后就开始开D，D题也是思路没有错，但是方法错了，没有用DFS，但是感觉用循环做也能做，但是一直做不对，到最后也没有作对，就做了两个题目，之后就开始补题，补题到C题的时候，脑子里有思路，但是一直在绕圈子，总感觉按照自己想的方法太复杂了，维护的数据太杂了，看到了jyf师哥的题解之后，就清晰了很多，自己想的还没有那么的通透，没有想的彻底，所以才会一直绕圈子，没有很清晰的思路，题目还是得多思考，多总结。

**补题题目**

C

**题意**: 有一群人上电梯，每个人要去的方向一不一定相同，给出ti 和 di，ti是每个人上电梯的时刻，di 是去的方向，0 是从左往右，1 是从右往左，每个人都会在 ti + 10 时刻下电梯题目要求的是电梯最后停下的时刻。
**思路**: 双队列，记录不同方向人的信息，首先寻找最早来的人，然后寻找同方向 ti + 10 内的人，并不断更新时刻，当电梯停下的时候，在寻找最早时刻的反方向的人或者是之后到的，如果有多个乘客的话，则会一起上电梯，可以只增加一个人的时间。

``` C++
#include <bits/stdc++.h>
using namespace std;

const int N = 1e4 + 10;

int n, ans;

queue <int> q[2];

int main()
{
    cin >> n;
    for(int i = 1; i <= n; i ++)
    {
        int x, y;
        cin >> x >> y;
        q[y].push(x);
    }

    while(q[0].size() || q[1].size())
    {
        if(q[1].empty())
        {
            ans = max(ans, q[0].front()) + 10;
            q[0].pop();
            while(q[0].size())
            {
                ans = max(ans, q[0].front() + 10);
                q[0].pop();
            }
            break;
        }
        if(q[0].empty())
        {
            ans = max(ans, q[1].front()) + 10;
            q[1].pop();
            while(q[1].size())
            {
                ans = max(ans, q[1].front() + 10);
                q[1].pop();
            }
            break;
        }
        if(q[0].front() < q[1].front())
        {
            ans = max(ans, q[0].front()) + 10;
            q[0].pop();
            while(q[0].size() && q[0].front() < ans)
            {
                ans = max(ans, q[0].front() + 10);
                q[0].pop();
            }

        }
        else
        {
            ans = max(ans, q[1].front()) + 10;
            q[1].pop();
            while(q[1].size() && q[1].front() < ans)
            {
                ans = max(ans, q[1].front() + 10);
                q[1].pop();
            }
        }
    }

    cout << ans << endl;

    return 0;
}

```

D
**题意**: 有两种不同类型的练习分别是 A 和 B，但是不一定做锻炼中的所有练习，所以规则是这样的，规则是最后一场比赛必须是B，当碰到A比赛时，可以选择跳过下一场比赛直接进行下下场比赛，但是碰到B比赛时，不能跳过，构造一个练习序列，并满足给出的 N ，N 是有多少种训练方法可以完成，如果当前数据有多种构造方法，输出字典序最小的答案。

**思路**: 模拟，找规律，深搜，通过枚举几个数据可知道，只要能被斐波那契数列分解，就可以构造，否则构造不出，并且分解斐波那契因子的时候只能从较大数开始分解，否则不能满足字典序最小并且会将一些可以分解的数判断错误，例如21，21是斐波那契数.但是如果从小的开始分解的话，最后分解完的数是7，不能在进行分解，就会导致答案错误

```c++
#include <bits/stdc++.h>
using namespace std;

typedef long long LL;

const int N = 1e5 + 10;

LL  phi[100] = {0, 1, 1};
LL num[100];
map<LL, string> h;
map<LL, bool> t;
bool flag;

void dfs(LL m)
{
    if(flag) return ;

    if(m == 1)
    {
        for(int i = 75; i >= 3; i --)
        {
            for(int j = 1; j <= num[i]; j ++)
            {
                cout << h[phi[i]];
            }
        }
        flag = true;
        puts("");
        return ;
    }

    for(int i = 75; i >= 3; i -- )
    {
        if(m % phi[i] == 0 && !t[m])
        {
            num[i] ++;
            dfs(m / phi[i]);
            num[i] --;
        }
    }
    t[m] = true;
}

int main()
{
    LL x = 1, y = 1;
    string s;
    for(int i = 1; i <= 75; i ++)
    {
        LL  z = x + y;
        x = y;
        y = z;
        phi[i + 2] = z;
        s += "A";
        h[z] = s + 'B';
    }
    LL n;
    cin >> n;
    dfs(n);
    if(!flag) puts("IMPOSSIBLE");
    return 0;
}
```

C

**题意**: 有 n 个块，每个块有一个唯一序号和一个颜色，每个块可以进行互换，前提是颜色相同，问经过一系列互换之后所有的块的序号能严格单增。

**思路**: 哈希，只要每个块的序号和对应下标所对应的块的颜色相同的话，就可以进行互换，如果不一样的话就达不到单增。

```c++
#include <bits/stdc++.h>
using namespace std;

const int N = 1e5 + 10;

map<int, int> h;

int n, m;
int a[N];
int main()
{
    cin >> n >> m;
    if(m == 1) puts("Y");
    else
    {
        for(int i = 1; i <= n; i ++)
        {
            int x, y;
            cin >> x >> y;
            a[i] = x;
            h[x] = y;
        }

        bool flag = true;
        for(int i = 1; i <= n; i ++)
        {
            int t = a[i];
            if(h[a[i]] != h[a[t]]) flag = false;
        }

        if(flag) puts("Y");
        else puts("N");
    }
    return 0;
}
```

F

**题意**:一个人在飞机上喜欢睡觉，	在t时刻开始睡觉，在t + T时刻睡醒，问能不能选择一段时间睡觉，前提是在不耽误飞机餐的情况下。

**思路**: 模拟，只要相邻两个飞机餐的间隔大于 T 就可以满足题意。

```c++
#include <bits/stdc++.h>
using namespace std;

const int N = 1e5 + 10;

int t, d, m;
int a[N];
int main()
{
    cin >> t >> d >> m;

    for(int i = 1; i <= m; i ++)
    {
        cin >> a[i];
    }
    bool flag = false;
    for(int i = 1; i <= m; i ++)
    {
        if(a[i] - a[i - 1] >= t)
        {
            flag = true;
        }
    }
    if(d - a[m] >= t) flag = true;
    if(flag) puts("Y");
    else puts("N");
    return 0;
}

```

G:

**题意**: 王位继承问题，要求确认每次人去世后的继承人编号。规则如下

- 遵循直系优先于旁系规则，也就是当支系的祖先去世后
- 同一支系下，优先选择编号较小的孩子

现在有两种操作: 
1， xi：xi序号生了一个孩子。
2， xi：xi序号的人去世。

**思路**:dfs次序，从序号为1的开始进行搜索，将搜索的顺序用队列记录下来，如果去世的人是皇帝，那么就是皇帝后面的一个人继承，如果不是皇帝，那么皇帝是不变的。

```c++
#include <bits/stdc++.h>
using namespace std;

const int N = 1e5 + 10;

int n, em, cnt = 1;
vector <int> e[N], d;
queue<int> q;
bool flag[N];

void dfs(int u)
{
    q.push(u);
    for(auto x : e[u]) dfs(x);
}

int main()
{
    ios::sync_with_stdio(false);
    cin >> n;
    for(int i = 1; i <= n; i ++)
    {
        int a, b;
        cin >> a >> b;
        if(a == 1) e[b].push_back(++ cnt); 
        else d.push_back(b);
    }

    dfs(1);

    for(auto x : d)
    {
        flag[x] = true;
        while(flag[q.front()]) q.pop();
        cout << q.front() << endl;
    }

    return 0;
}
```

   

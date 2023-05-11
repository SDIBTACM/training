# 组队赛一
- 孙源
- 郭一鸣
- 王传瑞

## E题解
### **题意** ：将无序数列分组，每组间可以任意交换，最后输出能否将无序数组变成升序数组
### **题解**：先建立每个数字到组号的映射，同时记录每个位置的组号，从1-n进行遍历，判断升序状态下每个数字的组号和之前位置的组号是否相等，若都相等，则输出'Y'，其他情况输出'N'。
```cpp
#include <bits/stdc++.h>

#define MAXN 100010
#define M 1000010

using namespace std;

typedef long long ll;

map<int,int> mp;

int p[MAXN];

int main()
{
    int n,m;
    cin>>n>>m;
    for(int i=1;i<=n;i++)
    {
        int a,b;
        cin>>a>>b;
        mp[a]=b;
        p[i]=b;
    }
    int i;
    for(i=1;i<=n;i++)
    {
        if(mp[i]!=p[i])
            break;
    }
    if(i==n+1)
        cout<<"Y"<<endl;
    else
        cout<<"N"<<endl;
    return 0;
}
```
## F
### **题意** 输入某人需要休息的时间t，飞机的飞行时间d，和提供餐的次数，输出是否能在飞行过程中休息够t分钟。
### **思路** 分别计算出送餐的时间间隔，找间隔是否有超过t的。
```cpp
#include <bits/stdc++.h>

#define MAXN 100010
#define M 1000010

using namespace std;

typedef long long ll;

map<int, int> mp;
int p[MAXN];

int main()
{
    int t, d, m;
    cin >> t >> d >> m;
    int tem = 0;
    int flag = 0;
    for (int i = 1; i <= m; i++)
    {
        int ti;
        cin >> ti;
        if ((ti - tem) >= t)
            flag = 1;
        tem = ti;
        
    }
    if ((d - tem) >= t)
        flag = 1;
    if (flag)
        cout << "Y" << endl;
    else
        cout << "N" << endl;
    return 0;
}
```

## C
### **题意**：这个题的意思是有一个双向运行的电梯，每一时刻只能往一个方向运行。乘客分别在不同的时刻到来，当乘客在电梯上时在10s内同方向的乘客可以继续乘坐，不同方向的乘客只能等结束后才能乘坐，若电梯都没乘客时，电梯放向按先到的乘客。
### **思路**：用变量sum来记录所需要的时间，用一个数组分别存每一个乘客到来的时刻以及他们的方向（题目给出的方向用0，1表示），从最先到来的乘客开始每次找当前在电梯运行时间范围内到来的所有的乘客，每到来一位乘客就更新该方向电梯运行的时间，若有不同方向的乘客则用f标记。若当前放向乘客到来时间大于电梯该方向运行时间，且有不同方向的乘客等待（或当前电梯运行方向时间内的所有乘客都结束），电梯改变方向则另一方向等待的乘客消耗的时间就是10s因此就是sum+10。
```cpp
#include <bits/stdc++.h>

#define MAXN 100010
#define M 1000010

using namespace std;

typedef long long ll;

pair<int, int> p[MAXN];

int main()
{
    int n;
    cin >> n;
    for (int i = 0; i < n;i++)
    {
        int t, d;
        cin >> t >> d;
        if(!d)
            d = 2;
        p[i].first = t, p[i].second = d;
    }
    int sum = p[0].first + 10;
    int flag = p[0].second;
    int f = 0, i = 1;
    while(i<n)
    {
        if(p[i].first<=sum)
        {
            if(p[i].second==flag)
            {
                sum = p[i].first+10;
                i++;
            }
            else
            {
                f = p[i].second;
                i++;
            }
        }
        else
        {
            if(f)
            {
                sum += 10;
                flag = f;
                f = 0;
            }
            else
            {
                sum = p[i].first + 10;
                flag = p[i].second;
                i++;
            }
        }
    }
    if(f)
    {
        sum += 10;
    }
    cout << sum << endl;
    return 0;
}
```

## B
### **题意:** 
### **思路:** 某一进制B，某一数n对（B+1）求余的余数为y（即n%(B+1)==y）。所得的余数y与n的每一位的关系为：若n的偶数位减小1，则n的余数就减小1，若n的奇数位减小1，则n的余数就减小B（仅适用对进制加1的数求余）。这个题，题目中给进制B与数n的所有位，要想n%(B+1)的余数为0，只需要在n的偶数位减去(n%(b+1))，或在奇数位上减去B+1-s就好，这个题要求得到最小的n，因此从最高位开始找若是偶数位大于或等于（n%(B+1)），就直接让该位减n%(B+1)就好，或奇数位大于或等于B+1-s;
```cpp
#include <bits/stdc++.h>

#define MAXN 200010
#define M 10000010

using namespace std;

typedef long long ll;

int a[MAXN];

int read()
{
    int ans = 0, f = 1;
    char c = getchar();
    while (c < '0' || c > '9')
    {
        if (c == '-')
            f = -1;
        c = getchar();
    }
    while (c >= '0' && c <= '9')
    {
        ans = ans * 10 + c - 48;
        c = getchar();
    }
    return ans * f;
}

int main()
{
    int b, n;
    cin >> b >> n;
    int s = 0;
    for (int i = 1; i <= n; i++)
    {
        a[i] = read();
        s = ((s * b) + a[i]) % (b + 1);
    }
    if (!s)
    {
        cout << "0 0" << endl;
        return 0;
    }
    int on = b + 1 - s, en = s;
    int res = 0, i;
    for (i = 1; i <= n; i++)
    {
        if ((n - i) % 2 == 0)
        {
            if (a[i] >= en)
            {
                res = en;
                break;
            }
        }
        else
        {
            if (a[i] >= on)
            {
                res = on;
                break;
            }
        }
    }
    if (res)
    {
        cout << i << ' ' << a[i] - res << endl;
    }
    else
    {
        cout << "-1 -1" << endl;
    }
    return 0;
}
```

## G
### **题意**：所有人的编号都是从1-n，输入1，x，代表编号x的人有一个孩子出生，输入2，x，表示编号为x的人去世，下一任国王优先是该去世国王的孩子，否则就是该去世国王的兄弟，输出当每一个人去世时当前的国王。
### **思路**：利用数组tr来存储这个国王家族成员所形成的树，用数组d[]来记录国王去世顺序的编号，从第一个国王开始深搜，该家族中国王的继承顺序存到数组或队列中。最后按队列中国王的继承顺序输出没有去世的国王。
```cpp
#include <bits/stdc++.h>

#define MAXN 100010

using namespace std;

vector<int> tr[MAXN];
int d[MAXN], idx = 1, m = 0;
queue<int> q;
int vis[MAXN];

void dfs(int x)
{
    for (auto &i : tr[x])
    {
        q.push(i);
        dfs(i);
    }
}

int main()
{
    int n;
    scanf("%d", &n);
    for (int i = 0; i < n; i++)
    {
        int t, x;
        scanf("%d%d", &t, &x);
        if (t == 1)
        {
            tr[x].push_back(++idx);
        }
        else
        {
            d[++m] = x;
        }
    }
    q.push(1);
    dfs(1);
    int tem = 0;
    for (int i = 1; i <= m; i++)
    {
        vis[d[i]] = 1;
        while (!q.empty())
        {
            tem = q.front();
            if (!vis[tem])
            {
                break;
            }
            else
            {
                q.pop();
            }
        }
        printf("%d\n", tem);
    }
    return 0;
}
```


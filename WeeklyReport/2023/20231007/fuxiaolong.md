# 本周
放假在家根本学不了一点，自律基本为0，果然学习只能在学校学，回来周六一天还有一堆课，感觉啥也没干，可能是爬完泰山累的，周天在宿舍一动没动。
## 训练赛补题

A .
题意：给n个数，输出最大的数的位置和第二大的数的值，排下序就就解决了
```
#include<bits/stdc++.h>
vector<PII> a;
int cmp(PII a,PII b)
{
    return a.first > b.first;
}
int main()
{
    int t;
    cin>>t;
    while(t--)
    {
        int n;
        cin >> n;
        for (int i = 1; i <= n;i++)
        {
            int x;
            cin >> x;
            a.pb({x, i});
        }
        sort(a.begin(), a.end(),cmp);
        cout << a[0].second << ' ' << a[1].first;
        cout << endl;
        for (int i = 1; i <= n;i++)
        {
            a.clear();
        }
    }

    system("pause");
}
```
B .
题意：题目给出H，h,D,求出影子最长

思路：人从灯下向墙走，顺序枚举人到灯的距离 i，当影子没到墙上时，最大为影子正好到墙边，画图用相似三角形可以算出最大为（h/H）*D；当影子落在墙上时，可以得到人在地上的影子（假设全落在地上）为x=h/（H-h）*i，画图可以延长求出tan=（H-h)/i，
可以得到墙上的影子 q=（x-（d-i））*tan；所以总影子为q+d-i；取最大即可
```
#include<bits/stdc++.h>
int a[N];
double H, h, d;
double tanA, x;
int main()
{
    int t;
    cin >> t;
    double ans = 0;
    while (t--)
    {
        ans = 0;
        cin >> H >> h >> d;
        ans = h / H * d;
        for (double i = 0; i <= d; i += 0.001)
        {
            x = h / (H - h) * i;
            if (x <= d - h / H * d)
            {
                continue;
            }
            else
            {
                tanA = (H - h) / i;
                double q = (x - (d - i)) * tanA;
                ans = max(ans, q + d - i);
            }
        }
        printf("%.3lf\n", ans);
    }
    system("pause");
}
```
F.
用map统计一下出现次数就行了
```
#include<bits/stdc++.h>
map<string, int> mp;
int main()
{
    int n;
    cin >> n;
    while(n--)
    {
        string s;
        cin >> s;
        mp[s]++;
    }
    int k;
    cin >> k;
    while(k--)
    {
        int t;
        cin >> t;
        int ans = 0;
        while(t--)
        {
            string s;
            cin >> s;
            if(mp[s])
            {
                ans++;
            }
        }
        cout << ans << endl;
    }

    system("pause");
}
```
I.
题意:给定一些整数进入结构并从结构出来的顺序，确定它是堆栈还是队列？

思路： 用stl模拟一下就ok了
```
#include<bits/stdc++.h>
int a[N];
stack<int> st;
queue<int> q;
int main()
{
    int n;
    cin >> n;

    while (n--)
    {
        int t;
        cin >> t;
        bool f1 = 1, f2 = 1;
        for (int i = 1; i <= t; i++)
        {
            int x;
            cin >> x;
            st.push(x);
            q.push(x);
        }
        for (int i = 1; i <= t; i++)
        {
            cin >> a[i];
        }
        for (int i = 1; i <= t; i++)
        {

            if (a[i] != st.top())
            {
                f1 = 0;
                break;
            }
            st.pop();
        }
        for (int i = 1; i <= t; i++)
        {
            if (a[i] != q.front())
            {
                f2 = 0;
                break;
            }
            q.pop();
        }
        if (f1 && f2)
        {
            cout << "both" << endl;
        }
        else if (f1 && !f2)
        {

            cout << "stack" << endl;
        }
        else if (!f1 && f2)
        {

            cout << "queue" << endl;
        }
        else
        {
            cout << "neither" << endl;
        }
        while (!q.empty())
        {
            q.pop();
        }
        while (st.size())
        {
            st.pop();
        }
    }

    system("pause");
}
```
K.
题意：给定一个n*m的矩阵，要求矩阵内部的数等于它上下左右的和的数为k个。

思路：初始化矩阵为0，k最大为（n-2）*（m-2），每放置一个数，k就会减一,放到k=0，打印出来就好 了

```
#include<bits/stdc++.h>
const int N = 1e4 + 5;
const int INF = 0x3f3f3f3f;
const int sed = 131;
int a[20][20];
int main()
{
    int p;
    cin >> p;
    while (p--)
    {
        int n, m, k;
        cin >> n >> m >> k;
        memset(a, 0, sizeof(a));
        int mk = (n - 2) * (m - 2);
       if(k==mk)
        {
            for (int i = 1; i <= n; i++)
            {
                for (int j = 1; j <= m - 1; j++)
                {
                    cout << a[i][j] << ' ';
                }
                cout << a[i][m] << endl;
            }
            continue;
        }
        k = mk - k;
        for (int i = 1; i <= n; i++)
        {
            for (int j = 2; j <= m - 1; j++)
            {
                a[i][j] = k;
                k--;
                if(!k)
                {
                    break;
                }
            }
            if(!k)
            {
                break;
            }
            
            
        }
        for (int i = 1; i <= n; i++)
        {
            for (int j = 1; j <= m-1; j++)
            {
                cout << a[i][j] << ' ';
            }
            cout << a[i][m] << endl;
        }
    }

    system("pause");
}
```










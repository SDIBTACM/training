# 周报
## 本周完成
做了两周的每日一题感觉自己的思维题做的是真的烂， 做思维题感觉自己真的猪脑
当时A题就想了三个小时, 真的有被自己笨到, 后来发现只要对连续的 1 进行操作后, 最后只会剩下两种状态
A - Complementary XOR
```
const int N = 2e5+10, M = 1e9+7;

using pii = pair<int, int>;
void solve()
{
    int n; cin >> n;
    string a, b; cin >> a >> b;
    a = " " + a, b = " " + b;
    int cnt = 0;
    fr (i, 1, n) cnt += (a[i] == b[i]);
    if (cnt != n && cnt != 0) return void(cout << "NO\n");

    cout << "YES\n";
    vector<pii> ans;
    int i = 1;
    while (i <= n)
    {
        while (i <= n && a[i] == '0') ++i;
        if (i > n) break;
        int j = i;
        while (j <= n && a[j] != '0') ++j;
        ans.push_back({i, j-1});
        i = j;
    }
    if ((ans.size() + (cnt == n)) % 2 == 0)
    {
        ans.push_back({1, n});
        ans.push_back({1, 1});
        ans.push_back({2, n});
    }
    cout << ans.size() << endl;
    for (auto &it : ans) cout << it.first << " " << it.second << endl;
}
```
## 下周计划
快到期末考试了， 要把时间留出来去抓紧补课要不然又要挂科， 买的c++11的书也到了， 也要开始看了
翻了下书才发现，原来c++11新添了这么多东西， 学c++11像是学一门新语言
好几个月没有写python， 发现连算法轮子都写不出来了， 下周的每日一题试试用py写写， 重新熟悉一下， 暑假好用来写项目
玩也玩够了， 要开始学习了


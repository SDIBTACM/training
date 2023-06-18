# 周报
## 本周
 [这周每日一题的c题](https://codeforces.com/problemset/problem/1735/C)
- 题意：给26个字母调整排个环形，把单词的每个字母换成它后面的那个字母便是单词原版和新版的区别。输入新版单词，求字典序最小的原版单词。
### 题解
- 给每个字母找它前面的那个字母——父亲字母。相同的字母，它的父亲字母也相同。
- 给某个找父亲字母时，优先考虑字典序小的字母，然后检查这个父亲字母是否合法
- 检查这个父亲字母是否合法的方法：通过这个父亲向上寻找爷爷，再向上找它的...如此向上寻找，判断结果为：不能成环即合法。
- 上面检查父亲是否合法时，对于第26个字母是不成立的，因为它要承担起将这个直线连成环的责任。所以对于第26个字母，要认此时唯一不是父亲身份的为父亲。

代码
```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
using ull = unsigned long long;
#define mem(a, b) memset(a, b, sizeof(a));
#define IOS ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
const int N = 1e5 + 5;
///////////////////////////////////////////////////////////////////////////////////
map<char, char> mp;
map<char, char> vis;
bool check(char p, char s)
{
    while (p = mp[p]) if (p == s) return 1;
    return 0;
}
void solve()
{
    mp.clear(), vis.clear();
    ll n; cin >> n;
    string s; cin >> s;
    ll cnt = 0, p;
    for (int i = 0; i < s.size(); ++i)
    {
        if (mp[s[i]] == 0)
        {
            ++cnt;
            for (p = 'a'; vis[p] || p == s[i] || (cnt != 26 && check(p, s[i])); ++p);
            mp[s[i]] = p;
            vis[p] = s[i];
        }
        cout << mp[s[i]];
    }
    cout << endl;
}
int main()
{
    IOS;
    int T; cin >> T; while (T--)
        solve();
    return 0;
}
```
## 本周感想
- 形如每日一题一般使用简短的代码就能AC的题目，虽然写起来可以很快的完成，比起长篇的代码debug也要很容易，但是AC后带来的成就远不如长篇代码AC的题目。(按标答代码长度，故意写长写复杂当然不算)
- 蓝桥杯。有个广搜题看完题目直接略过，想先看看后面的题目来着，然后就忘掉了，结束前几分钟的时候想起来了but已经迟了hahahah。    判断等腰三角形个数：我用的map，等边三角形判了，三个点排在一条直线判了，虽然知道应该会有重点，但是因为想不到怎么快速的判断就没写，程序复杂度是nlogn。可气的是，虽然这个程序写完了，但是我突然想到zlk蓝桥杯省赛的成绩很大可能是蓝桥杯的编辑系统不能编辑他写的代码，于是就用dev上运行试试，结果一直报错，改来改去废了好久时间最终决定不浪费时间了(dev里的map<vector<ll>,ll>mp竟然也编辑错，然后map < vector<ll>, ll > mp这样用空格分开就编辑对了,搞我是吧)，后来听廖银师哥说才知道用dev得先调一下C++标准，害。   前缀和题：题面竟然把题意放在输入数据的范围里面，然而我没注意到这个数据范围的特性，把题目想复杂了后以我的水平也只能写个爆搜。   其他题目怎么样已经记不清楚了，但是赛后自己判题的时候是判一个错一个，不过竟然也有国二，，，太棒了就当上天奖励我的。
- 实验室晚上的校园网差的有点离谱了

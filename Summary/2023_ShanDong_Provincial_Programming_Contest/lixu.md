# 省赛报告
## 第一天
早上起床买了包子后去做高铁，在wck的建议下提前把车票的报销凭证取了出来，没想到真的有用。下车后去酒店放下包，后坐free的地铁奔赴齐鲁工大，去的时候围着学校走了好久，中间开了个乐跑。到学校报道，领完东西去吃饭（本来以为那个餐卷是在餐厅随便点饭吃的，本来想着看那个饭贵吃那个，逛了一圈后问了个阿姨才发现是领盒饭的hahah，饭还是很丰盛的，吃不了）。

之后去报告厅凉快了一会，然后去打热身赛，竟然有中文题面，有点高兴，整的忘记了怎样打组队赛了，草草的a了三道签到题后，看了看最后一题也a了。k完题想着要熟悉一下电脑系统，毕竟一开始连编辑器都不会开了，于是又操作了半个多小时的电脑。后出去等集合，然后拍照。想出去吃个饭的小心思被发现了，不知道是怎么暴露的。晚上吃完饭后回酒店早早睡了。
## 第二天
早上刚六点的时候就被wck的电话叫醒了，说是正式赛有中文题面，直接睡不着了，下楼去吃完早饭然后就奔赴比赛场地了。

### 比赛刚开始时
比赛开始后真的发了中文题面，一整个激动住了，然后还是zlk去码板子，连着签了俩签到题，中间由于加减乘除的计算出了问题，两个人检验都错到一起了，就wa了一发。然后就卡住了。后看了好几道题目发现没有思路，然后zlk看中了d题，给我说思路，我是真听不明白，愣是让zlk孤军奋战了，然后wck虽然刚开始也不信服，不过还是帮着zlk去改bug，而我不信服的去找其他题，不过好在想到了第三道题。
### 比赛中期
zlk代码写的太复杂了，wck帮忙de了好久两个人都给de麻了。中间我中了第三题，之后本来我应该和zlk、wck一起debug的，但是由于我认为榜是歪了的，坚持不看D（真出生啊，我要是一起de应该很快就能de出来，毕竟zlk的思路和题解是一模一样的，只是实现的不太好），去看了好久的其他题，看了好多题（结果还就是错过了最简单的L题），看了E题感觉k进制可以做，但是m的倍数我搞不来，结果zlk看了一会想到了搜索，直接被我给否决了（真真出生啊，不过当时还没想到区间，所以只有爆搜还是不够的）。然后我又去看其他题了，又没去搞D题。
### 比赛后期
最后两个小时，我看了K题感觉也可以做（其实不简单，赛后补了几个题，这题难度至少排第9），导致我和wck浪费掉了最后一个小时。抛下了zlk一个人想着d题。最后十分钟，三个人一起de出了D题一个bug，还超时了。赛后发现题解和zlk思路一模一样，但是zlk代码写的一直是比较复杂的，而我写的比较简单，按照平时组队赛我应该去配合zlk去写代码的，但是由于我没信服队友（真对不起队友啊），导致就只有最后的十分钟打了个配合，快速de出一个bug后还是超时了，赛后把原代码的数组标记改成vector删除就过了。怪我没看一点d题，苦了队友了。
### 赛后总结
人手一份的中文题面属实是把我方向给打乱了，导致我一直在找其他能做的题（想来平时组队赛的时候，猎题也不是我的活，结果给个中文题面就让我不会打了），而没和队友去debug，最后打了个这么差的成绩。wck和zlk比赛的时候还是和平时训练一样的，就我没稳住，没把重心放在一道题目上过，犯大错了。
### 赛后补题
D题：外层二分枚举答案，内层贪心目标是尽量背重一点的人，用二分找到目标，然后删除被背的人（赛中写的标记，结果超时了）。

代码：
~~~
#include<bits/stdc++.h>
using namespace std;
#define IOS ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
#define ll long long

int n;
pair<ll, ll> p[100005];
bool check(int V)
{
    vector<ll> se;
    for (int i = 0; i < n; i++) 
        if (p[i].second < V) 
            se.push_back(p[i].first);
    for (int i = n - 1; i >= 0; i--)
    {
        if (se.size() == 0) return 1;
        if (p[i].second < V) continue;
        ll ww = p[i].second - V + p[i].first;
        auto it = upper_bound(se.begin(), se.end(), ww);
        if (it == se.begin()) continue;
        --it;
        se.erase(it);
    }
    if (se.size()) return 0;
    return 1;
}
void slove()
{
    cin >> n;
    for (int i = 0; i < n; i++) cin >> p[i].second >> p[i].first;
    sort(p, p + n);
    int l = 1, r = 1e9;
    while (l < r)
    {
        int mid = l + r + 1 >> 1;
        if (check(mid)) l = mid;
        else r = mid - 1;
    }
    cout << l << endl;
}

int main()
{
    IOS
        int t;
    cin >> t;
    while (t--) slove();
    return 0;
}
~~~

L题：因为地图是正方形的，所以不论方块的位置在哪里，只要扩大的过程中一直让黑色的保持正方形就可以。（猎了这么多题，竟然略过了L题，我哭死）

代码：
~~~
#include<bits/stdc++.h>
using namespace std;
using ll = long long;
#define IOS ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);

int main()
{
    IOS;
    ll n, x, y; cin >> n >> x >> y;
    cout << "Yes" << endl;
    cout << n - 1 << endl;
    ll ux = x, dx = x, ly = y, ry = y;
    while (ux > 1 && ly > 1) cout << --ux << " " << --ly << " " << dx - ux << " " << ry - ly << endl;
    while (ux > 1 && ry < n) cout << --ux << " " << ++ry << " " << dx - ux << " " << ly - ry << endl;
    while (dx < n && ly > 1) cout << ++dx << " " << --ly << " " << ux - dx << " " << ry - ly << endl;
    while (dx < n && ry < n) cout << ++dx << " " << ++ry << " " << ux - dx << " " << ly - ry << endl;

    return 0;
}
~~~

E题：k进制，往左扩大一位的时候，扩大可以操作的区间，如果区间范围有一个m，那么就一定可以修改为m的倍数。往右缩小一位的时候，直接缩小k倍就行。需要注意的是，扩大后就不能缩小了，一直扩到最大就像，而区间用ll类型不够，必须用__int128才行。

代码：
~~~
#include<bits/stdc++.h>
using namespace std;
using ll = long long;
#define IOS ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);

void solve()
{
    ll n, k, m, a, b; cin >> n >> k >> m >> a >> b;
    if (n % m == 0) return void(cout << 0 << endl);
    if (k == 1) return void(cout << -1 << endl);

    ll sum = 0;
    ll mi = 0x7f7f7f7f7f7f7f7f;
    while (n)
    {
        ll ans = 0;
        __int128 l = n, r = n;
        while (!(r / m > l / m || l % m == 0 || r % m == 0))
        {
            l = k * l, r = k * r + k - 1;
            ans += a;
            if (r < l) break;
        }
        mi = min(ans + sum, mi);

        n /= k;
        sum += b;
        if (n % m == 0) mi = min(mi, sum);
    }
    cout << mi << endl;
}

int main()
{
    IOS;
    int T; cin >> T; while (T--)
        solve();

    return 0;
}
~~~

B题：一直贪心就好了，把需要同一个工种的工程编号放在一个优先队列里，当这个工种的人数扩大时就可以从头开始筛选，满足要求就给工程是条件数量-1，条件为0时就可以把这个工程的奖励加入候补名单然后挨个添加。

代码:
~~~
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
#define IOS ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);

struct node
{
    ll siz;
    vector<pair<ll,ll>> m, k;
};
queue <ll> q;
map<ll, ll> mp;
map<ll, priority_queue<pair<ll,ll>>> xq;

inline void check(ll siz, priority_queue<pair<ll, ll>>& p, vector<node>& a)
{
    while (p.size())
    {
        if (-p.top().first > siz) return;
        if (--a[p.top().second].siz == 0) q.push(p.top().second);
        p.pop();
    }
}

void solve()
{
    ll g; cin >> g; while (g--)
    {
        ll t, u; cin >> t >> u;
        mp[t] = u;
    }
    ll n; cin >> n;
    vector<node> a(n);
    for (int i = 0; i < n; ++i)
    {
        ll mi, ki; a[i].siz = 0;
        cin >> mi; while (mi--)
        {
            ll t, u; cin >> t >> u; if (mp[t] >= u) continue;
            a[i].m.push_back({ t,u }); 
            ++a[i].siz;
            xq[t].push({ -u,i });
        }
        if (a[i].m.empty()) q.push(i);
        cin >> ki; while (ki--)
        {
            ll t, u; cin >> t >> u;
            a[i].k.push_back({ t,u });
        }
    }
    ll ans = 0;
    while (q.size())
    {
        ll x = q.front(); q.pop();
        ++ans;
        for (int i = 0; i < a[x].k.size(); ++i)
        {
            mp[a[x].k[i].first] += a[x].k[i].second;
            check(mp[a[x].k[i].first], xq[a[x].k[i].first], a);
        }
    }
    cout << ans << endl;
}

int main()
{
    IOS;
    //int T; cin >> T; while (T--)
        solve();

    return 0;
}
~~~

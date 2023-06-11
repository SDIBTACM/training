# 省赛报告

+ 赛中
+ A.签到题
    + 判断当天生产完之后的货物加上之前剩余的货物能否满足当天供货要求即可。
+ I.签到题
    + 枚举每个骰子，掷出的点数，判断题目给出的条件是否在枚举范围内即可，wa了一发，少枚举了几种情况
+ G.匹配
    + 按照题意 i-j=a[i]-a[j]  将当前值与下标作差即可，差相同的可以连接，利用map加vector，就像讲解题目的那位说的一样拉一些链群（没听过），然后两两结合，输出总和。
+ 补题
+ B.建筑公司
    + 类似拓扑排序，采用优先队列，将符合条件的工程先完成，然后从队列中弹出队首分别判断是否可以完成（一开始采用类似负重越野vector存储未完成的工程的写法，显示第十四个超时）
```
struct node
{
    ll siz;
    vector<pair<ll, ll>> m, k;
};
vector<node> a(100005);
queue <ll> q;//q里面存答案
map<ll, ll> mp;
map<ll, priority_queue<pair<ll, ll>>> xq;

void check(ll siz, priority_queue<pair<ll, ll>>& p)
{
    while (p.size())
    {
        if (-p.top().first > siz) return;
        if (--a[p.top().second].siz == 0) q.push(p.top().second);
        p.pop();
    }
}

void slove()
{
    ll g; cin >> g; 
    while (g--)
    {
        ll t, u; cin >> t >> u;
        mp[t] = u;
    }
    ll n; cin >> n;
    for (int i = 0; i < n; ++i)
    {
        ll mi, ki; a[i].siz = 0;
        cin >> mi; while (mi--)
        {
            ll t, u; cin >> t >> u;
            if (mp[t] >= u) continue;
            a[i].m.push_back({ t,u });
            ++a[i].siz;//要求数量
            xq[t].push({ -u,i });
        }
        if (a[i].m.empty()) q.push(i);
        cin >> ki; 
        while (ki--)
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
            mp[a[x].k[i].first] += a[x].k[i].second;//当前工作已经做完
            check(mp[a[x].k[i].first], xq[a[x].k[i].first]);
            //xq是没做完的，每次做完后检测，偶没有新的可以做。
        }
    }
    cout << ans << endl;
}

```
+ D.负重越野
    + 二分答案+贪心，枚举所有的速度判断当前速度是否符合题意，赛中采用数组标记的方式判断是否符合题意，最终超时！赛后补题将数组标记，改为vector存储速度小于枚举速度的所有成员，可以被别人背起来就删除该成员，vector内为空符合条件。
```
pair<ll, ll> p[100005];

int n;
bool check(int V)
{
    vector<ll> se;
    for(int i=0;i<n;i++)
    {
        if(p[i].second<V)
        {
            se.push_back(p[i].first);
        }
    }
    for(int i=n-1;i>=0;i--)
    {
        if(se.size()==0) return 1;
        if(p[i].second<V) continue;
        ll ww=p[i].second-V+p[i].first;
        auto it=upper_bound(se.begin(),se.end(),ww);
        if(it==se.begin()) continue;
        --it;
        se.erase(it);
    }
    if(se.size()) return 0;
    return 1;
}
void slove()
{
    ll ma = 0;
    cin >> n;
    
    for (int i = 0; i < n; i++)
    {
        cin >> p[i].second >> p[i].first;
        ma=max(p[i].second,ma);
    }
    sort(p,p+n);
    int l = 1, r =ma;
    while (l < r)
    {
        int mid = l + r + 1 >> 1;
        if (check(mid)) l = mid;
        else r = mid - 1;
    }
    cout << l << endl;
}
```
+ E.数学问题
    + 暴力枚举所有可能，赛中考虑了暴力枚举的可能但是短时间内没想到x的处理方法就放弃了。实际上不需要考虑x，只需要暴力枚举处理之后的区间即可，只要区间内出现m的倍数即可，输出所有情况中花费最小的答案(还有一点小bug那就是需要用到__int128 不然会爆long long)。
```
void slove()
{
    ll n,k,m,a,b;
    cin>>n>>k>>m>>a>>b;
    if(n%m==0)
    {
        cout<<0<<endl;
        return ;
    }
    if(k==1)
    {
        cout<<-1<<endl;
        return ;
    }
    ll sum=0,ans;
    ll mi=0x7f7f7f7f7f7f7f7f;
    while(n)
    {
        ans=sum;
        __int128 l=n,r=n;
        while(1)
        {
            if(r/m>l/m||l%m==0||r%m==0)
                break;
            l=k*l,r=k*r+k-1;
            ans+=a;
            if(r<l) break;
        }
        mi=min(ans,mi);
        n/=k;
        sum+=b;
        if(n%m==0)
        {
            mi=min(mi,sum);
        }
    }
    cout<<mi<<endl;
}

```
+ L.曲尺
    + 简单的构造题，用l和黑方框结合成正方形，然后围着正方形的四个角向外循环，即可利用L将网格填满，不会出现不符合题目要求的情况。
  
+ 总结
    + 比赛时，没有掌握好节奏，前几道题由于过于简单，每个人都有每个人的思路，争吵了好久，浪费很多时间。
    + 由于此次是中文题面，每个人都能看懂题，导致每个人都看自己的题目，力量分散。
    + 而且由于题面好读，每个人同时开好几道题，围着几道最难的题目做了好久，D题本来有机会做出来，但各自为战debug浪费好久时间，知道是超时之后已经没时间改代码了。
    + 通过这次比赛可以看出来，非常基础的一些思想和算法掌握的太不牢固了，做的题目太少，平时很多算法思想用不到就会生疏，导致忽然遇到一道题就想不起来该用什么算法。
    + 看了一会验题大佬的验题过程，太牛了，我们遇到的很多坑，他们直接就想到了。

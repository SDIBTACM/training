# 比赛总结
## 做出来的题
### A
开始读错题了,英语是硬伤
#### 思路
满足长度大于1并且元素都为a并且不能向左右两边扩展(扩展后不满足元素都为a)的子字符串的长度和.
#### 代码
```cpp
void solve()
{
  int n,ans=0;
  string s;
  cin>>n>>s;
  s+='1';
  int x=0;
  for(auto &i:s)
  {
    if(i=='a')x++;
    else
    {
      if(x>1)ans+=x;
      x=0;
    }
  }
  cout<<ans<<endl;
}
```
### B
看起挺难的,其实还好,就是写起来有些地方要注意
#### 思路
将线也看成一个格子,这样就能给出坐标进行搜索了
x,y 最大为1000,将所有线都看成格子够  坐标范围0-2000(**题意坐标从0开始**)
每次输入的坐标x,y 经过处理后都为 2x,2y
然后为切割的线标记为1  矩形的边界标记为0
对每个格子进行bfs搜索有多少格子能连在一起,能搜到边界则不考虑,找出最大的即可(**格子的坐标始终为奇数,统计的时候注意下**)
**还有就是题目给的x,y跟平时代码习惯里的x,y是反的,参考代码输入那里**
#### 代码
```cpp
int g[2005][2005];
int vis[2005][2005];
int dir[4][2] = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
int bfs(int &x, int &y)
{
  g[x][y]=1;
  int ans, dx, dy, flag = 0;
  queue<pair<int, int>> q;
  q.push({x, y});
  pair<int, int> p;
  if(x%2&&y%2)ans=1;
  while (q.size())
  {
    p = q.front();
    q.pop();
    g[p.first][p.second] = 1;
    for (int i = 0; i < 4; ++i)
    {
      dx = p.first + dir[i][1], dy = p.second + dir[i][0];
      if(dx<0||dx>2000||dy<0||dy>2000)continue;
      if (g[dx][dy] == 1)
        continue;
      if (g[dx][dy] == -1)
      {
        flag = 1;
        continue;
      };
      q.push({dx, dy});
      g[dx][dy]=1;
      if(dx%2&&dy%2)ans++;
    }
  }
  return flag ? 0 : ans;
}
void solve()
{
  int t, n = 1000, m = 1000, s, e;
  cin >> t;
  int x0, y0, x1, y1;
  cin >> x0 >> y0;
  x0 = 2 * x0, y0 = 2 * y0;
  for (int i = 0; i <= n * 2; ++i)
    g[i][0] = g[i][2 * m] = -1;
  for (int i = 0; i <= 2 * m; ++i)
    g[0][i] = g[2 * n][i] = -1;
  for (int i = 1; i <= t; ++i)
  {
    cin >> x1 >> y1;
    x1 = 2 * x1, y1 = 2 * y1;
    if (x0 == x1)
      for (s = min(y0, y1), e = max(y0, y1); s <= e; s++)
        g[s][x0] = 1;
    else
      for (s = min(x0, x1), e = max(x0, x1); s <= e; s++)
        g[y0][s] = 1;
    x0 = x1, y0 = y1;
  }

  int ans = 0;
  for (int i = 0; i <= 2 * n ; ++i)
    for (int j = 0; j <= 2 * m; ++j)
    {
      if (g[i][j] == 0)
        ans = max(ans, bfs(i, j));
    }
  cout << ans << endl;
}
```
### C
看起来挺难的 ,但是感觉这个题出的时候就有漏洞
#### 思路
不管对于坐标x还是y,它都只能选择加上0 或者2^n再除2;
所以x或y二进制里最低位的1每次都会右移,题目又说了有解,
那么每次操作后不会出现最低位的1移除第0位(1>>1=0不存在)
所以最小的次数就是abs(lobit(x)-n+1)(x或者y的最低位,有解,那么他们的最低位肯定一样)
#### 代码
```cpp
void solve()
{
  int n,x,y,a,b;
  cin>>n>>x>>y;
  int t=0,ans=0;
  while((x&1)!=1)
  {
    x>>=1;
    t++;
  }
  cout<<abs(t-n+1);
}
```
### D
#### 思路
用一个数组a记录 然后正序遍历
对于一个气球高度为x,则查看a[x+1]是否为0,为0代表当前气球之前没有高度为x的箭,需要新射一支箭.a[x]++;
a[x+1]不为0,则当前气球可由击破了x+1高度的箭击破.a[x]++,a[x+1]--;
#### 代码
```cpp
unordered_map<int,int>mp;
void solve()
{
  int n,ans=0;
  int x;
  cin>>n;
  for(int i=1;i<=n;++i)
  {
    cin>>x;
    if(mp[x+1])
    {
      mp[x+1]--;
      mp[x]++;
    }
    else mp[x]++;
  }
  for(auto &i:mp)
  {
    ans+=i.second;
  }
  cout<<ans<<endl;
}
```
### E
#### 思路
mp统计每种可能出现的次数就行
#### 代码
```cpp
map<string,int>mp;
void solve()
{
  string s,ans;
  int n,m,pos,maxx=0;
  cin>>n>>m;
  for(int i=1;i<=n;++i)
  {
    cin>>s;
    pos=s.find('*');
    for(char j='a';j<='z';++j)
    {
      s[pos]=j;
      mp[s]++;
    }
  }
  for(auto &i:mp)
  {
    if(i.second>maxx)maxx=i.second,ans=i.first;
  }
  cout<<ans<<' '<<maxx<<endl;
}
``` 
### F
#### 代码
```cpp
void solve()
{
  int t=0,x;
  for(int i=1;i<=9;++i)
  {
    cin>>x;
    if(x==9)t=1;
  }
  puts(t?"F":"S");
}
```
### G
#### 思路
根据题意分类讨论,分辨判断jack 和mary的分数大小,以及出现分数加上10小于23的情况.每种数值的牌只有四张,统计一下次数.
#### 代码
```cpp
int a[15];
void solve()
{
  int n,x,jk=0,my=0;
  cin>>n;
  cin>>x;jk+=min(x,10),a[x]++;
  cin>>x;jk+=min(x,10),a[x]++;
  cin>>x;my+=min(x,10),a[x]++;
  cin>>x;my+=min(x,10),a[x]++;
  for(int i=1;i<=n;++i)
  {
    cin>>x;
    a[x]++;
    my+=min(x,10);
    jk+=min(x,10);
  }
  if(jk<=my)
  {
    if(23-my==10)
    {
      for(int i=10;i<=13;++i)if(a[i]<4)
      {
        cout<<i<<endl;
        return ;
      }
      cout<<-1<<endl;
    }
    else if(23-my<10&&a[23-my]<4)cout<<23-my<<endl;
    else cout<<-1<<endl;
  }
  else
  {
    for(int i=23-jk+1;my+i<=23&&i<10;++i)
    {
      if(a[i]<4)
      {
        cout<<i<<endl;
        return ;
      }
    }
    for(int i=10;jk+10>23&&my+10<=23&&i<=13;++i)
    {
      if(a[i]<4)
      {
        cout<<i<<endl;
        return ;
      }
    }
    cout<<-1<<endl;
  }
}
```
## 补题
### H
补题时删除set的元素,一直是erase(st.begin())或者earse(val),记得是有这两个重载的,样例跑了30个,然后就过不去了.
看了题解代码,每次删除都传迭代器就ac了:erase(st.find(val))  有点疑惑.
#### 思路
用两个set维护,一个存选中了但未翻面的卡牌,一个记录选中且翻面的卡牌.
初始时先从一侧k个,然后选择l个最大的翻面.后面再按照顺序删除一个,加一个,记录最大可能得答案.
可以按照循环的思路,在原数组后面再加一个原数组:123->123123 这样如果k为2  这样遍历起来方便些,就是费空间.
#### 代码
代码里重载了比较运算符,不重载也行,用rebegin()找最大值,还有就是代码里的l,k表达的意思与题目相反
```cpp
int ft[200005], bk[200005];
class cmp
{
public:
  bool operator()(const int &a,const int &b)
  {
    return a>b;
  }
};
multiset<int,cmp> pq;
multiset<int> st;
void solve()
{
  int n;
  cin >> n;
  for (int i = 1; i <= n; ++i)
  {
    cin >> ft[i];
    ft[i + n] = ft[i];
  }
  for (int i = 1; i <= n; ++i)
  {
    cin >> bk[i];
    bk[i + n] = bk[i];
  }
  ll sum = 0, l = 0, k = 0, ans = 0;
  int t;
  cin >> l >> k;
  for (int i = n - l + 1; i <= n; ++i)
  {
    sum += ft[i];
    pq.insert(bk[i]);
  }
  while (st.size() < k)
  {
    t = *pq.begin();
    sum += t;
    st.insert(t);
    pq.erase(pq.find(t));
  }
  ans = sum;
  for (int i = n + 1; i <= n + l; ++i)
  {
    sum += ft[i] - ft[i - l];
    if (st.find(bk[i - l]) != st.end())
    {
      sum -= bk[i - l];
      st.erase(st.find(bk[i - l]));
      pq.insert(bk[i]);
      t = *pq.begin();
      sum += t;
      st.insert(t);
      pq.erase(pq.find(t));
    }
    else
    {
      pq.erase(pq.find(bk[i - l]));
      if ((*st.begin()) < bk[i])
      {
        t = *st.begin();
        sum -= t;
        pq.insert(t);
        st.erase(st.find(t));
        st.insert(bk[i]);
        sum += bk[i];
      }
      else
        pq.insert(bk[i]);
    }
    ans = max(ans, sum);
  }
  cout << ans << endl;
}
```

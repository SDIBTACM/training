# 题解
## A
*略*
## B

### 题目描述
给你长度为n的数组a，要求还原出b数组，使得ai=i%b1+i%b2+....+i%bm

### 思路
如果bj>i，则bj就能贡献i
如果bj=i,则贡献为0
如果bj<i，则贡献为i%b~
所以枚举i，判断bj在此时是否存在贡献为0的情况，就可以找出bj的值.
并且通过a1确定b数组的长度m，因为任何数mod1=0,1对大于1的数取模=1。
```h
void solve()
{
  int n,x,y,b;
  cin>>n;
  vector<int>a;
  cin>>x;
  cout<<x<<' ';
  b=x;
  for(int i=2;i<=n;++i)
  {
    cin>>y;
    int sum=0;
    for(auto j:a)sum+=i%j;
    int t=a.size();
    x=(b-t)*i+sum;
    if(x==y)continue;
    x=(x-y)/i;
    for(int j=1;j<=x;++j)a.push_back(i);
  }
  for(auto i:a)cout<<i<<' ';
  cout<<endl;
}
```
## C
### 题目描述
给你一个n,让你找出a,b,满足a,b,n构成直角三角形时符合两两互质的数量和不两两互质的数量
### 思路
n不大，可以直接枚举
1. n为斜边时，a，b一定小于n 枚举a，然后根据勾股定理找出是否存在b以及是否满足题目条件
2. n不为斜边，根据勾股定理可得a^2-b^2=n^2,平方差一下(a-b)*(a+b)=n^2,枚举n^2的因子即可找出a，b，由于n不大所以直接dfs搜索 再去一下重即可
```h
void dfs(int pos, ll t)
{
  // if (mp[{pos, t}])
  //   return;
  // mp[{pos, t}] = 1;
  if (t >= n)
    return;
  ll a = sum / t,b=t;
  if (a > b)
    swap(a, b);
  if ((b + a) % 2 == 0&&mp1[{a,b}]==0)
  {
    mp1[{a,b}]=1;
    ll x = b + a >> 1, y = b - x;
    if (__gcd(n, x) == 1 && __gcd(n, y) == 1 && __gcd(x, y) == 1)
      sum3++;
    else
      sum4++;
  }
  for (int i = pos + 1; i < fac.size(); ++i)
  {
    dfs(i, t * fac[i]);
  }
}
void f()//n不为斜边
{
  int N = n * n;
  for (int i = 2; i * i <= N; ++i)
  {
    if (N % i == 0)
    {
      while (N % i == 0)
        sum *= i, N /= i, fac.push_back(i);
    }
  }
  if (N > 1)
  {
    sum *= N;
    fac.push_back(N);
  }分解素因子


  dfs(-1, 1);//枚举因数
}
void solve()
{
  int sum1 = 0, sum2 = 0;
  cin >> n;
  for (ll i = 1; i < n; ++i)
  {
    ll x = n * n - i * i, y = sqrt(x);
    if (y * y == x && y < n && y > i)
    {
      if (__gcd(n, i) == 1 && __gcd(n, y) == 1 && __gcd(n, y) == 1)
        sum1++;
      else
        sum2++;
    }
  }//n为斜边
  f();
  cout << sum1 << ' ' << sum2 << ' ' << sum3 << ' ' << sum4 << endl;
}
```
## D
### 题目描述
略
### 思路
找公式计算墙数，蜂室数。
总的蜂室为偶数时，可以保证每个蜂室都只缺一面墙，为奇数时就会有一个蜂室缺两面墙
```h
void solve()
{
  ll n,m,x,y;
  cin>>m>>n;
  x=(n+1)/2*4*m+(n+1)/2*(m+1)+n/2*m;
  if(n%2==0)x+=2*m-2;
  y=(n+1)/2*m+n/2*(m-1);
  y=(y+1)/2;
  cout<<x-y+1<<endl;
}
```
## E
### 题目描述
给你旅行天数，以及中途需要经过的营地数量，从一个营地到下一个营地需要一天，并且只能在有绿洲的营地休整(一次一天)，问你有多少休整的方案。
### 思路
数据不大，直接dfs加记忆化
mp[i][j]表示剩余的可修整营地数量，和可休整的天数
```h
ll mp[25][61];
//nowpos,现在在第nowpos个可休整营地，day剩余可休整的天数,pos 总的可休整的营地数
ll dfs(int nowpos, int day, int &pos)
{
  if (pos == nowpos)//到达终点营地
  {
    return 1;
  }
  if(~mp[pos-nowpos][day]!=0)return mp[pos-nowpos][day];//以及遍历过这种情况了，直接返回答案
  if (day == 0)//没有可休整天数了，只能直接走到终点，方案唯一
  {
    return  mp[pos-nowpos][day]=1;
  }
  ll ans = 0;
  for (int i = 0; i <= day; ++i)
  {
    ans += dfs(nowpos + 1, day - i, pos);
  }
  return mp[pos-nowpos][day]=ans;
}
void solve()
{
  memset(mp, -1, sizeof mp);
  int n, m;
  cin >> n >> m;
  vector<int> a(n + 1, 0);
  for (int i = 1; i <= n; ++i)
    cin >> a[i];
  while (m--)
  {
    int pos, d;
    cin >> pos >> d;
    pos++;
    cout << dfs(1, d - a[pos - 1], pos) << '\n';
  }
}
```
## F
### 题目描述
略
### 思路
数据不大,暴力枚举每一种可能！
```h
int n;
vector<pair<string, string>> a(25);
map<int, int> mp;
bool check(vector<string> &v)//判断是否可行
{

  for (int i = 0; i <= 3; ++i)
  {
    mp.clear();
    for (int j = 0; j < 3; ++j)
      mp[v[j][i]] = 1;
    if (mp.size() != 1 && mp.size() != 3)
      return 0;
  }
  return 1;
}
long long dfs(int pos, vector<string> &v)
{
  if(n-pos+1+v.size()<3)return 0;//不可能存在可行方案，剪枝
  if(v.size()>3)return 0;
  if (pos == n + 1)
  {
    return check(v);
  }
  ll ans = 0;
  ans+=dfs(pos+1,v);//不选
  v.push_back(a[pos].first);//选第一个
  ans += dfs(pos + 1, v);
  v.pop_back();
  v.push_back(a[pos].second);//选第二个
  ans += dfs(pos + 1, v);
  v.pop_back();
  return ans;
}
void solve()
{
  cin >> n;
  vector<string> v;
  for (int i = 1; i <= n; ++i)
    cin >> a[i].first >> a[i].second;
  cout << dfs(1, v)/2 << endl;//枚举了重复的 /2
}
```
## G
### 题目描述
删除回路的情况
 例如 a!b!a!c  删完 a!c
 a!a!b!b!c  删完 a!b!c
 a!a   删完a!a  最后一个不可删
### 思路
反复遍历检查是否存在回路的情况，存在就删，不存在就说明最简了
```h
bool check(string &s1, string &s2)
{
  if (s1.size() != s2.size())
    return 0;
  for (int i = 0; i < s1.size(); ++i)
    if (s1[i] != s2[i] && abs(s1[i] - s2[i]) != 32)
      return 0;
  return 1;
}
void solve()
{
  string s, s1;
  cin >> s;
  vector<string> a;
  for (auto &i : s)
  {
    if (i == '!')
    {
      a.push_back(s1);
      s1.clear();
    }
    else
      s1.push_back(i);
  }
  // a.push_back(s1);
  a.push_back("000");
  for (int i = 1; i < a.size() - 1; ++i)
  {
    if (check(a[i - 1], a[i]))
    {
      a.erase(a.begin() + i);
      i = 0;
    }
    else if (check(a[i - 1], a[i + 1]))
    {
      a.erase(a.begin() + i + 1);
      a.erase(a.begin() + i);
      i = 0;
    }
  }
  a.pop_back();
  a.push_back(s1);
  cout << a[0];
  for (int i = 1; i < a.size(); ++i)
    cout << '!' << a[i];
  puts("");
}
```

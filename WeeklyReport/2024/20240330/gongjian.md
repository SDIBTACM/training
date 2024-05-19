 # 选拔赛3翻译 
  ## G - Excitation of Atoms 
  Chanek先生目前正在参加一个在镇上很受欢迎的科学博览会。他在博览会上发现了一个令人兴奋的谜题，并想解决它。 
  有 N原子编号从 1 到 N。这些原子特别古怪。最初，每个原子都处于正常状态。每个原子都可以处于激发状态。激发原子 i 需要 Di 能量。当原子 i 兴奋时，它会给 Ai能源。您可以激发任意数量的原子（包括零）。这些原子也形成一种特殊的单向键。对于每个 i，（1≤i<N），如果原子i被激发，原子Ei也将被无成本激发。最初，Ei = i+1。请注意，原子 N不能与任何原子形成键。 
  
   Chanek 先生必须完全改变 K债券。正好是 K 次，Chanek 先生选择了一个原子 i （1≤i<N），并将 Ei 更改为 i 和当前 Ei 以外的其他值.请注意，原子的键可以保持不变或多次更改。帮助Chanek先生确定他可以达到的最大能量！ 
  
   注意：您必须首先将 K 完全更改在你开始激发原子之前键合。 
  ## F-ABCAB 
  对于长度为 N 的字符串 S 和整数 i （0≤i≤N），我们将字符串 fi（S）定义为以下项的串联：  
  S的前i个字符， 
  S的逆转，以及 
  S的最后（N−i） 字符， 
  按此顺序。例如，如果 S= abc 且 i=2，则 fi（S）= abcbac。 
  您将获得一个长度为 2N 的字符串 T。求一对长度为 N 的字符串 S 和整数 i （0≤i≤N），使得 fi（S）=T。如果不存在这样的 S 和 i 对，请报告该事实。 
  
  # 补题 
  ## B 
  ```c++ 
  #include <bits/stdc++.h> 
  using namespace std; 
  int a[10000]; 
  void intn (int n) 
  { 
  for (int i=1;i<=n;i++) 
  a[i]=i; 
  } 
  int find (int n) 
  { 
  if (n==a[n]) 
  return n; 
  else{ 
  a[n]=find(a[n]); 
  return a[n]; 
  } 
  } 
  void unionn (int x,int y) 
  { 
  int ax=find(x); 
  int ay=find(y); 
  a[ax]=ay; 
  } 
  int main() 
  { 
  int n,m,sum=0; 
  cin>>n>>m; 
  intn(n); 
  for (int i=0;i<m;i++){ 
  int x,y; 
  cin>>x>>y; 
  unionn(x,y); 
  } 
  for (int i=1;i<=n;i++){ 
  if (i==a[i]) 
  sum++; 
  } 
  cout<<sum<<endl; 
  } 
  ```
## C 
  ```c++ 
  #include <bits/stdc++.h> 
  using namespace std; 
     long long f(long long n) 
  { 
  long long sum1=0,sum2=0,i=1; 
  while(n>4){ 
  if (n%4==0&&i%2==1){ 
  n-=1; 
  sum1++; 
  i++; 
  continue; 
  } 
  if (n%4==0&&i%2==0){ 
     n-=1; 
  sum2++; 
  i++; 
  continue; 
  } 
  if (n%2==0&&n%4!=0&&i%2==1){ 
  sum1=sum1+n/2; 
  n=n/2; 
  i++; 
  } 
  if (n%2==0&&n%4!=0&&i%2==0){ 
  sum2=sum2+n/2; 
  n=n/2; 
  i++; 
  continue; 
  } 
  if (n%2!=0&&i%2==1){ 
  sum1++; 
  n-=1; 
  i++; 
  continue; 
  } 
  if (n%2!=0&&i%2==0){ 
  sum2++; 
  n-=1; 
  i++; 
  continue; 
  } 
     } 
  if (n<=4){ 
  while(n>0){ 
  if (n%2!=0&&i%2==1){sum1++;n=n-1;i++; continue;} 
  if (n%2!=0&&i%2==0){sum2++;n=n-1;i++; continue;} 
  if (n%2==0&&i%2==1){sum1=sum1+n/2;n=n/2;i++; continue;} 
  if (n%2==0&&i%2==0){sum2=sum2+n/2;n=n/2;i++; continue;} 
  } 
  } 
  return sum1; 
  } 
  int main() 
  { 
  int t; 
  cin>>t; 
  while(t--){ 
  long long n; 
  cin>>n; 
  cout <<f(n)<<endl; 
     } 
  return 0; 
  } 
  ```
## E 
  ```c++ 
  #include <bits/stdc++.h> 
  using namespace std; 
  #define max 1000000 
  typedef long long ll; 
  int vis[max];vector <int>a[max]; 
  int sum,t; 
  void dfs(int x) 
  { 
  sum++; 
  if (sum>=1e6){ 
  t=1; 
  return ; 
  } 
  for (ll i=0;i<a[x].size();i++){ 
  if (vis[a[x][i]]==0){ 
  vis[a[x][i]]=1; 
  dfs(a[x][i]); 
  vis[a[x][i]]=0; 
  } 
  } 
  
   } 
  int main() 
  { 
  int n,m; 
  cin>>n>>m; 
  for (ll i=0;i<m;i++){ 
  int x,y; 
  cin>>x>>y; 
  a[x].push_back(y); 
  a[y].push_back(x); 
  } 
  vis[1]=1; 
  dfs(1); 
  if (t==1) 
  sum=1e6; 
  cout<<sum<<endl; 
  return 0; 
  } 
  ``` 
  # 本周 
  ## 学习了并查集并解决了B题，学习了vector解决了E题，学习了树。 
  # 下周 
  ## 进一步学习图，和整理选拔赛的题。 
 

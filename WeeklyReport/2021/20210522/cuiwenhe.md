##  周报

- 本周整理了一下部分代码

  对于读入空格但以换行符结束(运用scanf("%[ ^\n]"));

  组合数；ll Sqrt(ll x)
  {
      ll l = -1,r = 1e10,mid;
      while(r-l > 1)
      {
          mid = (l+r)>>1;
          if(mid*mid > x)
              r = mid;
          else if(mid*mid < x)
              l = mid;
          else
              return mid;
      }
      return l;
  }

  交换ab的值：a ^= b ^= a ^= b;//第一种
  swap(a,b)//第二种

  运用中间变量t：t=a;a=b;b=t;

  部分实用定义：

  ```c++
  #include <bits/stdc++.h>
  using namespace std;
  #define mem(a, b) memset(a, b,sizeof(a))
  #define PI acos(-1)
  typedef long long ll;
  const int INF = 0x3f3f3f3fLL;
  const long long LLF = 0x3f3f3f3f3f3f3f3fLL;
  const int maxn = 1e6 + 15;
  const int mod = 1e9 + 7;
  ```

- 下周会继续补背包的题目和周末比赛的题目，也会加强对其他算法的学习，算法还是自己的弱项，在思考问题方面还有很大的不足
- 对c++还是不是很了解，会尽量缩小与队友之间的差距，会努力的
### 2020.9.6 周报

## 本周学习计划
- [x] `学习RQM算法`

### RQM
RMQ ( Range Minimum / Maximum Query ) 问题是指：对于长度为 n 的数列 A，回答若干询问 RMQ (A , i , j ) ( i , j ≤ n)，返回数列A中下标在 i , j 里的最小（大）值，也就是说，RMQ问题是指求区间最值的问题。
RMQ有多种求解方式 主要学习了了ST法
核心是它的转移方程**dp[i][j] = min(dp [i][j - 1], dp [i + (1 << j - 1)][j - 1])**


####实现代码

```c++

void rmq_init()
{
    for(int i=1;i<=N;i++)
        dp[i][0]=arr[i];//初始化
    for(int j=1;(1<<j)<=N;j++)
        for(int i=1;i+(1<<j)-1<=N;i++)
            dp[i][j]=min(dp[i][j-1],dp[i+(1<<j-1)][j-1]);
}


int rmq(int l,int r)//查找
{
    int k=log2(r-l+1);
    return min(dp[l][k],dp[r-(1<<k)+1][k]);
}
```
###下周计划
**1.掌握动态规划**
**2.学习KMP**
# 第一次组队赛
王智炜、林  涛、马宝运、周  东  
## E 题[题目链接](https://vjudge.net/contest/556687#problem/E)
### 输入
第一行两个整数，n 和 m ，表示有 n 个数字和 k 种颜色  
下面有n行，每行两个整数 ni, ci，1<=ni<=n，1<=ci<=k  
ni 在 2~n+1 行中仅出现一次，ci 是 ni 的颜色  
### 输出
输出**Y**或者**N**
### 样例
样例1  
4 2  
3 1  
4 2  
1 1  
2 2  
输出：`Y`  
  
样例2  
4 2  
2 1  
4 2  
1 1  
3 2  
输出：`N`  
### 题意
样例 2~n+1 行，颜色相同的数字可以调换位置，颜色不同的数字不能调换位置  
删除输入的第一行，若 ni==i（行），输出**Y**，否则输出**N**  
例如样例1可以变为  
```
1 1  
2 2  
3 1  
4 2  
```
样例1输出Y  
样例2不可以变为以上形式，故输出N
### 思路
```
struct{
    int n,c;
}q[100005];
```
将数字与颜色存入数组，对相同颜色的数字排序，最后检查是否 ni==i 。时间超限。  
优化：存入数组后，检查 q[ni] 的颜色是否与 ni 相同，若不同输出N，若全部相同输出Y  
### AC代码
```
#include <stdio.h>

typedef struct{
    int a,b;
}Q;

int main(){
    Q q[100005];
    int n,m;
    int i,j,k;
    scanf("%d%d",&n,&m);
    for(i=1;i<=n;i++){
        scanf("%d%d",&q[i].a,&q[i].b);
    }
    for(i=1;i<=n;i++){
        if(q[q[i].a].b!=q[i].b){
            break;
        }
    }
    if(i<=n){
        puts("N");
    }
    else{
        puts("Y");
    }
    return 0;
}
```
  
## F 题 [题目链接](https://vjudge.net/contest/556687#problem/F)
### 输入
第一行三个整数 t,d,m ，  
1<=t,d<=10<sup>5</sup>,0<=m<=1000，  
t 在飞机上睡觉的时长，d 是飞机飞行的时长，m 是在飞机上吃饭的次数，时间从0开始  
下面有 m 行，表示吃饭的时间点，吃饭时瞬间完成的
### 输出
Y 或 N  
### 样例
#### Sample 1
3 10 3  
2  
4  
7  
输出：Y  
#### Sample 2
4 10 3  
2  
4  
7  
输出：N
#### Sample 3
5 5 0  
输出：Y
### 题意
睡觉时间时连续的，若吃饭与睡觉能兼得，则输出**Y**，否则输出**N**  
### 思路
将吃饭时间点存入数组，再存入下飞机的时间点，检查两时间点的间隔是否能够睡觉，若能输出Y，若所有时间间隔都不能够睡觉，输出N  
### AC代码
```
#include <stdio.h>
#include <algorithm>
using namespace std;

int main(){
    int t,d,m;
    int x[1005]={0},i;
    scanf("%d%d%d",&t,&d,&m);
    for(i=1;i<=m;i++){
        scanf("%d",&x[i]);
    }
    x[i]=d;
    sort(x+1,x+m+1);
    for(i=1;i<=m+1;i++){
        if(x[i]-x[i-1]>=t){
            break;
        }
    }
    if(i<=m+1){
        puts("Y");
    }
    else{
        puts("N");
    }
    return 0;
}
```

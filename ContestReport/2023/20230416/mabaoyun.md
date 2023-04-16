# A
## 读题
输入 每行给出三个单词，第一个无视，第二个单词是已经有的物品，第三个单词是想要的物品
要求 每人每个has和want相同的单词连成环

## 思路
1.定义一个三维数组`x[100][3][20]`, 100人，每人3个单词，每个单词19字符
2.定义二维数组`vis[100][3]`,对应上面数组的每个单词，每项的初始值为-1
3.vis数组 has存相同单词的want位置，want一样
4.遍历判断每个单词是否能成环，若能，ans++
5.输出

## 代码
请问哪里错了，自己测试没有问题，用[qoj](https://qoj.ac/problem/3614?locale=zh-cn)的“下发文件下载”的测试数据也没有问题
```
#include <stdio.h>
#include <string.h>

int main(){
    char s[100][3][20];
    char vis[100][3];
    int i,j,k,a,flag;
    int n,ans=0;
    for(i=0;i<300;i++){
        vis[0][i]=-1;
    }
    scanf("%d",&n);
    for(i=0;i<n;i++){
        for(j=0;j<3;j++){
            scanf(" %s",s[i][j]);
        }
    }
    for(i=0;i<n;i++){
        for(j=0;j<n;j++){
            if(vis[j][1]!=-1){
                continue;
            }
            if(strcmp(s[i][2],s[j][1])==0){
                vis[i][2]=j;
                vis[j][1]=i;
                break;
            }
        }
    }
    for(i=0;i<n;i++){
        if(vis[i][1]==-1||vis[i][2]==-1){
            continue;
        }
        a=vis[i][2];
        flag=0;
        while(1){
            if(a==i){
                break;
            }
            if(vis[a][2]==-1){
                flag=1;
                break;
            }
            a=vis[a][2];
        }
        if(flag==0){
            ans++;
        }
    }
    if(ans==0){
        puts("No trades possible");
    }
    else{
        printf("%d\n",ans);
    }
    return 0;
}
```

# 周报

## 本周总结

做了一套英语四级卷子，体测，周末打比赛

## 本周感想

做了英语四级卷子，意识到自己的差距很大，一这个状态是肯定过不了的。所以学习英语还是不能放松。数字逻辑，线代作业没有跟上，老师没让交就松懈了。物理又开了新课，落下了很多。跑个1000就累的不行，还是要多锻炼。。。

## 下周计划

1.坚持背英语
2.复习学过的算法
3.补上各科作业，学习物理
4.坚持锻炼
#include<stdio.h>
#include<string.h>
#include<iostream>
#include<algorithm>
using namespace std;

bool cmp(int a,int b)
{
    return a>b;
}

int main()
{
    int p,i;
    scanf("%d",&p);
    for(i=1;i<=p;i++)
    {
        int n,count=0,ttt=0,j,k;
        char a[100],b[100];
        scanf("%d ",&n);
        cin.getline(a,100);
        strcpy(b,a);
        n=strlen(a);
        for(j=n-1;j>=0;j--)
        {
            for(k=0;k<count;k++)
            {
                if(a[j]<b[k])
                {ttt=1;break;}
            }
            if(ttt==1)
                break;
            b[count]=a[j];
            count++;
        }
        printf("%d ",i);
        if(ttt==0)
        {printf("BIGGEST\n");continue;}
        ttt=a[j];
        a[j]=a[n-1-k];
        a[n-1-k]=ttt;
        for(k=0;k<=j;k++)
            printf("%c",a[k]);
        for(k=n-1;k>j;k--)
            printf("%c",a[k]);
        printf("\n");
    }
    return 0;
}


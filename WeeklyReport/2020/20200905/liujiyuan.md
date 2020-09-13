qw## 周报
    呆在家大半年，一脸懵的就大二了，想到大学生活的四分之一就这么过去了，有点后怕，可能是自己的付出太少吧。
    19级重新分了队，师哥让每队选一个算法进行学习，由于自己知识不够吧，面对各种眼花缭乱的算法，半个多周过去了才决定选后缀数组，并进行了初步了解。
    大二课程明显比大一多了不少，上课时总是难以坚持听课，导致课后含泪复习来完成作业。
    这周背英语单词的强度提高了，不对第一次四级考试抱有希望，算是为以后考四级打基础吧，英语还是难。
#include <stdio.h>
int main()
{
    long long i,t,a[100001],j,h,b[100001],k,m=0,n,p=0;
    scanf("%lld",&t);
    for(i=1;i<=t;i++)
    {
        m=0;p=0;
        scanf("%lld",&n);
        for(j=1;j<=n;j++)
        {
            b[j]=j;
            scanf("%lld",&a[j]);
        }
        for(j=1;j<=n-1;j++)
        {
            k=j;
            for(h=j;h<=n;h++)
                if(a[h]<a[k])
                {
                    k=h;
                }
            t=a[j];a[j]=a[k];a[k]=t;
            t=b[j];b[j]=b[k];b[k]=t;
        }
        for(j=1;j<n+1;j++)
        {
            if(b[j]==0)
                continue;
            for(h=n;h>j;h--)
            {
                if(b[h]==0)
                    continue;
                if(a[j]!=a[h]&&b[j]<b[h])
                {
                    m+=a[h]-a[j];
                    p+=2;
                    b[j]=0;b[h]=0;
                    break;
                }
            }
        }
        printf("%lld %lld\n",m,p);
    }
    return 0;
}



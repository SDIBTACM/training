这个周学习了结构体排序，下面是整理的代码
# 结构体排序  
某小学考完试要对成绩进行排序。有语文、数学、英语三门成绩。  
用 数组应用1-插入排序 进行排序。  
要求用函数指针实现比较函数，插入函数不用变。  
插入函数定义如下：  
int insertsort(struct student a[], int n, int (*compare)(struct student *, struct student *, int), int increase)  

参数说明：struct student a[]表示要排序的结构体数组，int n表示学生数量，int (*compare)(struct student *, struct student *, int)比较函数，int increase 升序还是降序，=1表示升序=0表示降序。  

insertsort()函数中要调用search_pos()，定义如下：  

int search_pos(struct student a[], int n, struct student key, int (*compare)(struct student *, struct student *, int), int increase)  

参数说明同上。  
举例说明：比较函数可以这样实现（按姓名排序）  

输入:  

第一行两个整数n,m。n表示学生数量，m表示排序的个数  
接下来n行，分别输入学生的姓名、语文成绩、英语成绩、数学成绩  
接下来m行表示要如何排序，每行一个字符和一个整数。  
字符为N表示按姓名排序  
字符为C表示按语文成绩排序  
字符为E表示按英语成绩排序  
字符为M表示按数学成绩排序  
字符为T表示按总成绩排序  
整数=1表示升序，=0表示降序  

(特别说明：如果有相同成绩，则按姓名排序。比如zhangsan和lisi的数学成绩相同，则lisi排在zhangsan前，语文、英语、总成绩也是如此)  

输出每次排序结果。按姓名、语文、英语、数学、总成绩的顺序输出，不同排序中间加一空行。

1.定义结构体
```c++
#include<string.h>
#include<stdio.h>
#include<stdlib.h>
struct student
{
    char name[32];
    int chi;
    int math;
    int eng;
    int tot;
};
char x;
```
2.比较函数
```c++
//比较姓名

int compareName(struct student *a, struct student *b, int increase)
{
    if (increase)
    {
        if (strcmp(a->name, b->name) <= 0)
            return 1;
        else
            return 0;
    }
    else
    {
        if (strcmp(a->name, b->name) <= 0)
            return 0;
        else
            return 1;
    }
}

//比较四个分数

int cmp(struct student *a, struct student *key,int increase)
{
    if(x=='N')
        return compareName(a,key,increase);
    if(x=='C')
    {
        if(a->chi==key->chi)
            return compareName(a,key,1);
        else
        {
            if(increase)
            {
                if(a->chi<key->chi)
                    return 1;
                return 0;
            }
            else
            {
                if(a->chi>key->chi)
                    return 1;
                return 0;
            }
        }
    }
    if(x=='E')
    {
        if(a->eng==key->eng)
            return compareName(a,key,1);
        else
        {
            if(increase)
            {
                if(a->eng<key->eng)
                    return 1;
 
                return 0;
            }
            else
            {
                if(a->eng>key->eng)
                    return 1;
                return 0;
            }
        }
    }
    if(x=='M')
    {
        if(a->math==key->math)
            return compareName(a,key,1);
        else
        {
            if(increase)
            {
                if(a->math<key->math)
                    return 1;
 
                return 0;
            }
            else
            {
                if(a->math>key->math)
                    return 1;
                return 0;
            }
        }
    }
    if(x=='T')
    {
        if(a->tot==key->tot)
            return compareName(a,key,1);
        else
        {
            if(increase)
            {
                if(a->tot<key->tot)
                    return 1;
                return 0;
            }
            else
            {
                if(a->tot>key->tot)
                    return 1;
                return 0;
            }
        }
    }
}
```

3.插入排序
```c++
int search_pos(struct student a[], int n, struct student key, int (*compare)(struct student *, struct student *, int), int increase)
{
    int i=0;
    while(compare(&a[i],&key,increase)&&i<n)
    {
        i++;
    }
    return i;
}
int insert(struct student a[], int n,int pos,struct student key)//后移
{
 
    for(int i=n-1; i>=pos; i--)
        a[i+1]=a[i];
    a[pos]=key;
    return n+1;
}
int insertsort(struct student a[], int n, int (*compare)(struct student *, struct student *, int), int increase)
{
    for(int i=1; i<n; i++)
    {
        struct student key=a[i];
        int pos=search_pos(a,i,key,compare,increase);
        insert(a,i,pos,key);
    }
    return 0;
}
```

4.主函数
```c++
int main()
{
    int n,m;
    int y;
    scanf("%d%d",&n,&m);
    struct student *a=(struct student *)malloc(sizeof(struct student)*n);//动态分布结构体数组
    for(int i=0; i<n; i++)//输入
    {
        scanf("%s %d %d %d",&a[i].name,&a[i].chi,&a[i].eng,&a[i].math);
        a[i].tot=a[i].chi+a[i].math+a[i].eng;
    }
    for(int z=0; z<m; z++)
    {
        scanf(" %c %d",&x,&y);//或者%c前面不加“ ”换成getchar();
        insertsort(a,n,&cmp,y);//&cmp调用函数指针
        for(int i=0; i<n; i++)//输出
        {
            printf("%s %d %d %d %d\n",a[i].name,a[i].chi,a[i].eng,a[i].math,a[i].tot);
        }
        printf("\n");
    }
    return 0;
}
```

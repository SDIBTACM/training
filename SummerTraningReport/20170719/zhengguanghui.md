## 优先队列的排序方法
```.cpp
struct node
{
    friend bool operator< (node n1,node n2)
    {
        if(n1.rp == n2.rp)
            return strcmp(n1.name,n2.name)<0;
        return n1.rp>n2.rp;
    }
    char name[25];
    int rp;
};
```
------
不知道为啥上次没有上传上去QAQ

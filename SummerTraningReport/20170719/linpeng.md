 # c++: pair类模板的用法详解
---
## pair:

头文件：#include<utility>

类模板：template <class T1, class T2> struct pair

参数：T1是第一个值的数据类型，T2是第二个值的数据类型。

功能：pair将一对值组合成一个值，这一对值可以具有不同的数据类型（T1和T2），两个值可以分别用pair的两个公有函数first和second访问。

具体用法：

1.实例化：　

```cpp
1     pair<string,string> p1("hello","word"); //调用default constructor
2     pair<double,int>    p2(1.0,1);//调用constructor
3     pair<double,int>    p3(p2); //调用 copy
```
 

2.对象的赋值以及make_pair（）的应用：

```cpp
1     pair<string,string> p1;
2     pair<string,string> p2("good","good");
3     p1= p2;
4     p1= make_pair("hello","word");
5     p1 = pair<string,string>("nice","nice");
```
3.pair中元素的访问（first & second）：

```cpp
1     pair<double,int>    p1(1.0,2);
2     pair<string,string> p2("hello","word");
3     int i = p1.second;  // i = 2
4     double d = p1.first; // d = 1.0
5     string s1 = p2.first; // s1 = hello
6     string s2 = p2.second; // word
```
4.pair数组与元素排序：

```cpp
 1 #include<iostream>
 2 #include<cstdio>
 3 #include<algorithm>
 4 using namespace std;
 5 pair<int,int>pa[100];
 6 int cmp(pair<int,int>a,pair<int,int>b){
 7     if(a.first!=b.first)return a.first>b.first;
 8     else return a.second<b.second;
 9 }
10 int main(){
11     int a,b;
12     for(int i=0;i<5;i++)scanf("%d%d",&a,&b),pa[i]=make_pair(a,b);
13     sort(pa,pa+5,cmp);
14     for(int i=0;i<5;i++)printf("%d %d\n",pa[i].first,pa[i].second);
15     return 0;
16 }

```

本文为个人随笔,如有不当之处,望各位大佬多多指教.
若能为各位提供小小帮助,不胜荣幸.

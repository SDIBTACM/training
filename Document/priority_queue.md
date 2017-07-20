# 优先队列
```cpp 
头文件:   #include<functional>
         #include<queue>

```

```cpp
实例化:   priority_queue<int,vector<int>,less<int> >que;
         priority_queue<int,vector<int>,greater<int> >que;
         
```

```cpp
自定义优先级：(1)struct cmp{
                bool operater()(int &a,int &b){
                  return a>b; //最小值优先,若改成a<b，则是最大值优先
               }
               priority_queue<int,vector<int>,cmp>que;
               
            (2)struct qu{
                int x;
                bool operater < (const qu &a) const{
                      return x>a.x;//最小值优先，反之最大值优先
                      }
                }
                priorith_queue<qu> que;
```
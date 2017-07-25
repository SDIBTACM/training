#  对组
```cpp
    类模板：template <class T1, class T2> struct pair

    参数：T1是第一个值的数据类型，T2是第二个值的数据类型。
```
#  功能：
```cpp
            pair将一对值组合成一个值，这一对值可以具有不同的数据类型（T1和T2），
          两个值可以分别用pair的两个公有函数first和second访问。
    
            pair是一种模板类型，其中包含两个数据值，两个数据的类型可以不同，基本的定义如下：
              pair<int, string> a  ///表示a中有两个类型，第一个元素是int型的，第二个元素是string类型的，
           如果创建pair的时候没有对其进行初始化，则调用默认构造函数对其初始化。
              pair<string, string> a("aaa", "aaaa")///也可以像上面一样在定义的时候直接对其初始化。
              
            由于pair类型的使用比较繁琐，用typedef简化声明：            
              typedef pair<string, string> mypair;
              mypair a("aaa", "aaaa");
              mypair b("bbb", "bbbb");
```
#  操作
```cpp
          对于pair类，由于它只有两个元素，分别名为first和second，因此直接使用普通的点操作符即可访问其成员
          name = pair.second;
          
          生成新的pair对象,可以使用make_pair对已存在的两个数据构造一个新的pair类型：
          int data = 8;
          char m = 'c';
          pair<int, char> b;
          b = make_pair(a, m);
              


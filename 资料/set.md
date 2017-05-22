# set 的常用用法 (C++98)

## set 简介
Sets are containers that store unique elements following a specific order.<br/>
一个不包含重复元素的集合<br/>

## 常用函数
insert 插入<br/> erase 删除<br/> swap 交换<br/> clear 清空容器 <br/>

### insert 
插入函数<br/>参数类型
>(1)pair<iterator,bool> insert (const value_type& val);<br/> (2)iterator insert (iterator position, const value_type& val);<br> (3) template <class InputIterator>
  void insert (InputIterator first, InputIterator last);<br/>

```
// set::insert (C++98)
#include <iostream>
#include <set>

int main()
{
    std::set<int> myset;
    std::set<int>::iterator it;
    std::pair<std::set<int>::iterator, bool> ret;

    // set some initial values:
    for (int i = 1; i <= 5; ++i)
        myset.insert(i * 10); // set: 10 20 30 40 50

    ret = myset.insert(20); // no new element inserted

    if (ret.second == false)
        it = ret.first; // "it" now points to element 20

    myset.insert(it, 25); // max efficiency inserting
    myset.insert(it, 24); // max efficiency inserting
    myset.insert(it, 26); // no max efficiency inserting

    int myints[] = {5, 10, 15}; // 10 already in set, not inserted
    myset.insert(myints, myints + 3);

    std::cout << "myset contains:";
    for (it = myset.begin(); it != myset.end(); ++it)
        std::cout << ' ' << *it;
    std::cout << '\n';

    return 0;
}
```
>Output : myset contains: 5 10 15 20 24 25 26 30 40 50

### erase 
删除set内某个或某几个元素<br/>

参数类型
>(1)void erase (iterator position);<br/>(2)size_type erase (const value_type& val);<br/>(3)void erase (iterator first, iterator last);<br/>

```
// erasing from set
#include <iostream>
#include <set>

int main()
{
    std::set<int> myset;
    std::set<int>::iterator it;

    // insert some values:
    for (int i = 1; i < 10; i++)
        myset.insert(i * 10); // 10 20 30 40 50 60 70 80 90

    it = myset.begin();
    ++it; // "it" points now to 20

    myset.erase(it);

    myset.erase(40);

    it = myset.find(60);
    myset.erase(it, myset.end());

    std::cout << "myset contains:";
    for (it = myset.begin(); it != myset.end(); ++it)
        std::cout << ' ' << *it;
    std::cout << '\n';

    return 0;
}
```
>myset contains: 10 30 50

### swap 
交换集合内两个元素的位置 <br/>

参数类型
>void swap（set＆x）;

```
// swap sets
#include <iostream>
#include <set>

main()
{
    int myints[] = {12, 75, 10, 32, 20, 25};
    std::set<int> first(myints, myints + 3);      // 10,12,75
    std::set<int> second(myints + 3, myints + 6); // 20,25,32

    first.swap(second);

    std::cout << "first contains:";
    for (std::set<int>::iterator it = first.begin(); it != first.end(); ++it)
        std::cout << ' ' << *it;
    std::cout << '\n';

    std::cout << "second contains:";
    for (std::set<int>::iterator it = second.begin(); it != second.end(); ++it)
        std::cout << ' ' << *it;
    std::cout << '\n';

    return 0;
}
```
>first contains: 20 25 32<br/>second contains: 10 12 75
### clear
清空某个集和的所有内容<br/>

参数类型
>void clear();

```
// set::clear
#include <iostream>
#include <set>

int main()
{
    std::set<int> myset;

    myset.insert(100);
    myset.insert(200);
    myset.insert(300);

    std::cout << "myset contains:";
    for (std::set<int>::iterator it = myset.begin(); it != myset.end(); ++it)
        std::cout << ' ' << *it;
    std::cout << '\n';

    myset.clear();
    myset.insert(1101);
    myset.insert(2202);

    std::cout << "myset contains:";
    for (std::set<int>::iterator it = myset.begin(); it != myset.end(); ++it)
        std::cout << ' ' << *it;
    std::cout << '\n';

    return 0;
}
```
>myset contains: 100 200 300<br/>myset contains: 1101 2202

## STL通用的函数不在一一列举

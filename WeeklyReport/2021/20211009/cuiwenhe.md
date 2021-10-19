###  周报

- 本周应用了STL的rope

 #include <ext/rope>

using namespace __gnu_cxx;// 头文件

push_back(x)//在末尾添加x

insert(pos,x)//在pos插入x

erase(pos,x)//从pos开始删除x个

replace(pos,x)//从pos开始换成x

substr(pos,x)//提取pos开始x个

at(x)/[x]//访问第x个元素

本周还学习了替罪羊平衡树(暴力美学)复杂度(logn)

就是将不满足条件的进行中序遍历然后分治拎起来，重构树，有插入，删除，第k大，和查询前驱后继的操作，注意有个平衡因子=0.75

- 本周感受

  应用选择好STL会事半功倍，自己了解的算法还太少

- 下周计划

在完成日常学习的同时，会将精力投入到算法的学习和题目的练习中，会更好的安排自己的时间
## 辗转相除法
```cpp
int GCD(int a, int b)
{
	return b ? GCD(b, a%b): a;
}
```
<br>
求最大公约数用<br>
就一行代码，没什么好说的<br>
估计会有一些思维题需要用到GCD<br>
<br><br>

## 凸包
```cpp
void Graham()
{
    int k = 0;
    for(int i = 1; i < n; i++)
        if(p[i].y < p[k].y || (p[i].y == p[k].y && p[i].x < p[k].x))
			k = i;
	
    swap(p[k], p[0]);
	sort(p+1, p+n, cmp);
	s[0] = p[0];
	s[1] = p[1];
	top = 1;

	for(int i = 2; i < n; i++)
	{
		while(top >= 1 && chaji(s[top-1], s[top], p[i]) >= 0)
			top--;
		top++;
		s[top] = p[i];
	}
}
```
叉积与极角排序都是为凸包所服务的<br>
主要思想就是将所有的点都用一个多边形包括<br>
以解决任意两点最长距离、最外点连多边形周长等<br>
要注意的就是点的数目是1和2的情况要判断下<br>
<br><br>

## 组队赛
- 被各位dalao按在地上摩擦
- 成功的带歪了榜
- 思维题WA因为思维不够灵活
- 感觉自己像瓜皮一样
- 以后应该审好题再submit
- 太心急了，下标出错，找不出错误的地方
<br><br><br><br>

希望以后能变得更强<br>
いじょうです


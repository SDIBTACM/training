# 选拔赛2  
## C题：  
您被赋予一个长度为 n 的字符串 s，且 n 是偶数。字符串 s 仅由 0 和 1 组成，即它是一个二进制字符串。
字符串 s 中恰好包含 n2 个 0 和 n2 个 1（其中 n 为偶数）。
您可以执行一种操作：反转 s 的任意一个子串。字符串的子串是指该字符串中连续的一段子序列。
请问最少需要多少次操作才能使字符串 s 变成交替字符串？所谓交替字符串，指的是对于所有 i，第 i 个字符与第 i+1 个字符不相等。通常有两种类型的交替字符串：01010101... 或 10101010...  
输入：  
第一行包含一个整数 t（1 ≤ t ≤ 1000），表示测试用例的数量。
每个测试用例的第一行包含一个整数 n（2 ≤ n ≤ 105；n 为偶数），表示字符串 s 的长度。
每个测试用例的第二行包含一个长度为 n 的二进制字符串 s（si ∈ {0, 1}）。字符串 s 恰好包含 n2 个 0 和 n2 个 1。
保证所有测试用例中 n 的总和不超过 105。  
输出：  
对于每个测试用例，输出使字符串 s 变成交替字符串所需的最少操作次数。  
## D题：    
商店中有 N 件商品。对于每一件商品 i（i = 1, 2, ..., N），第 i 件商品的价格为 A_i 日元。
高桥拥有 K 张优惠券。每张优惠券可以用于一件商品上。同一商品上可以使用任意数量（包括零张）的优惠券。若对价格为 a 日元的商品使用 k 张优惠券，则可以以 max{a - kX, 0} 日元的价格购买该商品。
请输出高桥购买全部商品所需的最低金额。
约束条件： 1 ≤ N ≤ 2 × 10^5 1 ≤ K, X ≤ 10^9 1 ≤ A_i ≤ 10^9
输入中的所有值均为整数。  
输入格式：  
N K X A_1 A_2 ... A_N  
输出格式：  打印答案。  
## E题：  
给定一个整数 N，找出满足以下所有条件的最小整数 X：
X 不小于 N。 存在一对非负整数 (a, b)，使得 X = a^3 + a^2b + ab^2 + b^3。
约束条件： N 是一个整数。 0 ≤ N ≤ 10^18  
输入格式： N  
输出格式： 以整数形式打印答案。  
## F题：  
蒙诺卡普有一棵包含 n 个顶点且以顶点 1 为根的树。他决定学习广度优先搜索（BFS），于是从根节点开始在该树上运行了 BFS。BFS 可以用以下伪代码描述：
```
a = []  # 记录顶点被处理的顺序
q = Queue()
q.put(1)  # 将根节点放入队列尾部
while not q.empty():
    k = q.pop()  # 从队列头部取出第一个顶点
    a.append(k)  # 将 k 添加到访问顺序序列的末尾
    for y in g[k]:  # g[k] 是顶点 k 的所有子节点列表，按升序排序
        q.put(y)
```
蒙诺卡普对 BFS 如此着迷，以至于最后不慎丢失了他的树。幸运的是，他还保留着 BFS 算法访问顶点的顺序序列（伪代码中的数组 a）。他知道每个顶点都被恰好访问了一次（因为它们被恰一次放入和取出队列）。此外，他还知道每个顶点的所有子节点在其访问顺序中都是按照升序排列的。
蒙诺卡普意识到存在许多具有相同访问顺序 a 的不同树（在一般情况下），所以他不指望恢复原来的那棵树。他满足于找到任何具有给定访问顺序 a 且高度最小的树。
树的高度是指树中顶点的最大深度，而一个顶点的深度是指从根到该顶点路径上的边数。例如，顶点 1 的深度为 0，因为它就是根节点，而根节点所有子节点的深度均为 1。
帮助蒙诺卡普找到任何具有给定访问顺序 a 和最小高度的树。  
输入格式：  
第一行包含一个整数 t（1≤t≤1000），表示测试用例的数量。
每个测试用例的第一行包含一个整数 n（2≤n≤2⋅105），表示树中顶点的数量。
每个测试用例的第二行包含 n 个整数 a1, a2, ..., an（1≤ai≤n；ai≠aj；a1=1），表示 BFS 算法按照访问顺序访问顶点的序列。
保证所有测试用例中 n 的总和不超过 2⋅105。  
输出格式：  
对于每个测试用例，输出具有给定访问顺序 a 的树的最小可能高度。  
## G题：
问题描述 我们有一个 N × N 的棋盘。令 (i, j) 表示棋盘上从顶部数第 i 行、从左边数第 j 列的格子。
棋盘由 N 个字符串 S[i] 描述。其中 S[i][j]，即字符串 S[i] 的第 j 个字符，具有以下含义：  
若 S[i][j] = '.', 则格子 (i, j) 为空。
若 S[i][j] = '#', 则格子 (i, j) 被一个白色兵占据，该兵不能移动或移除。
我们在格子 (A_x, A_y) 上放置了一个白色象。请根据国际象棋规则（见“注释”部分），找出将这个象从 (A_x, A_y) 移动到 (B_x, B_y) 所需的最小步数。如果无法移动至目标位置，则输出 -1。
注释 棋盘上位于 (i, j) 的白色象可以按照以下方式在一步内移动：
对于每个正整数d，
象可以移动到 (i + d, j + d)，前提是：
(i + d, j + d) 在棋盘范围内；
对于每个 1 ≤ l ≤ d，格子 (i + l, j + l) 不被白色兵占据。
象可以移动到 (i + d, j - d)，前提是：
(i + d, j - d) 在棋盘范围内；
对于每个 1 ≤ l ≤ d，格子 (i + l, j - l) 不被白色兵占据。
象可以移动到 (i - d, j + d)，前提是：
(i - d, j + d) 在棋盘范围内；
对于每个 1 ≤ l ≤ d，格子 (i - l, j + l) 不被白色兵占据。
象可以移动到 (i - d, j - d)，前提是：
(i - d, j - d) 在棋盘范围内；
对于每个 1 ≤ l ≤ d，格子 (i - l, j - l) 不被白色兵占据。  
约束条件
2 ≤ N ≤ 1500
1 ≤ A_x, A_y ≤ N
1 ≤ B_x, B_y ≤ N
(A_x, A_y) ≠ (B_x, B_y)
对于所有 1 ≤ i ≤ N，S[i] 是长度为 N 的只包含 '.' 和 '#' 字符的字符串。
S[A_x][A_y] = '.'
S[B_x][B_y] = '.'  
## H题：
厨师Monocarp刚刚将n道菜肴放入烤箱。他知道第i道菜的理想烹饪时间是ti分钟。
在任何正整数分钟T，Monocarp最多可以从烤箱中取出一道菜。如果第i道菜在某个分钟T被取出，那么它的不愉快值为|T−ti|，即T与ti之间的绝对差值。一旦菜品离开烤箱，就不能再放回去了。
Monocarp需要将所有菜肴都从烤箱中取出。他能获得的最小总不愉快值是多少？  
输入  
第一行包含一个整数q（1≤q≤200）——测试用例的数量。
接着有q个测试用例。
每个测试用例的第一行包含一个整数n（1≤n≤200）——烤箱中菜肴的数量。
测试用例的第二行包含n个整数t1，t2，…，tn（1≤ti≤n）——每道菜的理想烹饪时间。
所有q个测试用例中n的总和不超过200。  
输出  
对于每个测试用例，输出一个整数——当Monocarp将所有菜肴从烤箱中取出时，他能获得的最小总不愉快值。记住，Monocarp只能在正整数分钟取出菜肴，并且任何一分钟都不能取出多于一道菜。  
# 本周感想：  
- 这学期开学以来，专业课太少了，感觉都不叫计算机专业了，高等数学——离散数学——大学物理，堪称大一下三巨头，真的痛苦 :sweat_smile:  
+ 还有一堆水课，真的占据好多时间，不想上 :triumph:  
* dfs和bfs这两种算法感觉有点有趣，尤其是G题，做出来的时候真的有成就感 :sunglasses:  
+ 但是F题和D题得加限制条件才能对，而这个限制条件根本想不到，不参考网上题解真的做不出来 :hot_face:  
+ 英语水平在大学飞速下降，选拔赛题意不好理解，得多记点这方面的单词，打算拿个本记下每次遇到的生词  
# 下周计划：  
1. 把dfs和bfs这两算法的题做完，再把STL的算法看看，素数筛也忘了...
2. 复习大学物理，下周或者下下周测试  
3. 高数也得理解理解，上大学感觉脑子变笨了  

# A
## 数学交易
假如有一群人有拥有的东西，以及想要的东西。  
（表格）  
注意列出的这些人都不能配对并相互交易。然而，如果Sally、Steve和Carlos聚在一起，Sally可以用她的时钟和Steve交换她想要的娃娃，然后Steve可以用那个时钟和Carlos交换Steve想要的画。  
这种个体交易链的创造，让大量的人获得他们想要的东西，称为数学交易。理想情况下，每个人都参与数学交易，但这并不总是可能的(对不起，Maria)。因此目标是创建一个最长长度的单链。如果参与者想要交易或获得多个物品，确定最长的数学交易就会变得复杂。幸运的是，我们只考虑每个人只拥有一件物品，且没有一件物品是由一个以上的人拥有或想要的。  
### 输入
输入从包含正整数n (n≤100)的一行开始，n表示对交易感兴趣的人数。在这之后是n行，每一行用空格分隔三个字符串。第一个字符串是交易者的名字。第二个字符串是交易者拥有的东西。第三串是交易者想要的东西。所有商人的名字都是唯一的，没有一个物品会被不止一个人需要，也不会被不止一个人拥有。
### 输出
输出最长数学交易的长度。如果没有交易可能，输出短语“没有交易可能”。  
（例子）  
# B
## 越过山丘，第一部分
希尔加密(由数学家Lester S.Hill于1929年发明)是一种利用矩阵和模运算的技术。理想情况下，它与具有素数字符的字母表一起使用，因此我们将使用37个字符的字母表A， B，…，Z, 0,1，…，9和空格字符。涉及的步骤如下:  
1. 将初始文本(明文)中的每个字符替换为A→0,B→1，…，(空格)→36。如果明文是ATTACK AT DAWN，这就变成  
0 19 19 0 2 10 36 0 19 36 3 0 22 13  
2. 将这些数字分组为三分量向量，必要时在末尾填充空格。这一步之后我们有：  
 （大括号里面三个数字）  
3. 使用模37算法将这些向量中的每一个乘以预定的3 × 3加密矩阵。如果加密矩阵为  
（算出来大括号里九个数字）  
将第一个向量变换如下: 得（5 15 11）  
4. 将所有向量乘以加密矩阵后，将结果值转换回37个字符的字母表，并将结果连接起来，即可得到加密的密文。在我们的例子中，密文是  
FPLSFA4SUK2W9K3。  
这种方法可以推广到任何n × n的加密矩阵，在这种情况下，初始明文被分解成长度为n的向量。对于这个问题，你将得到一个加密矩阵和一个明文，并且必须计算相应的密文。  
### 输入
输入以一行开始，其中包含一个正整数n≤10，表示矩阵的大小和用于加密的向量。之后是n行，每行包含n个指定加密矩阵的非负整数。在这之后是一行，其中包含仅由上面指定的37个字符的字母表中的字符组成的明文。
### 输出
在单行上输出相应的密文。  
（例子）  
# C
## 排名问题
教练受够了体育排名——他认为那些编造虚假排名的人都是疯子。在教练看来，排名的变化应该只基于事实。例如，假设第4名的队伍和第1名的队伍比赛并且输了。为什么要改变排名呢?“差”队输给了“好”队，所以排名不会改变。换句话说，没有证据表明顺序应该改变，那么为什么要改变呢?只有当第四名的球队击败第一名的球队时，你才会做出改变。现在你有证据表明排名应该改变！具体来说，第一名的球队应该直接排在第四名的球队下面(我们现在有证据支持这一点)，第二到第四名的球队应该各自上升一个。结果是，以前的第一名球队现在排在第四名，比击败它的球队降低一个名次，以前的第四名球队现在排名第三。请注意，现在在第一到第三名的车队的相对位置不会改变——没有证据表明他们应该改变。
为了概括这个过程，假设位置n的队伍战胜了位置m的队伍。如果n < m，那么排名应该没有变化；如果n> m，则所有在位置m + 1, m + 2，…， n应该向上移动一个位置，m位置的队伍移动到n位置。  
例如，假设有5支队伍最初的排名顺序是T1(最好)、T2、T3、T4、T5(最差)。假设T4大于T1。然后如上所述，新的排名应该是T2, T3, T4, T1, T5。在下一场比赛中假设T3战胜T1。在此之后，排名应该不会改变-排名较好的球队击败排名较差的球队。如果在接下来的游戏中T5打败了T3，那么新的排名将是T2、T4、T1、T5、T3，以此类推。
教练已经准备好编写一个程序来实现这个计划，但后来他听说了英超联赛的平局。我们最后见到他时，他正一动不动地站在那里，眼睛盯着窗外。我们猜该由你来写程序了。  
### 输入
输入的第一行包含两个正整数n m (n, m≤100)，表示队伍数量和比赛场次。队名是T1, T2，…， Tn，最初每个队伍Ti在排名中处于第i位(即T1队在第一名，Tn队在最后一名)。第一行之后是m行，详细描述了一组比赛。按时间顺序排列。每条线的形式为Ti Tj(1≤i, j≤n, i 6 = j)，表示Ti队击败Tj队。  
### 输出 
输出单行，列出球队的最终排名。用单个空格分隔队名   
（例子）；  
# D
## 简单的数独
数独游戏有各种不同的形状和难度等级。传统的数独游戏是一个9 × 9的格子。最初，一些单元格用数字填充，一些是空的。目标是用1 - 9范围内的数字填充每个单元格，并受以下限制:  
•每个数字1 - 9必须在每行出现一次  
•每个数字1 - 9必须在每列中出现一次  
•每个数字1 - 9必须在每个3 × 3子网格中出现一次  
数独游戏的难度差别很大。最简单的谜题可以用以下两种简单的技巧来解决:  
单值规则:搜索只有一个可能值的正方形。  
唯一位置规则:在任何行、列或子网格中搜索只能放置在九个位置之一的值。  
考虑图1所示的部分解决的数独谜题。单一值规则适用于网格正方形A7，其中8是唯一可以放置的值。唯一位置规则可用于将5放在正方形B3中，因为它是第3行中唯一可以放置5的位置。  
（例一数独）  
最简单的数独游戏也只能通过这两条规则来解决;更难的谜题使用了剑鱼、x翼和虫子等技巧。  
对于这个问题，你将得到一个数独谜题，并且必须确定它是否是一个简单的谜题，即是否可以通过使用单一值和唯一位置规则来解决它。  
# 输入
输入包括一个九行数独谜题。每行包含9个值，范围从0到9，其中0表示谜题中的空白。
# 输出 
如果是简单的数独，输出Easy这个单词，后面跟着已解决的数独。这个谜题应该打印成九行，每行用一个空格分隔值。如果谜题不容易，不容易输出，是可以使用上述两个规则解决的数独谜题。对于部分解和使用的完整解使用相同的格式。而不是表示未填充的方格的数字。  
（例子）
# E
## 沉甸甸
学校的计算机科学图书馆需要临时关闭进行装修，图书馆的所有图书由你负责保管。在克服了被选中的震惊，以及校园里竟然有一个CS图书馆的震惊之后，你就开始工作了。这些书已经装在相同重量的盒子里了。你想把这些箱子堆在木托盘上，以防储藏室被水淹，但是你有一个小问题：你不知道在托盘被重量压垮之前你能堆多少箱子。我们把可放置在一个托盘上的箱子的最大数量称为箱子限制。  
你可以有序地把一个盒子放在托盘上，然后是第二个，第三个，等等，直到托盘破裂，但这似乎非常耗时(并导致一个非常无聊的竞赛问题)。但是，如果您有一个或多个托盘进行试验，您可能能够更快地确定箱的限制。例如，假设由于箱子的大小和储藏室天花板的高度，任何堆叠中的最大箱子数为3。只有一个托盘，你可以先尝试一个盒子。如果托盘塌了，你需要去找一些更结实的托盘。如果托盘能承受得住的话，你可以试试另一个箱子。如果托盘坍塌现在你知道箱子限制是1；否则，您可以尝试第三个箱子，这将让您知道箱子限制是2(如果托盘倒塌)还是3(如果托盘不倒塌)。这种方法最多需要三个不同的实验。然而，如果你有两个托盘，你最多只需要两个实验就可以确定箱子的限制:首先，你在第一个托盘上尝试两个箱子;如果它成立，你尝试第三个，它会让你知道箱子限制是2还是3。如果第一个托盘在第一次实验中坍塌，你就把第二个托盘拖出来，在上面放一个盒子。这个实验的结果告诉你箱子限制是1还是0。
你不能完全确定储藏室的高度(这决定了可以堆放的最大箱子数量)，也不能确定你有多少个托盘可供试验。你想知道的是，在给定这些信息的情况下，在给定最优策略的情况下，你需要运行的最坏情况下的最小实验次数是多少。很遗憾你之前不知道CS图书馆——也许在其中一本书中会有一些对你有帮助的东西。  
### 输入
输入由单行组成，包含两个正整数n和m (n≤5 000,m≤20)，其中n表示可以堆叠的最大箱子数量(不管托盘是否会被它们压垮)，m表示可用的托盘数量。
### 输出
输出最优策略所需的最坏情况下的最小实验次数，然后是在第一次实验中使用的箱子数目。如果存在可用于第一次实验的范围框，则用连字符分隔该范围的最小值和最大值；如果只有一个这样的数字，只需输出该数字。  
（例子）
# F
## 全世界工人联合起来！不要靠近
你负责一组工作人员，他们正在进行一个项目，这个项目是高度机密，我们甚至无法为它想出一个名字。他们分别住在图1左边所示的一组公寓里，每天早上他们都在同一时间离开，然后前往图1右边所示的一个工作站。为了从他们的家到工作站，他们必须通过中间显示的安全门。每个门都有两个不同的走廊，分别标记为A和B。从图中可以看出，每个门的A走廊总是在B走廊的北面。  
让工人们去工作站听起来很简单，但随着新冠病毒的爆发，一个问题出现了。首先，由于保持社会距离，没有两名工人可以走同一扇门。其次，由于大门的布局，如果一个工人走A走廊，那么走他们北面大门的人就不能走B走廊——太近了。类似的情况也会发生，如果一个工人走B走廊——工人走南面的门不能走A走廊。  
你负责分配工人到各个工位，每个工位一名工人。哪个工人被分配到哪个工作站并不重要，但您希望在遵守上述社交距离要求的同时，最大限度地减少所有工人的总距离。由于入口和出口的奇怪布局，当使用A走廊和B走廊作为门时，有时会有很大的距离差异，这使问题变得复杂。给定所有的相对距离，确定工人到工作站的分配，使每个人的总距离最小化。  
### 输入
输入从一行开始，其中包含一个正整数n≤50，指定工人、门和工作站的数量，每个编号为1到n。接下来是n行，每个包含2n个正整数。这些线中的第i条给出了从工人i到n个门入口的距离；前两个值是门1到走廊A和走廊B的距离，后两个值是门2到走廊A和走廊B的距离，以此类推。在这之后还有n行，每一行包含2n个正整数。这些线中的第j行以类似的方式给出了从工作站j到n个门出口的距离。所有距离均为正数且≤1000。
### 输出
在输出的第一行打印工人所能达到的最小总距离。然后用n行显示每个工人的分配。这些行的第i行形式为i  gi  wi，表示工作人员i使用门 gi到达工作站wi。使用示例输出中显示的格式。如果有多个最优答案，任何一个都可以接受。  
（例子）

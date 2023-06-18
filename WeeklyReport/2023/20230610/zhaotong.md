# 题解周报
## A - Divisibility by 2^n
**思维**
给n个数， 先统计所给的n个数里面可以拆分出几个2， 然后遍历一遍1 ~ n， 看第i个数（1 <= i <= n）可以以拆分出几个2， 将这些数能提供的2的数量排个序，从大到小的加上原有的数量，看是否能得到目标数量
```C++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

bool cmp(const int a, const int b) {
	return a > b;
}

int main() {
	ios::sync_with_stdio(0);
	cin.tie(0);
	int t;
	cin >> t;
	while (t--) {
		int n;
		cin >> n;
		int num_2 = 0;

		for (int i = 0; i < n; i++) {
			int t;
			cin >> t;
			while (t % 2 == 0) {
				num_2++;
				t /= 2;
			}
		}
		vector<int> have_2(n + 1, 0);
		for (int j = 2; j <= n; j++) {
			int t = j;
			while (t % 2 == 0) {
				have_2[j]++;
				t /= 2;
			}
		}
		sort(have_2.begin(), have_2.end(), cmp);
		int sum = 0;
		int cnt = 0;
		int flag = 1;

		for (int i = 0; i <= n; i++) {
			if (sum + num_2 >= n) {
				cout << cnt << endl;
				flag = 0;
				break;
			}
			sum += have_2[i];
			cnt++;
		}
		if (flag) {
			cout << -1 << endl;
		}
	}
	return 0;
}
```
***
# B - Swap Game
**思维，博弈**
玩家行动的回合可以任意选择除第一个数以外的数与第一个数减一后作交换， 如果在玩家行动开始时，第一个数已经是0则输掉游戏。换言之，每回合玩家选择一个数减一，谁先将一个数减到0，谁就能赢，所以最优选择是选最小的那个数，选完数以后，该数会移动到1位置， 也就相当于将这个数保护起来了，下一个玩家不能选这个数。在游戏一开始， 除了第一个数，A可以选择此外最小的数，所以跑一遍看最小的数是在1位置还是后面位置，在后面位置A赢，反之B赢，同时有最小B赢。
```C++
#include <iostream>
#include <vector>
using namespace std;
const int inf = 0x3f3f3f3f;
int main() {
	int t;
	cin >> t;
	while (t--) {
		int min_a = inf, min_b = inf;
		int n;
		cin >> n;
		cin >> min_b;
		for (int i = 1; i < n; i++) {
			int temp;
			cin >> temp;
			min_a = min(min_a, temp);
		}
		if (min_a == min_b) {
			cout << "Bob" << endl;
		}
		else if (min_a < min_b) {
			cout << "Alice" << endl;
		}
		else if (min_a > min_b) {
			cout << "Bob" << endl;
		}
	}
	return 0;
}
```
***
## C - Permutation Operations
**思维**
进行n次，每次将第i个数（1 <= i <= n）本身及其以后的数都加上k（1 <= k <= n），k不能重复，将给的数列在进行n次操作后转换成严格递增数列。一开始想用贪心，选择需要增加的最小值加上去，结果查找最小值的过程超时了，其实只需要将需要增加的数从大到小排个序，一一匹配就行，
其实就是换了个思路，贪心是用最小的力气去完成任务，现在是用最大的力气去完成最困难的任务。而数列的元素大小范围是1到n，也就是说必然存在解。
```C++
#define _CRT_SECURE_NO_WARNINGS
#include <vector>
#include <iostream>
#include <algorithm>
using namespace std;
const int inf = 0x3f33f3f;
int main() {
	int t;
	scanf("%d", &t);
	while (t--) {
		int n;
		int pre;
		scanf("%d", &n);
		vector<pair<int, int>> arr(n + 1);
		vector<int> ans(n + 1, 0);
		scanf("%d", &pre);
		arr[1].first = inf;
		arr[0].first = inf;
		for (int i = 2; i <= n; i++) {
			int temp;
			scanf("%d", &temp);
			arr[i].first = temp - pre;
			arr[i].second = i;
			pre = temp;
		}
		sort(arr.begin(), arr.end());


		for (int i = n, j = 0; i >= 2; i--, j++) {
			ans[i] = arr[j].second;
		}
		cout << 1;
		for (int i = 2; i <= n; i++)
			cout << " " << ans[i];
		cout << endl;
	}
	return 0;
}
```
***
## D - Make Nonzero Sum (easy version)
**思维**
给出由1和-1组成的数列，奇数位加，偶数位减， 可以将数列任意分段，分段后每段的奇数位加，偶数位减，使得最后的总和为0，首先，如果1和-1的个数和不为偶数的话，可知无论如何都无法做到和为零，如果个数和为偶数的话，将第i个数和第i + 1个数两两配对（1 <= i < n），可知这两个数就只有相同和不相同两种情况，如果相同，如（1， 1），（-1， -1），那么将这两个数选为一个区间，就能使区间和为零，如果不同，如（1， -1），将这两个数分别选成一个区间，也能作到区间和为零，也就是说，如果n为偶数，那么必然有解，只需按以上方法输出区间位置即可。
```C++
#include <vector>
#include <iostream>
using namespace std;
int main() {
	int t;
	cin >> t;
	while (t--) {
		int n;
		cin >> n;
		vector<int> arr(n + 1);
		for (int i = 1; i <= n; i++) {
			cin >> arr[i];
		}
		if (n % 2 == 1) {
			cout << -1 << endl;
			continue;
		}
		int sum = 0;
		vector<int> ans;
		for (int i = 1; i <= n; i += 2) {
			if (arr[i] == arr[i + 1]) {
				ans.push_back(i);
				ans.push_back(i + 1);
			}
			else {
				ans.push_back(i);
				ans.push_back(i);
				ans.push_back(i + 1);
				ans.push_back(i + 1);
			}
		}
		cout << ans.size() / 2 << endl;
		for (int i = 0; i < ans.size(); i += 2) {
			cout << ans[i] << " " << ans[i + 1] << endl;
		}
	}

	return 0;
}
```
## E - Complementary XOR
**思维**
给出由0， 1组成的数列2个，将第一个数列的某一区间的0变成1，1变成0，第二个数列则根据第一个数列区间选择的情况，将剩下的区间的0变1，1变0.至多进行n + 5次操作，问能否将第一和第二数列同时全部置0。观察后发现，如何想同时置0，那么第一第二数列要么相同，要么相反。且如果满足以上条件，将第一数列置0后第二数列也是要么相同或者相反。那么只需要对第一数列进行操作就行，将数列的每一位分别拿出来看，如果是0不用管，如果是1，就单独将这个数选作一个区间。将第一数列遍历时统计非第一位的1的个数， 记作sum，遍历完后看sum + 数列二的第一个数 % 2看是否等于奇数，就能知道将第一数列置0后，第二数列是相同还是相反了，如果相反，在分别选择区间（1, n）（1， 1）（2， n）。
```C++
#include <vector>
#include <iostream>
using namespace std;
int main() {
	int t;
	cin >> t;
	while (t--) {
		int n;
		cin >> n;
		string str1, str2;
		cin >> str1 >> str2;
		vector<int> arr1(n + 1), arr2(n + 1);
		for (int i = 1; i <= n; i++) {
			arr1[i] = str1[i - 1] - '0';
			arr2[i] = str2[i - 1] - '0';
		}
		int sum = 0;
		int flag = 1;
		if (arr1[1] == arr2[1]) {
			for (int i = 2; i <= n; i++) {
				if (arr1[i] != arr2[i]) {
					cout << "NO" << endl;
					flag = 0;
					break;
				}
			}
		}
		else {
			for (int i = 2; i <= n; i++) {
				if (arr1[i] == arr2[i]) {
					cout << "NO" << endl;
					flag = 0;
					break;
				}
			}
		}

		vector<int> ans;
		if (flag) {
			if (arr1[1] == 1) {
				ans.push_back(1);
				ans.push_back(1);
			}
			int sum = 0;
			for (int i = 2; i <= n; i++) {
				if (arr1[i] == 1) {
					ans.push_back(i);
					ans.push_back(i);
					sum++;
				}
			}
			if ((sum + arr2 [1]) % 2 == 1) {
				ans.push_back(1);
				ans.push_back(n);
				ans.push_back(1);
				ans.push_back(1);
				ans.push_back(2);
				ans.push_back(n);
			}
			cout << "YES" << endl;
			cout << ans.size() / 2 << endl;
			for (int i = 0; i < ans.size(); i += 2) {
				cout << ans[i] << " " << ans[i + 1] << endl;
			}
		}
	}
	return 0;
}
```
## F - Number Game
**博弈，思维**
A在游戏前选定一个数K，在游戏时每回合可以选择一个比K小的数，而B可以将这个数移除后加到任意一个数上，在下一回合进行同样的操作，但是能选择数的大小减一，问K最大是多少。对于B来说，无论A选择的K是多少，在游戏的最后一个回合A必然要选择一个1，那个B只需要尽可能多的破坏剩下的1就行，而A则要尽可能的保护1，每次选择的时候，选则最大的能选择的数。推演后可知，数列中1的数量就是K的最大值，跑一遍模拟。
```C++
#include <vector>
#include <iostream>
using namespace std;
int main() {
	int t;
	cin >> t;
	while (t--) {
		int n;
		cin >> n;
		vector<int> NUM(n + 1, 0);
		for (int i = 1; i <= n; i++) {
			int t;
			cin >> t;
			NUM[t]++;
		}
		int k;
		for (k = NUM[1]; k >= 1; k--) {
			vector<int> num = NUM;

			int fail = 0;
			for (int i = k; i >= 1; i--) {
				if (num[i] < 1) {
					int flag = 0;
					for (int j = i - 1; j >= 1; j--) {
						if (num[j] > 0) {
							num[j]--;
							flag = 1;
							break;
						}
					}
					if (flag == 0) {
						fail = 1;
						break;
					}
				}
				num[1]--;
			}
			if (fail == 0) {
				break;
			}
		}
		cout << k << endl;
	}
	return 0;
}
```
## G- Scuza
**二分查找，前缀和**
爬台阶，如果腿长够，就接着爬，爬到腿不够长或者爬完为止，题目简单，但是卡时间，所以用二分查找和前缀和进行优化。用Max数组存该位置及其之前位置的最高台阶的值，用二分查找Max中第一次出现的比腿长大的值，该位置就是最多能到的位置，用前缀和直接得出答案。
```C++
#include <vector>
#include <iostream>
#include <algorithm>
using namespace std;
int main() {
	int t;
	cin >> t;
	while (t--) {
		int n, m, mmax = -1;
		cin >> n >> m;
		vector<long long> preFix(n + 1), Max(n + 1);
		for (int i = 1; i <= n; i++) {
			int t;
			cin >> t;
			preFix[i] = preFix[i - 1] + t;
			mmax = max(mmax, t);
			Max[i] = mmax;
		}
		for (int i = 1; i <= m; i++) {
			int t;
			cin >> t;
			auto it = upper_bound(Max.begin(), Max.end(), t);
			if (it == Max.end()) {
				cout << preFix[n] << " ";
			}
			else {
				int pos = &*it - &Max[0];
				cout << preFix[pos - 1] << " ";
			}
		}
		cout << endl;
	}
	return 0;
}
```

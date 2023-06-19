# 题解周报
## A - Prefix Sum Addicts
**思维**
长度n的数列，给出n ~ n-k的前缀和，问给出的数据是否合法。k个前缀和能确定k - 1个数的值，先看这些数是否已经排好序，在看由前缀和得出的最靠前的数，该数就是数列之前数的上界，因为下界没确定，所以贪心一下，将前面的数全部取最大值，看是否等于或超过目标值。
```C++
#include <vector>
#include <algorithm>
#include <iostream>
using namespace std;
const int inf = 0x3f3f3f3f;
int main() {
	int t;
	cin >> t;
	while (t--) {
		int n, k;
		cin >> n >> k;
		if (k == 1) {
			int t;
			cin >> t;
			cout << "YES" << endl;
			continue;
		}
		vector<long long> preFix(k + 1);
		for (int i = 1; i <= k; i++) {
			cin >> preFix[i];
		}

		vector<long long> arr(k + 1);
		for (int i = 2; i <= k; i++) {
			arr[i] = preFix[i] - preFix[i - 1];
		}

		if (arr.size() >= 3) {
			if (!is_sorted(arr.begin() + 2, arr.end())) {
				cout << "NO" << endl;
				continue;
			}
		}

		int times = n - k + 1;		
		if (times * arr[2] >= preFix[1]) {
			cout << "YES" << endl;
		}
		else {
			cout << "NO" << endl;
		}
	}
	return 0;
}

```

## B - Good Subarrays (Easy Version)
**思维**
如果一个数大于该数的下标，这个数就是好数，好数组成的连续片段就是好片段，问有几个好片段，好片段之间可以重叠。双指针题，左指针指向好片段要包含的数，右指针指向片段边界，然后右指针从右向左缩小，每次缩小计数加一（可以不跑循环直接加）。
```C++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;
const int MAXN = 2e5 + 5;
int arr[MAXN];
int main() {
	int t;
	scanf("%d", &t);
	while (t--) {
		int n;
		scanf("%d", &n);
		for (int i = 1; i <= n; i++) {
			scanf("%d", &arr[i]);
		}

		long long sum = 0;
		int i = 1, j = 1;
		while (i <= n)
		{
			for(; j <= n && arr[j] >= j - i + 1; j++) {
			}
			sum += j - i;
			i++;		
		}
		cout << sum << endl;
	}
	return 0;
}
```

## C - Playing with GCD
**数论，思维**
根据A数列构造B数列， a[i] = gcd(b[i], b[i + 1]),思路是先根据a[i]和a[i - 1]的最小公倍数求出b[i]，再用b[i]和b[i + 1]来验证a[i]是否成立。
```C++
#include <vector>
#include <iostream>
#include <algorithm>
using namespace std;
int gcd(int m, int n) {
	if (m % n == 0) return n;
	else return gcd(n, m % n);
}
int main() {
	int t;
	cin >> t;
	while (t--) {
		int flag = 1;
		int n;
		cin >> n;
		vector<int> a(n + 2);
		vector<int> b(n + 2);
		for (int i = 1; i <= n; i++) {
			cin >> a[i];
		}
		a[0] = 1, a[n + 1] = 1, b[0] = 1;
		for (int i = 0; i <= n; i++) {
			b[i + 1] = a[i] * a[i + 1] / gcd(a[i], a[i + 1]);
			int temp = gcd(b[i + 1], b[i]);
			if (temp != a[i]) {
				flag = 0;
				cout << "NO" << endl;
				break;
			}
		}
		if (flag) {
			cout << "YES" << endl;
		}

	}
	return 0;
}
```

## D - Removing Smallest Multiples
**贪心**
选择一个K，将下标为K的倍数的数中最小的数删除，代价为K。思路是从小到大选择K，看能否删除，直到不能删除为止。
```C++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;
int main() {
	int T;
	cin >> T;
	while (T--) {
		int n;
		cin >> n;
		string str;
		cin >> str;
		str = '*' + str;
		vector<bool> vis(n + 1, false);
		long long ans = 0;
		for (int i = 1; i <= n; i++) {
			for (int j = i; j <= n; j += i) {
				if (str[j] == '0') {
					if (vis[j] == false) {
						ans += i;
						vis[j] = true;
					}
				}
				else {
					break;
				}
			}
		}
		cout << ans << endl;
	}
	return 0;
}
```

# 省赛
## 概况：
去之前没怎么关心这次省赛，也不是很了解省赛题的难度，空手就去考了，结果可想而知。
和大三的几个人配合的还可以，毕竟才认识没几个周。但感觉当时我啥也没干，卡题的时候也没找新题去做，一直在想那一道题，确实不应该。
吃一堑长一智，但机会就那么多，这次已经浪费一次了，这一年得努努力了。
大三退役了感觉心里少了些东西，感觉我们该成长了，该成为我们以前心中的榜样了。
## 补题
D题：
```c++
#include<bits/stdc++.h>
#define int long long
#define MAXN 100100
typedef long long ll;
using namespace std;
pair<int, int> pa[MAXN];
inline bool check(int n,int v) {
	vector<int> vec;
	for (int i = 0;i<n;++i) {
		if (pa[i].first<v) {
			vec.push_back(pa[i].second);
		}
	}
	sort(vec.begin(), vec.end());
	for (int i = 0;i<n;++i) {
		if (vec.size()==0) {
			return true;
		}
		if (pa[i].first<v) {
			continue;
		}
		auto it = upper_bound(vec.begin(), vec.end(),(pa[i].first-v)+pa[i].second);
		if (it!=vec.begin()) {
			--it;
			vec.erase(it);
		}
	}
	if (vec.size()==0) {
		return true;
	}
	return false;
}
signed main() {
	ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);
	int t;
	cin >> t;
	while(t--){
		int n;
		cin >> n;
		for (int i = 0;i<n;++i) {
			cin >> pa[i].first >> pa[i].second;
		}
		sort(pa, pa + n);
		int l=pa[0].first, r = pa[n-1].first;
		sort(pa, pa + n, [](pair<int, int> p1, pair<int, int> p2) {
			if (p1.second == p2.second) {
				return p1.first > p2.first;
			}
			return p1.second > p2.second;
			});
		int mid = (l + r) / 2;
		int ans = 0;
		while (l<=r) {
			mid = (l + r)/2;
			if (check(n,mid)==true) {
				ans = mid;
				l = mid + 1;
			}
			else {
				r = mid - 1;
			}
		}
		cout << ans << endl;
	}
	return 0;
}
```

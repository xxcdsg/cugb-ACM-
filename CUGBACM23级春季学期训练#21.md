## [A. Maximize?](https://codeforces.com/contest/1968/problem/A)

* 暴力

## [B. Prefiquence](https://codeforces.com/contest/1968/problem/B)

* 取最长的 $a$ 的前缀为 $b$ 的子序列
* 贪心，一个一个字母找最近的

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;
 
#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;
 
const int inf = 1e9 + 10;
const ll INF = 1e18 + 10;
 
void test(){
	int n,m;cin >> n >> m;
	string a,b;cin >> a >> b;
	int l = 0;
	int i;
	for(i = 0;i < n;i++){
		while(l < m && a[i] != b[l]) l++;
		l++;
		if(l > m){
			break;
		}
	}
	cout << i << '\n';
}
 
int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [C. Assembly via Remainders](https://codeforces.com/contest/1968/problem/C)

* 给出 $x$ 序列，求 $a$ 序列使得对于任意的 $2\leq i\leq n$ 都有 $a[i]\ \% \ a[i-1]=x[i]$

##### 思路：

* 那么就是要满足
* $a[i]=ka[i-1]+x[i]$ ，并且为了让求余能够取到 $x[i]$ ，要满足 $x[i] < a[i-1]$ ，那么我们取 $a[i]$ 值的时候尽量取大一点即可
* 那么我们取 $a[1]=501$ ，之后每个数都取 $a[i]=a[i-1]+x[i]$ ，即可

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;

#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;

const int inf = 1e9 + 10;
const ll INF = 1e18 + 10;

void test(){
	int n;cin >> n;
	vector<int> x(n + 1);
	for(int i = 2;i <= n;i++) cin >> x[i];
	vector<int> a(n + 1);
	a[1] = 501;
	for(int i = 2;i <= n;i++){
		a[i] = x[i] + a[i - 1];
	}
	for(int i = 1;i <= n;i++)
		cout << a[i] << ' ';
	cout << '\n';
}

int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [D. Permutation Game](https://codeforces.com/contest/1968/problem/D)

* 两个玩家初始位置不同
* 每次可以选择从 $x$ 位置跳到 $p[x]$ 位置，或者不跳
* 并且每回合会加上 $a[x]$ 
* 两个玩家都采取最佳决策，问哪个玩家在 $k$ 回合之后分数最大

##### 思路：

* 对于一人人来说，对它来说的最佳决策肯定是跳到某个最大位置，然后一直停在那里
* 当然，考虑到中间可能经过较小数，不一定会跳到整个排列的最大位置
* 并且因为数的数量最多就是 $n$ ，因此我们不需要跳 $k$ 次
* 我们只要考虑跳前 $min(n,k)$ 次即可

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;
 
#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;
 
const int inf = 1e9 + 10;
const ll INF = 1e18 + 10;
 
void test(){
	int n,k,pb,ps;cin >> n >> k >> pb >> ps;
	vector<ll> p(n + 1),a(n + 1),ma(n + 1,0),sum(n + 1,0);
	for(int i = 1;i <= n;i++) cin >> p[i];
	for(int i = 1;i <= n;i++) cin >> a[i];
	ll ma1 = 0,ma2 = 0;
	for(int i = 1;i <= min(k,n);i++){
		sum[i] = sum[i - 1] + a[pb];
		ma[i] = max(ma[i - 1],a[pb]);
		ma1 = max(ma1,(k - i) * ma[i] + sum[i]);
		pb = p[pb];
	}
	for(int i = 1;i <= min(k,n);i++){
		sum[i] = sum[i - 1] + a[ps];
		ma[i] = max(ma[i - 1],a[ps]);
		ma2 = max(ma2,(k - i) * ma[i] + sum[i]);
		ps = p[ps];
	}
	if(ma1 > ma2) cout << "Bodya\n";
	else if(ma1 == ma2) cout << "Draw\n";
	else cout << "Sasha\n";
}
 
int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [E. Cells Arrangement](https://codeforces.com/contest/1968/problem/E)

* 在 $n*n$ 的网格中选 $n$ 个点
* 问集合 $\set{dis|dis=|x_i-x_j|+|y_i-y_j|}$ 的最大尺寸

##### 思路：

* 首先我们考虑 $dis$ 的范围 $[0,2n-2]$ ，其实数量并不多
* 假设 $n$ 比较大时，我们能否将所有数都取到呢
* 首先取 $1$ ，那么我们假设取 $(1,1),(1,2)$
* 那么之后我们取点，假设距离 $(1,2)$ 为 $x$ ，距离 $(1,1)$ 就是 $x+1$ 了，也就是说我们可以每次取两个 $dis$
* 那么要取完所有数就需要 $(2n-2)/2+2$ 个位置，就是 $n+1$ 个位置，这当然不行
* 我们想到，因为我们一开始是取了两个相邻的数，因此除数为 $2$ ，那么我们取多一点数，取三个即可
* 特判较小的情况

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;

#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;

const int inf = 1e9 + 10;
const ll INF = 1e18 + 10;

void test(){
	int n;cin >> n;
	cout << "1 1\n";
	cout << "1 2\n";
	if(n == 3) cout << "2 3\n";
	else if(n == 4){
		cout << "4 2\n";
		cout << "4 4\n";
	}else if(n != 2){
		cout << "2 2\n";
		int l = 2,r = 2;
		for(int i = 4;i <= n;i++){
			for(int j = 1;j <= 3;j++){
				if(l <= r) l++;
				else r++;
			}
			l = min(l,n);
			r = min(r,n);
			cout << l << ' ' << r << '\n';
		}
	}
}

int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [F. Equal XOR Segments](https://codeforces.com/contest/1968/problem/F)

* 给出一个长度为 $n$ 的数组 $x$
* 每一个问给出 $l,r$ ，问能否将 $x_l...x_r$ 段分成至少两个连续段，段的异或和相同

##### 思路：

* 假设分成偶数段，那么将所有数都异或起来，必定为 $0$ ，因此当段的异或值为 $0$ 时，必然可以
* 否则肯定多一段异或值，因此整段的异或值就是分成每段的异或值
* 我们再二分找两个出来即可（一个不行，有可能后面都是 $0$ ）

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;
 
#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;
 
const int inf = 1e9 + 10;
const ll INF = 1e18 + 10;
 
void test(){
	int n,q;cin >> n >> q;
	vector<int> a(n + 1);
	for(int i = 1;i <= n;i++) cin >> a[i];
	vector<vector<int>> sum(n + 1,vector<int>(31,0));
	vector<pair<int,int>> xora(n + 1);
	for(int i = 1;i <= n;i++){
		xora[i].first = xora[i - 1].first ^ a[i];
		xora[i].second = i;
		for(int j = 0;j <= 30;j++){
			sum[i][j] = sum[i - 1][j];
			if(a[i] & (1 << j))
				sum[i][j]++;
		}
		a[i] ^= a[i - 1];
	}
	sort(xora.begin() + 1,xora.end());
	while(q--){
		int l,r;cin >> l >> r;
		int tem = 0;
		for(int i = 0;i <= 30;i++){
			if((sum[r][i] - sum[l - 1][i]) % 2 != 0)
				tem |= (1 << i);
		}
		if(tem == 0) cout << "Yes\n";
		else{
			tem ^= a[l - 1];
			auto it = lower_bound(xora.begin(),xora.end(),make_pair(tem,l));
			if(it != xora.end() && it -> second <= r - 1 && it -> first == tem){
				it = lower_bound(xora.begin(),xora.end(),make_pair(a[l - 1],it -> second + 1));
				if(it != xora.end() && it -> second <= r - 1 && it -> first == a[l - 1]){
					cout << "Yes\n";
				}else
					cout << "No\n";
			}
			else
				cout << "No\n";
		}
	}
}
 
int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [G1. Division + LCP (easy version)](https://codeforces.com/contest/1968/problem/G1)

* 给出一个字符串，将其分成 $k$ 连续段（不能跳过如何字母），问它们的最长公共前缀为什么

##### 思路：

* 二分答案，然后一个一个匹配，匹配不上用 $kmp$ 得出的前缀数组转移而非重新置为 $0$

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;
 
#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;
 
const int inf = 1e9 + 10;
const ll INF = 1e18 + 10;
 
const int MAXN = 2e5 + 10;
 
int nex[MAXN];
void fnex(const string &t)
{
	int end = t.size();
	nex[0] = -1;
	int ptop = -1;
	int i = 0;
	while(i < end)
	{
		if(ptop == -1 || t[i] == t[ptop])
			nex[++i] = ++ptop;
		else
			ptop = nex[ptop];
	}
}
 
void test(){
	int n,l,r;cin >> n >> l >> r;
	string s;cin >> s;
	fnex(s);
	int k = l;
	int _l = 1,_r = n / k;
	while(_l <= _r){
		int mid = (_l + _r) / 2;
		int num = 0,no = 0;
		for(int i = 0;i < n;i++){
			if(no == -1 || s[i] == s[no]){
				no++;
				if(no == mid){
					no = 0;
					num++;
				}
			}else{
				i--;
				no = nex[no];
			}
		}
		if(num >= k) _l = mid + 1;
		else _r = mid - 1;
	}
	cout << _r << '\n';
}
 
int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [A. Two Friends](https://codeforces.com/problemset/problem/1969/A)

* 只有 $i$ 与 $p[i]$ 的人被邀请时， $i$ 人会来，问至少两人来的的最少邀请数量

##### 思路：

* 正常邀请两个人，那么选 $i,p[i],p[p[i]]$ ，答案就是 $3$
* 如果 $p[p[i]]=i$ ，那么就可以邀请两个人即可
* 因此只要找是否存在 $p[p[i]]=i$ 的即可

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

const int N = 51;
 
int a[N];
 
int find(int x){
	if(a[x] == x) return x;
	else return a[x] = find(a[x]);
}

void test(){
	int n;cin >> n;
	for(int i = 1;i <= n;i++){
		cin >> a[i];
	}
	bool f = 0;
	for(int i = 1;i <= n;i++){
		if(a[a[i]] == i) f = 1;
	}
	if(f) cout << 2 << '\n';
	else cout << 3 << '\n';
}

int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [B. Shifts and Sorting](https://codeforces.com/problemset/problem/1969/B)

* 可选一个区间，将区间内最后一个字符放在第一个，其他字符都向后移动一位，该操作的成本为区间的大小
* 问将字符串变为非降序排序的最低总成本
* 该字符串只包含 $01$

##### 思路：

* 相当于将所有的 $0$ 放到 $1$ 之前
* 那么对于一个 $0$ 来说，它想要移动到前面所有 $1$ 的左边，它的最小花费就是前面所有 $1$ 的数量加上它本身的一个
* 如果我们贪心地做，从左到右一个一个移动 $0$ 到最前面，那么恰好能让所有 $0$ 的花费就为最小值

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
	string s;cin >> s;
	ll num = 0,ans = 0;
	bool f = 0;
	for(auto c : s){
		if(c == '1'){
			num++;
			f = 1;
		}
		else if(f) ans += num + 1;
	}
	cout << ans << '\n';
}
 
int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [C. Minimizing the Sum](https://codeforces.com/problemset/problem/1969/C)

* 给一个序列，能进行将一个数变为相邻的数的操作 $k$ 次，问数组最小的可能总和为多少

##### 思路：

* dp
* 因为 $k$ 取值很小，我们设 $dp[i][x]$ 代表到 $i$ 位置，操作了 $x$ 次
* 那么转移方程就有 $dp[i][x]=min\set{dp[i-j][x-j+1]+(i-j)*min\set{a[i],a[i-1],...a[i-j+1]},dp[i][x]}$
* 取 $min\set{a[i],a[i-1],...a[i-j+1]}$ 可通过循环中直接求，或者用其他方法

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;
 
#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;
#define lson (x << 1)
#define rson (x << 1 | 1)
#define int long long
 
const int inf = 1e9 + 10;
const ll INF = 1e18 + 10;
 
const int MAXN = 3e5 + 10;
ll mii[MAXN][11];
ll a[MAXN];
 
void test(){
	int n,k;cin >> n >> k;
	vector<vector<ll>> dp(n + 1,vector<ll>(11,0));
	for(int i = 1;i <= n;i++) cin >> a[i];
	
	for(int i = 1;i <= n;i++){
		mii[i][0] = a[i];
		for(int j = 1;j <= k;j++){
			mii[i][j] = mii[i][j - 1];
			if(i + j <= n)
				mii[i][j] = min(mii[i][j],a[j+i]);
		}
	}
	
	for(int i = 1;i <= n;i++){
		for(int j = 0;j <= k;j++){//目标使用
			ll mi = INF;
			for(int t = max(i - j,(ll)1);t <= i;t++){//跳跃位置
				mi = min(dp[t - 1][j - (i - t)] + mii[t][i-t] * (i - t + 1),mi);
			}
			dp[i][j] = mi;
		}
	}
	ll mi = INF;
	for(int i = 0;i <= k;i++)
		mi = min(mi,dp[n][i]);
	cout << mi << '\n';
}
 
signed main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [C. Everything Nim](https://codeforces.com/problemset/problem/1966/C)

*  $n$ 个石堆，每次可选一个数 $k$ 将所有非空石堆去掉 $k$ 个石头， $k$ 最大取所有非空石堆石头数量的最小值
* 不能操作即为失败
*  $A$ 先操作， $B$ 后操作，问那个人会胜利

##### 思路：

* 首先，相同石头数量的石堆本质上是一样的
* 其次，假设某个人操作时 $k$ 的取值可大于 $1$ ，那么它肯定就赢了
* 因为它可以决定之后去掉现在非空石堆石头数量的最小值后是先手还是后手

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
	vector<int> a(n + 1);
	for(int i = 1;i <= n;i++){
		cin >> a[i];
	}
	a[0] = 0;
	sort(a.begin(),a.end());

	vector<int> b;

	for(int i = 1;i <= n;i++){
		if(a[i] != a[i - 1])
			b.push_back(a[i] - a[i - 1]);
	}

	bool f = 1;

	int len = b.size();

	for(int i = 0;i < len - 1;i++){
		if(b[i] == 1) f = !f;
		else break;
	}
	
	if(f) cout << "Alice\n";
	else cout << "Bob\n";
}

int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [D. Shop Game](https://codeforces.com/problemset/problem/1969/D)

*  $A$ 可以买东西倒卖给 $B$
* 每个商品对 $A$ 来说的价值为 $a_i$ ，对 $B$ 来说的价值为 $b_i$
*  $B$ 可免费拿走至多 $k$ 件 $A$ 倒卖的商品
*  $B$ 必须拿走或者买走所有 $A$ 倒卖的商品
*  $A$ 想最大化利润， $B$ 想最小化 $A$ 的利润
* 问 $A$ 最后的利润为多少

##### 思路：

* 首先，无论如何， $B$ 免费取走的肯定是 $b_i$ 最大的 $k$ 个 $A$ 提供的商品
* 对 $A$ 来说，如果 $a[i] < b[i]$ ，并且这个商品 $B$ 不是免费取到的，那么这个商品它肯定是直接拿的
* 因此分成两种商品，第一种， $b_i$ 大的取 $k$ 个
* 第二种，所有的比后面的 $b_i$ 小的，并且 $a[i] < b[i]$
* 那么我们先按 $b[i]$ 来排序，然后预处理出所有位置，前面所有 $a[i] < b[i]$ 的贡献
* 然后从后往前拿较小 $k$ 个 $b[i]$

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
 
bool cmp(pair<int,int> a,pair<int,int> b){
	if(a.second == b.second) return a.first < b.first;
	else
		return a.second > b.second;
}
 
void test(){
	int n,k;cin >> n >> k;
	vector<pair<int,int>> pa(n + 1);
	for(int i = 1;i <= n;i++)
		cin >> pa[i].first;
	for(int i = 1;i <= n;i++)
		cin >> pa[i].second;
	sort(pa.begin() + 1,pa.end(),cmp);
	ll ans = 0;
	vector<ll> suf(n + 2,0);
	for(int i = n;i >= 1;i--){
		suf[i] = max(suf[i + 1],suf[i + 1] + (pa[i].second - pa[i].first));
	}
	ll sum = 0;
	priority_queue<int,vector<int>,less<int>> pqu;
	for(int i = 1;i <= k;i++){
		pqu.push(pa[i].first);
		sum -= pa[i].first;
	}
	for(int i = k + 1;i <= n;i++){
		ans = max(ans,sum + suf[i]);
		if(!pqu.empty() && pa[i].first < pqu.top()){
			sum += pqu.top();
			sum -= pa[i].first;
			pqu.pop();
			pqu.push(pa[i].first);
		}
	}
	cout << ans << '\n';
}
 
int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

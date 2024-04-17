## [A. Painting the Ribbon](https://codeforces.com/contest/1954/problem/A)

*  $A$ 可给 $n$ 部分涂最多 $m$ 种不同颜色， $B$ 最多可变 $k$ 部分为某种任意颜色，在 $A$ 采取最不利于 $B$ 操作的情况下， $B$ 能否将 $n$ 部分变成同一种颜色

##### 思路：

* 假设存在在 $n$ 部分的颜色中数量最多的数为 $ma$ ，那么 $B$ 最多只要将剩下的 $n-ma$ 都变成最多的颜色即可，那么 $A$ 就要让这个 $n-ma$ 尽量大，即让 $ma$ 小，而 $ma$ 最小为 $\lceil n/m \rceil$

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
	int n,m,k;cin >> n >> m >> k;
	int tem = (n - 1) / m + 1;
	if(n - tem <= k) cout << "No\n";
	else cout << "Yes\n";
}

int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [B. Make It Ugly](https://codeforces.com/contest/1954/problem/B)

* 我们称一直选 $2\leq i \leq|a|-1$ 其满足 $a[i-1]=a[i+1]$ ，进行 $a[i]=a[i-1]$ 的存在，最终能使得 $a$ 数组都变为同一个数，这样的数组为好数组
* 先给你一个好数组，问最少删除多少的数能使得数组不为好数组

##### 思路：

* 假设这个数为 $x$ ，那么当有两个连续非 $x$ 相连或者最边缘是非 $x$ 时，该数组不为好数组
* 为了使得好数组变为非好数组，那么我们就只要删除连续的 $x$ 即可，找出最短的连续 $x$ 的长度即为答案
* 注意当数组本身都是由 $x$ 组成的，那么无论删除多少，都为好数组

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
	vector<int> a(n + 2,-1);
	for(int i = 1;i <= n;i++) cin >> a[i];
	int len = 0;
	int mi = inf;
	for(int i = 1;i <= n + 1;i++){
		if(a[i] == a[1]) len++;
		else{
			mi = min(mi,len);
			len = 0;
		}
	}
	bool f = 0;
	for(int i = 1;i <= n;i++)
		if(a[i] != a[1]) f = 1;
	if(!f)
		cout << -1 << '\n';
	else
		cout << mi << '\n';
}

int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [C. Long Multiplication](https://codeforces.com/problemset/problem/1954/C)

* 给出两个长度相同的数，可以交换对应十进制位上的数，问它们的乘积最大时，两个数分别可以为多少（所给的数十进制上的数没有 $0$ ）

##### 思路：

* 交换对应十进制位上的数，那么它们两个的和固定不变 $x+y=z$
* 那么易知当 $x\to z$ 时， $xy$ 越大
* 那么我们的目标就是要让这两个数的差距尽量小
* 那么就只要让最高的不同位的较大的给一个数，其余的较大的给另外一个数即可让两个数的差距最小

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
	string x,y;cin >> x >> y;
	int len = x.size();
	int i = 0;
	for(;i < len;i++){
		if(x[i] != y[i]){
			if(x[i] < y[i]) swap(x[i],y[i]);
			i++;
			break;
		}
	}
	for(;i < len;i++){
		if(x[i] > y[i]) swap(x[i],y[i]);
	}
	cout << x << '\n' << y << '\n';
}

int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [D. Colored Balls](https://codeforces.com/problemset/problem/1954/D)

*  $n$ 种不同颜色的球，每种球有 $a_i$ 的数量
* 我们选中某些颜色，那么这个颜色集合的所有球的贡献为最少分组个数
* 分组每组最多两个球，并且这两个球颜色不能相同（或者单独一个球为一组）
* 问所有颜色集合的贡献之和
* 球的总数不超过 $5000$ ，颜色种类不超过 $5000$

##### 思路：

* 计数 $dp$
* 我们假设颜色最多的球的数量为 $x$ ，剩下的球的数量为 $y$
* 如果 $x \leq y$ ，那么每种颜色的球都可以与不同颜色的球匹配(奇数的多一个)，那么贡献就为 $\lceil \frac{x+y}{2} \rceil$
* 如果 $x > y$ ，那么颜色最多的球就算将其他颜色的球全匹配完了，还剩下一些，那么它的贡献就为 $x$
* 颜色最多的球可以从小到大枚举
* 剩下的球用计数 $dp$ 维护

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
const int mod = 998244353;

ll qpow(ll x,ll y){
	ll res = 1;
	while(y){
		if(y&1) res = (res * x) % mod;
		x = (x*x) % mod;
		y >>= 1;
	}
	return res;
}

void test(){
	int n;cin >> n;
	vector<int> a(n + 1);
	for(int i = 1;i <= n;i++)
		cin >> a[i];
	sort(a.begin(),a.end());
	ll ans = 0;
	vector<ll> dp(5001);
	dp[0] = 1;
	for(int i = 1;i <= n;i++){
		for(int j = 5000-a[i];j >= 0;j--){
			ans += dp[j] * max(a[i],(j + a[i] + 1) / 2) % mod;
			ans %= mod;
			dp[j + a[i]] += dp[j];
			dp[j + a[i]] %= mod;
		}
	}
	cout << ans << '\n';
}

int main(){
	IOS
	int t = 1;//cin >> t;
	while(t--){
		test();
	}
}
```

## [A. Nene's Game](https://codeforces.com/problemset/problem/1956/A)

* 每次同时删除第 $a_1,a_2,...a_k$ 个玩家
* 如果不存在第 $a_i$ 个就跳过
* 给出初始玩家数量，问最后玩家数量

##### 思路：

* 输出 $min(n,min({a_1,a_2,...a_k})-1)$

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
	int k,q;cin >> k >> q;
	int mi = inf;
	for(int i = 1;i <= k;i++){
		int x;cin >> x;
		mi = min(mi,x);
	}
	for(int i = 1;i <= q;i++){
		int n;cin >> n;
		cout << min(mi - 1,n) << ' ';
	}
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

## [B. Nene and the Card Game](https://codeforces.com/problemset/problem/1956/B)

*  $2n$ 张牌， $1到n$ 均有两张，每次一个人打一张牌，如果桌子上有相同的牌，那么那个人加一分
* 你先手
* 对方会采取最优操作使得自己的分数尽量高，当有多种最优取法，那么采取最小化你的分数的操作

##### 思路：

* 你有多少单张，对方也有多少，你有多少双张，对方也有多少，那么对方只要模仿你的操作即可，那么你的分数最小就为双张的数量

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
	for(int i = 1;i <= n;i++) cin >> a[i];
	sort(a.begin(),a.end());
	int ans = 0;
	for(int i = 1;i < n;i++){
		if(a[i] == a[i + 1]) ans++;
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

## [C. Nene's Magical Matrix](https://codeforces.com/problemset/problem/1956/C)

* 给一个 $n$ ，代表 $n*n$ 的矩阵 $a$ ，初始值均为 $0$ ，每次可以将一列或一行置为一个任意排列 $p$
* 问矩阵内的数的和最大为多少

##### 思路：

* 我们观察一下
* 这是 $n=1$

| 1    |
| ---- |

* 这是 $n=2$

| 1    | 2    |
| ---- | ---- |
| 2    | 2    |

* 这是 $n=3$

| 1    | 2    | 3    |
| ---- | ---- | ---- |
| 2    | 2    | 3    |
| 3    | 3    | 3    |

* 那么你就任意构造了，就是先处理外部，再处理内部即可
* 那么为什么这样是最大的呢，详见https://codeforces.com/blog/entry/128426

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
	int ans = 0;
	for(int i = 1;i <= n;i++)
		ans += i * (2*i - 1);
	cout << ans << ' ' << 2*n << '\n';
	for(int i = n;i >= 1;i--){
		cout << 1 << ' ' << i << ' ';
		for(int j = 1;j <= n;j++)
			cout << j << ' ';
		cout << '\n';
		cout << 2 << ' ' << i << ' ';
		for(int j = 1;j <= n;j++)
			cout << j << ' ';
		cout << '\n';
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

## [D. Nene and the Mex Operator](https://codeforces.com/contest/1956/problem/D)

* 给一个长度为 $n$ 的序列 $a$
* 每次你可以进行操作，选中一个区间 $[l,r]$ ，使得 $a[l]=a[l+1]=...=a[r]=MEX(\{a[l],a[l+1],...a[r]\})$
* 问 $a$ 序列所有元素和的最大值为多少

##### 思路：

* 首先，对于一串长度为 $n$ 的序列 $0$ 来说，它最大能变为多少
* 我们可以发现，通过一定的步骤，我们最大可以让任意长度的序列的元素均变为序列长度
* 那么只要确定要变化的区间即可，利用二进制即可

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

void init(int l,int r,queue<pair<int,int>> &qu){
	for(int i = l;i <= r;i++)
		qu.push({i,i});
}

void func(int l,int r,queue<pair<int,int>> &qu){
	if(l == r){
		qu.push({l,r});
		qu.push({l,r});
	}
	else{
		for(int i = r;i > l;i--){
			func(l + 1,i,qu);
		}
		qu.push({l,l});
		qu.push({l,r});
	}
}

void test(){
	int n;cin >> n;
	vector<int> a(n + 1);
	queue<pair<int,int>> qu;
	for(int i = 0;i < n;i++){
		cin >> a[i];
		if(a[i] == 0){
			qu.push({i,i});
		}
		a[i] = max(1,a[i]);
	}
	int ma = 0,mai = 0;
	for(int i = 0;i < (1 << n);i++){
		bool f = 0;
		int len = 0;
		int ans = 0;
		for(int k = 0;k < n;k++){
			bool ff = ((i >> k) & 1);
			if(!f && !ff){
				ans += a[k];
			}else if(!f && ff){
				len++;
				f = 1;
			}else if(f && !ff){
				len++;
			}else{
				len++;
				ans += len * len;
				f = 0;
				len = 0;
			}
		}
		if(ans > ma){
			ma = ans;
			mai = i;
		}
	}
	bool f = 0;
	int l = 0;
	for(int k = 0;k < n;k++){
		bool ff = ((mai >> k) & 1);
		if(!f && ff){
			l = k;
			f = 1;
		}else if(f && ff){
			func(l,k,qu);
			f = 0;
		}
	}
	cout << ma << ' ' << (int)qu.size() << '\n';
	while(!qu.empty()){
		cout << qu.front().first + 1 << ' ' << qu.front().second + 1 << '\n';
		qu.pop();
	}
}

int main(){
	IOS
	int t = 1;//cin >> t;
	while(t--){
		test();
	}
}
```

* 我这里是二进制区间，当然你也可以二进制代表是否要进行操作

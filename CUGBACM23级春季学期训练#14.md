## [B. Progressive Square](https://codeforces.com/contest/1955/problem/B)

*  $a[i+1][j]=a[i][j]+c,a[i][j+1]=a[i][j]+d$
* 问给出的 $b$ 数组能否匹配上 $a$

##### 思路：

*  $c,d$ 均大于 $0$ ，因此 $a[1][1]$ 肯定是最小的数

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
	int n,c,d;cin >> n >> c >> d;
	vector<int> b(n*n);
	for(int &_ : b) cin >> _;
	sort(b.begin(),b.end());
	vector<int> e(n*n);
	int ptop = 0;
	for(int i = 0;i < n;i++)
		for(int j = 0;j < n;j++)
			e[ptop++] = i*c + j*d + b[0];
	sort(e.begin(),e.end());
	bool f = 1;
	for(int i = 0;i < n*n;i++){
		if(e[i] != b[i]) f = 0;
	}
	if(f) cout << "Yes\n";
	else cout << "No\n";
}

int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [C. Inhabitant of the Deep Sea](https://codeforces.com/contest/1955/problem/C)

* 每次攻击消耗一点耐久值，先攻击第一艘船，然后攻击最后一艘，然后再攻击第一艘，一共攻击 $k$ 次，给出所有船的耐久 $a$ ，问最后会被击沉几艘船

##### 思路：

* 假设当前第一艘船为 $l$ ，最后一艘为 $r$ ，剩下的攻击次数为 $k$ ,那么改变状态需要的攻击次数就是 $min\{a[l]，a[r]，k/2\}$ ，最多改变船的数量，也就是 $n$ ，比暴力枚举好多了

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
	ll k;cin >> k;
	vector<ll> a(n);
	for(ll &_ : a) cin >> _;
	int l = 0,r = n - 1;	
	while(l < r && k){
		if(k == 1){
			a[l]--;
			k--;
			break;
		}
		ll mi = min({a[l],a[r],k / 2});
		a[l] -= mi;
		a[r] -= mi;
		k -= mi * 2;
		if(a[l] == 0)
			l++;
		if(a[r] == 0)
			r--;
	}
	if(l == r && k){
		a[l] -= min(a[l],k);
	}
	int ans = 0;
	for(int i = 0;i < n;i++)
		if(a[i] == 0)
			ans++;
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

## [D. Inaccurate Subsequence Search](https://codeforces.com/contest/1955/problem/D)

* 给出长度为 $n$ 的序列 $a$ 与长度为 $m$ 的 $b$ 序列
* 我们称一个长度为 $m$ 的序列 $c$ 为好序列当这个序列存在一种排列使得至少有 $k$ 个元素与序列 $b$ 对应位置的元素相同
* 问序列 $a$ 中所有长度为 $m$ 的连续序列中有多少是好序列

##### 思路：

* 每当 $l$ 移动时， $c$ 改变的数只有两个，就是去掉 $c[l]$ ，增加 $c[l+m]$
* 我们维护每个数出现的次数，如果 $c$ 中的数在 $b$ 中也出现了，那么就根据出现次数计算贡献

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
	vector<int> a(n + 1);
	map<int,int> mp;
	for(int i = 1;i <= n;i++)
		cin >> a[i];
	for(int i = 1;i <= m;i++){
		int x;cin >> x;
		mp[x]++;
	}
	for(int i = 1;i <= m;i++){
		mp[a[i]]--;
		if(mp[a[i]] >= 0)
			k--;
	}
	int ans = 0;
	if(k <= 0) ans++;
	for(int i = m + 1;i <= n;i++){
		mp[a[i - m]]++;
		if(mp[a[i - m]] > 0)
			k++;
		mp[a[i]]--;
		if(mp[a[i]] >= 0)
			k--;
		if(k <= 0)
			ans++;
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

## [E. Long Inversions](https://codeforces.com/contest/1955/problem/E)

* 给一个长度为 $n$ 的 $01$ 字符串，每次可翻转连续 $k$ 个 $01$ 字符，问能够变成全 $1$ 字符串的最大 $k$ 值是多少

##### 思路：

* 首先，假设我们已经确定了 $k$ ，那么我们要如何将一个字符串变成 $01$ 字符串？
* 假设我们翻转的区间起点为 $l[1],l[2],l[3]...$
* 如果我们翻转的区间是相同的，那么两次翻转同一个区间相当与没有翻转，因此我们可以认为区间均不同
* 然后我们假设最左边的区间的起点为 $l[1]$ ，同时最左边的 $0$ 位置为 $i_0$ ，那么我们可以想见，为了不让左边的 $1$ 变成 $0$ ，但是这个位置又是要翻转变成 $1$ ，因此 $l[1]=i_0$
* 但是如果我们直接这样暴力进行肯定不行（不过我没试过），我们用前缀和来处理区间的翻转
* 因为 $n$ 较小，因此暴力枚举 $k$ ，然后前缀和 $O(n)$ 测试是否可行即可

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
	string s;cin >> s;
	vector<int> a(n + 2,0);
	vector<int> pre(n + 2);
	function<void()> init = [&](){
		for(int i = 1;i <= n;i++){
			a[i] = s[i - 1] - '0';
			if(a[i] != a[i - 1])
				pre[i] = 1;
			else
				pre[i] = 0;
		}
	};
	for(int i = n;i >= 1;i--){
		init();
		for(int j = 1;j <= n - i + 1;j++){
			pre[j] += pre[j - 1];
			if(pre[j] % 2 == 0){
				pre[j]++;
				pre[j + i]--;
			}
		}
		bool f = 1;
		for(int j = n - i + 2;j <= n;j++){
			pre[j] += pre[j - 1];
			if(pre[j] % 2 == 0) f = 0;
		}
		if(f){
			cout << i << '\n';
			break;
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

## [G. GCD on a grid](https://codeforces.com/contest/1955/problem/G)

* 给出 $n*m$ 的矩阵 $a$ ，要从 $(1,1)$ 跑到 $(n,m)$ ，每次只能向下或者向右跑，问路径上所有数的 $gcd$ 的最大可能值为多少

##### 思路：

* 跑到某个点，此时的 $gcd$ 值只可能为它的因子， $1e6$ 内的数因子数量不多，我们用 $set$ 存，然后 $bfs$ 推
* 但是会 $T$
* 那么我们继续优化，可以想到，因为我们求的是最大的 $gcd$ ，那么假设跑到这里的 $gcd$ 有 $1,2,4,8,16$ ，因为 $1,2,4,8$ 同时是 $16$ 的因子，无论下一个数为多少，它得出的 $gcd$ 肯定比 $1,2,4,8$ 的因子更大，或者相同，因此就没必要保留 $1,2,4,8$，因此我们每次给新位置更新完 $gcd$ 后，再从大到小检查一遍，将那些是其他 $gcd$ 的因子的数去掉即可减少后面转移的复杂度

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

int gcd(int x,int y){
	if(y == 0) return x;
	else return gcd(y,x % y);
}

void test(){
	int n,m;cin >> n >> m;
	vector<vector<set<int>>> gc(n + 1,vector<set<int>>(m + 1));
	vector<vector<int>> a(n + 1,vector<int>(m + 1));
	for(int i = 1;i <= n;i++)
		for(int j = 1;j <= m;j++)
			cin >> a[i][j];
	gc[1][1].insert(a[1][1]);
	for(int st = 3;st <= n + m;st++){
		for(int i = max(1,st - m);i <= n && st - i >= 1;i++){
			int j = st - i;
			if(i >= 2){
				for(int x : gc[i - 1][j]){
					gc[i][j].insert(gcd(x,a[i][j]));
				}
			}
			if(j >= 2){
				for(int x : gc[i][j - 1]){
					gc[i][j].insert(gcd(x,a[i][j]));
				}
			}
			set<int> ne;
			for(auto it = gc[i][j].rbegin();it != gc[i][j].rend();it++){
				bool f = 1;
				for(auto x : ne){
					if(x % (*it) == 0)
						f = 0;
				}
				if(f)
					ne.insert(*it);
			}
			gc[i][j] = ne;
		}
	}
	cout << *gc[n][m].rbegin() << '\n';
}

int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

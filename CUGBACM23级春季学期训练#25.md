## [**A - Buildings**](https://atcoder.jp/contests/abc353/tasks/abc353_a?lang=en)

* 找

## [**B - AtCoder Amusement Park**](https://atcoder.jp/contests/abc353/tasks/abc353_b)

* 尽量拿，拿不了答案 $+1$ 重置

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
	int n,x;cin >> n >> x;
	int no = x,num = 1;
	for(int i = 1;i <= n;i++){
		int y;cin >> y;
		if(y > no){
			no = x;
			num++;
			no -= y;
		}else{
			no -= y;
		}
	}
	cout << num << '\n';
}

int main(){
	IOS
	int t = 1;//cin >> t;
	while(t--){
		test();
	}
}
```

## [**C - Sigma Problem**](https://atcoder.jp/contests/abc353/tasks/abc353_c)

* $f(x,y)=(x+y)\ mod\ 1e8$
* 求 $\sum_{i=1}^{N-1}\sum_{j=i+1}^{N}f(A_i,A_j)$
* 其中 $1\leq A_i < 1e8$

##### 思路：

* 因为 $x+y<2e8$ ，如果 $x+y\geq 1e8$ ，那么相当于减去 $1e8$
* 因此我们统计所有组合中大于 $1e8$ 的数量，之后减去即可

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
	set<ll> se;
	ll ans = 0;
	int n;cin >> n;

	vector<ll> a(n + 1,0);
	for(int i = 1;i <= n;i++) cin >> a[i];
	sort(a.begin(),a.end());

	ll sum = 0;
	for(int i = 1;i <= n;i++){
		ans += (i - 1) * a[i];
		ans += sum;

		int tem = 1e8 - a[i] + 0.01;

		int p = lower_bound(a.begin(),a.end(),tem) - a.begin();

		int num = max(0,i - p);

		ans -= (ll)(1e8 + 0.01) * num;

		sum += a[i];
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

## [**D - Another Sigma Problem**](https://atcoder.jp/contests/abc353/tasks/abc353_d)

*  $f(x,y)=x*10^{len(y)}+y$
* 求 $\sum_{i=1}^{N-1}\sum_{j=i+1}^{N}f(A_i,A_j)$

##### 思路：

* 我们维护下 $\sum^{N}_{j=i+1} 10^{len(A_j)}\ mod\ 998244353$ 即可

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

void test(){
	int n;cin >> n;
	vector<ll> sufsum(n + 2,0);
	vector<ll> a(n + 1);
	for(int i = 1;i <= n;i++) cin >> a[i];
	ll ans = 0;
	for(int i = n;i >= 1;i--){
		int x = a[i];
		ans += (sufsum[i + 1] + i - 1) * a[i];
		ans %= mod;
		ll _10 = 1;
		while(x){
			x /= 10;
			_10 *= 10;
		}
		sufsum[i] = (sufsum[i + 1] + _10) % mod;
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

## [**E - Yet Another Sigma Problem**](https://atcoder.jp/contests/abc353/tasks/abc353_e)

*  $f(S_i,S_j)$ 为两个字符串最长公共前缀的长度
* 问 $\sum_{i=1}^{N-1}\sum_{j=i+1}^{N}f(S_i,S_j)$

##### 思路：

* 那不就维护 $Trie$ 树即可

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

const int N = 3e5 + 10;

int ptop = 0;

pair<int,int> tire[N][26];

ll ans = 0;

void add(string &s){
	int no = 0;
	for(auto c : s){
		if(tire[no][c - 'a'].first == 0){
			tire[no][c - 'a'].first = ++ptop;
		}
		no = tire[no][c - 'a'].first;
		ans += tire[no][c - 'a'].second;
		tire[no][c - 'a'].second++;
	}
}

void test(){
	int n;cin >> n;
	for(int i = 1;i <= n;i++){
		string s;cin >> s;
		add(s);
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

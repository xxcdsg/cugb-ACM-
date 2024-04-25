## [A. Stickogon](https://codeforces.com/problemset/problem/1957/A)

* 给出 $n$ 个木棒的长度，问这 $n$ 个木棒最多能分为多少正多边形
* 要求：每条边一条棒
* 一条棒只属于一个正多边形
* 棒不能折断

##### 思路：

* 用边最少的正多边形：正三角形

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
	map<int,int> mp;
	for(int i = 1;i <= n;i++){
		int x;cin >> x;
		mp[x]++;
	}
	int ans = 0;
	for(auto [x,y] : mp){
		ans += y / 3;
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

## [B. A BIT of a Construction](https://codeforces.com/problemset/problem/1957/B)

* 给出 $n$ 与 $k$ ，要求构造一个长度为 $n$ 的序列 $a$ ，要求 $\sum_{i=1}^na_i=k$ ，并要求 $a_1|a_2|...|a_n$ 的值二进制为 $1$ 的数量最多。

##### 思路：

* 如果 $n=1$ 当然直接输出 $k$ 即可
* 如果 $n \neq 1$ ，我们假设 $k$ 的二进制为 $10110101$
* 明显我们不可能使得 $a_1|a_2|...|a_n=11111111$ ，也就是我们不可能使得 $1$ 的数量占满所有位置
* 那么我们退而求其次，我们能否 $1$ 的数量少一个
* 那么我们取 $highbit-1$ ，另外一个数取 $k-(highbit-1)$ ，然后其他均取 $0$ ，即能使得数量最多
* 特判 $k=2^x-1$

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
 
int get_high_bit(int x){
	int no = 1;
	while(no * 2 <= x){
		no *= 2;
	}
	return no;
}
 
void test(){
	int n,k;cin >> n >> k;
	if(n == 1){
		cout << k << '\n';
		return;
	}
	int tem = get_high_bit(k);
	if(2*k - 1 == k){
		cout << k << ' ';
		for(int i = 1;i <= n - 1;i++)
			cout << 0 << ' ';
		cout << '\n';
	}else{
		cout << tem - 1 << ' ';
		cout << k - (tem - 1) << ' ';
		for(int i = 3;i <= n;i++)
			cout << 0 << ' ';
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

## [E. Scuza](https://codeforces.com/problemset/problem/1742/E)

* 给一个长度为 $n$ 的序列 $a$ ， $a_i$ 代表比前一个台阶高 $a_i$ ，从前一个台阶到下一个台阶至少需要 $a_i$ 的腿长
* 问当腿长为 $k$ 时，最高能走到多高

##### 思路：

* 要满足 $max(a_1,a_2,...a_x)\leq k$ ，求 $\sum_{i=1}^xa_i$ 的最大值
* 也就是问当腿长最长时，最多能走到第几个阶梯，我们用前缀最大值加二分即可找到最远能走到第几个阶梯了

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
	vector<pair<ll,ll>> ve(n + 1,make_pair(0,0));
	for(int i = 1;i <= n;i++){
		cin >> ve[i].first;
		ve[i].second = ve[i - 1].second + ve[i].first;
		ve[i].first = max(ve[i].first,ve[i - 1].first);
	}
	while(q--){
		int x;cin >> x;
		pair<int,ll> pa = make_pair(x,INF);
		int l = 1,r = n;
		while(l <= r){
			int mid = (l + r) / 2;
			if(ve[mid].first > x)  r = mid - 1;
			else l = mid + 1;
		}
		cout << ve[r].second << ' ';
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

## [C. How Does the Rook Move?](https://codeforces.com/problemset/problem/1957/C)

*  $n*n$ 棋盘，每次选一个位置 $(x,y)$ 放白车，并且同时会在 $(y,x)$ 放黑车(除非 $x=y$ )，要求所有车都不在同一条直线
*  直到不能放车为止，问最后的棋盘状态有多少种

##### 思路：

* 我们假设已经放满了 $(n-1)* (n-1)$ 与 $(n-2)*(n-2)$
* 假设我们再放一个位置 $(n,n)$ ，那么有 $dp[n]+=dp[n-1]$
* 假设我们放一个位置 $(x,n)$ ，那么有 $dp[n]+=(n-1)*dp[n-2]$
* 假设我们放一个位置 $(n,x)$ ，那么有　$dp[n]+=(n-1)*dp[n-2]$
* 因此我们有 $dp$ 转移方程， $dp[n]=dp[n-1]+2(n-1)dp[n-2]$

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
const int mod = 1e9 + 7;
const int N = 3e5 + 10;

ll dp[N];

void get_dp(){
	dp[1] = 1;
	dp[0] = 1;
	for(int i = 2;i <= 3e5;i++){
		dp[i] = (dp[i - 1] + dp[i - 2] * (i - 1) * 2) % mod;
	}
}

void test(){
	int n,k;cin >> n >> k;
	for(int i = 1;i <= k;i++){
		int x,y;cin >> x >> y;
		n--;
		if(x != y)
			n--;
	}
	cout << dp[n] << '\n';
}

int main(){
	IOS
	int t = 1;cin >> t;
	get_dp();
	while(t--){
		test();
	}
}
```

## [F. Smaller](https://codeforces.com/problemset/problem/1742/F)

* 两个初始为 $"a"$ 的字符串，每次给一个字符串添加 $k$ 个字符串 $x$
* 然后任意重新排列，问第一个字符串能否比第二个字符串字典序小

##### 思路：

* 如果第一个字符串的最小字符大于第二个字符串的最大字符，那么肯定可以将这两个字符串都移动到最前面使得第一个字符串小于第二个字符串
* 否则我们就考虑两个字符串都由 $'a'$ 构成，那么比较长度即可
* 只统计数量，不具体考虑字符串组成，否则肯定会超时或者爆空间

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
	int q;cin >> q;
	char mac1 = 'a',mic1 = 'a',mac2 = 'a',mic2 = 'a';
	ll num1 = 1,num2 = 1;
	while(q--){
		int d,k;string s;cin >> d >> k >> s;
		if(d == 1){
			num1 += k * (ll)s.size();
			mac1 = max(mac1,*max_element(s.begin(),s.end()));
			mic1 = min(mic1,*min_element(s.begin(),s.end()));
		}else{
			num2 += k * (ll)s.size();
			mac2 = max(mac2,*max_element(s.begin(),s.end()));
			mic2 = min(mic2,*min_element(s.begin(),s.end()));
		}
		if(mic1 < mac2) cout << "Yes\n";
		else if(mic1 == mac1 && mac1 == mic2 && mic2 == mac2 && num1 < num2) cout << "Yes\n";
		else cout << "No\n";
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

## [D. A BIT of an Inequality](https://codeforces.com/problemset/problem/1957/D)

* 给一个长度为 $n$ 的序列 $a$
* 求三元组 $(x,y,z)$ 满足 $1\leq x \leq y \leq z \leq n$ ， $f(x,y)\ xor\ f(y,z)>f(x,z)$
* 其中 $f(l,r)=a_l\ xor\ a_{l+1}\ xor\ ...xor\ a_r$

##### 思路：

* 也就是说要满足 $f(x,z)\ xor\ a[y]>f(x,z)$
* 那么一个数异或了另外一个数变小了，那么这个数有什么性质呢
* 我们考虑 $a[y]$ 最高位，如果 $f(x,z)$ 在 $a[y]$ 的最高位为 $1$ ，那么肯定会变小，因此对于每个 $y$ 来说，我们要找多少个 $f(x,z)$ 对应 $a[y]$ 的最高位为 $1$
* 我们从异或前缀和 $b$ ，那么有 $f(x,z)=b[z]\ xor\ b[x-1]$ ，要想 $f(x,z)$ 对应 $a[y]$ 的最高位为 $1$ ，那么就有两种情况，即 $b[z]$ 与 $b[x-1]$ 对应 $a[y]$ 的最高位为 $1$ 都是 $1$ ，还有 $b[z]$ 与 $b[x-1]$ 对应 $a[y]$ 的最高位为 $1$ 都是 $0$
* 统计 $32$ 位的数量即可

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

int get_highbit(int x){
	int i = 0;
	for(;i < 32;i++){
		if(x < (1 << i)){
			i--;
			break;
		}
	}
	return i;
}

void test(){
	int n;cin >> n;
	vector<int> a(n + 1);
	for(int i = 1;i <= n;i++) cin >> a[i];
	vector<int> xorsum(n + 1,0);
	for(int i = 1;i <= n;i++){
		xorsum[i] = xorsum[i - 1] ^ a[i];
	}
	vector<vector<ll>> ct(n + 1,vector<ll>(33,0));
	for(int i = 1;i <= n;i++){
		for(int j = 0;j <= 32;j++){
			ct[i][j] = ct[i - 1][j];
			if((xorsum[i] & (1 << j))) ct[i][j]++;
		}
	}
	ll ans = 0;
	for(int i = 1;i <= n;i++){
		int highbit = get_highbit(a[i]);
		ans += ct[i - 1][highbit] * (ct[n][highbit] - ct[i - 1][highbit]) + 
		(i - ct[i - 1][highbit]) * (n - i + 1 - (ct[n][highbit] - ct[i - 1][highbit]));
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

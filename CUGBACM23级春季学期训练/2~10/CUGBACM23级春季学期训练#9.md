## [A-差分约束]()

* 对于形如：

$$
\begin{cases}
x_{c_{1}}-x_{c_{1}'}\leq y_1 \\
x_{c_{2}}-x_{c_{2}'}\leq y_2 \\
...\\
x_{c_{n}}-x_{c_{n}'}\leq y_n \\
\end{cases}
$$

* 对于单个式子而言 $x_{c_{1}}-x_{c_{1}'}\leq y_1$ ，可转换为 $x_{c_{1}}\leq x_{c_{1}'}+y_1$ ，而在图论中的点与点的距离也可以表示成这种样子，也就是点 $c_1'$ 到 $c_1$ 有一条权值为 $y_1$ 的单向边

* 建图，用SPFA判断是否有负环即可判断是否有解，并且最后的dis就是一个可行解

```c++
#include<bits/stdc++.h>
using namespace std;
#define ll long long
#define fo(i,n) for(int i = 1;i <= n;i++)
#define debug(i,x) cout << "case" << (i) << ":" << x << endl;
#define lson(x) (x << 1)
#define rson(x) (x << 1 | 1)
const int inf = 1e9;
const int MAXN = 5e3 + 10;
ll num[MAXN],dis[MAXN];
bool use[MAXN];
vector<pair<int,int>> ed[MAXN];
int n,m;
void init(){
	memset(dis,1e9,sizeof(dis));
}
bool spfa(){
	dis[1] = 0;
	queue<int> qu;
	fo(i,n){
		qu.push(i);
		use[i] = 1;
		num[i]++;
	}
	while(!qu.empty()){
		int u = qu.front();
		use[u] = 0;
		qu.pop();
		for(auto pa:ed[u]){
			int v = pa.first,ddis = pa.second;
			if(ddis + dis[u] < dis[v]){
				dis[v] = ddis + dis[u];
				if(!use[v]){
					num[v]++;
					use[v] = 1;
					qu.push(v);
					if(num[u] >= n)//出现负环
						return 0;
				}
			}
		}
	}
	return 1;
}
int main()
{
	cin >> n >> m;
	init();
	for(int i = 1;i <= m;i++){
		int u,v,dis;cin >> u >> v >> dis;
		ed[v].push_back({u,dis});
	}
	bool f = 1;
	if(!spfa()){
		f = 0;
	}
	if(f)
		fo(i,n)
			cout << dis[i] << ' ';
	else
		cout << "NO" << endl;
}
```

## [B - 合唱队形](https://www.luogu.com.cn/problem/P1091)

* 考察最长单调递增子序列，正反跑两次即可解决
* 我们维护一个单调递增的序列，每次加入一个值尝试能否直接接在序列后面，增长序列长度
* 如果不能，那么我们就要使得这个序列更好被更新，那么我们就将从底部到顶部第一个大于等于它这个值的数替换掉，那么下一次更新值对应的序列它就会比较小，那么就更好更新
* 它本质是将所有以某个数为结尾的最长单调递增子序列集合为同一个单调子序列
* 我们想，如果同样长度的单调子序列，如果我们要保留一个，那么我们肯定是保留结尾较小的，因为这样才能容易被更新
* 那么我们每次找第一个比这个数大的数来替换这个操作正是将同样长度的单调递增子序列替换为结尾更小的序列

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
	vector<int> t(n);
	for(int &x : t) cin >> x;
	vector<int> pre(n),suf(n);
	vector<int> st;
	for(int i = 0;i < n;i++){
		if(st.empty() || *st.rbegin() < t[i]){
			st.push_back(t[i]);
			pre[i] = st.size();
		}else{
			auto it = lower_bound(st.begin(),st.end(),t[i]);
			*it = t[i];
			pre[i] = it - st.begin() + 1;
		}
	}
	st.clear();
	for(int i = n - 1;i >= 0;i--){
		if(st.empty() || *st.rbegin() < t[i]){
			st.push_back(t[i]);
			suf[i] = st.size();
		}else{
			auto it = lower_bound(st.begin(),st.end(),t[i]);
			*it = t[i];
			suf[i] = it - st.begin() + 1;
		}
	}

	int ans = 0;
	for(int i = 0;i < n;i++){
		ans = max(ans,pre[i] + suf[i] - 1);
	}
	cout << n - ans << '\n';
}

int main(){
	IOS
	int t = 1;//cin >> t;
	while(t--){
		test();
	}
}
```

## [**A - Adjacent Product**](https://atcoder.jp/contests/abc346/tasks/abc346_a?lang=en)

* 输出相邻乘积

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
	int pre;cin >> pre;
	for(int i = 1;i <= n - 1;i++){
		int x;cin >> x;
		cout << x * pre << ' ';
		pre = x;
	}
	cout << '\n';
}

int main(){
	IOS
	int t = 1;//cin >> t;
	while(t--){
		test();
	}
}
```

## [**B - Piano**](https://atcoder.jp/contests/abc346/tasks/abc346_b)

* 无限循环的字符串 $wbwbwwbwbwbw$

* 问能否有一段能够有 $W$ 个 $w$ 与 $B$ 个 $b$

##### 思路：

* 因为已知 $w$ 和 $b$ 的数量，那么长度就已知，那么我们只要枚举起点即可，因为是循环的，因此相同字符串的相同位置为起点，那么得出的数量都是相同的，因此我们枚举起点即可

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
	int w,b;cin >> w >> b;
	string s = "wbwbwwbwbwbw";
	int n = w + b;
	for(int i = 0;i < n;i++){
		if(s[i % 12] == 'w') w--;
		else b--;
	}
	bool f = 0;
	if(w == 0 && b == 0) f = 1;
	for(int i = 0;i <= 11;i++){
		if(s[i % 12] == 'w') w++;
		else b++;
		if(s[(i + n) % 12] == 'w') w--;
		else b--;
		if(w == 0 && b == 0) f = 1;
	}
	if(f) cout << "Yes\n";
	else cout << "No\n";
}

int main(){
	IOS
	int t = 1;//cin >> t;
	while(t--){
		test();
	}
}
```

## [**C - Σ**](https://atcoder.jp/contests/abc346/tasks/abc346_c)

* 统计 $[1,K]$ 中未出现在 $A$ 中的和

##### 思路：

* 用 $set$ 可以很容易统计出 $[1,K]$ 中出现在 $A$ 中的和，再用求和公式一减即可

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
	int n;
	ll k;
	cin >> n >> k;
	set<int> se;
	ll sum = k*(k + 1) / 2;
	for(int i = 1;i <= n;i++){
		int x;cin >> x;
		if(x <= k && se.find(x) == se.end()){
			sum -= x;
			se.insert(x);
		}
	}
	cout << sum << '\n';
}

int main(){
	IOS
	int t = 1;//cin >> t;
	while(t--){
		test();
	}
}
```

## [**D - Gomamayo Sequence**](https://atcoder.jp/contests/abc346/tasks/abc346_d)

* 将原本的 $01$ 字符串变成有仅有一处两个相邻字符相同的字符串需要变的最少字符个数

##### 思路：

* 我们假设位置是 $i$ ，那么有 $s[1...i]=010101010...$ 或者 $s[1...i]=10101010$ 两种情况且仅有着两种情况，我们假设 $s[1...i]=...01010101...0$ 的最小代价为 $pre0[i]$ ，$s[1...i]=...01010101...1$ 的最小代价为 $pre1[i]$ 同时，那么有 $pre1[i]=pre0[i-1]+(s[i]=='1'?0:1)$ 与 $pre0[i]=pre1[i-1]+(s[i]=='0'?0:1)$ 这两个转移方程，同理可求 $suf0$ 与 $suf1$ ，最后求 $pre0[i]+suf0[i+1]$ 与 $pre1[i]+suf1[i+1]$ 的最小值即可

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
	vector<ll> pre0(n,0),pre1(n,0),suf0(n,0),suf1(n,0),c(n);
	for(int i = 0;i < n;i++) cin >> c[i];
	if(s[0] == '1') pre0[0] = c[0];
	else pre1[0] = c[0];
	if(s[n - 1] == '1') suf0[n - 1] = c[n - 1];
	else suf1[n - 1] = c[n - 1];
	for(int i = 1;i < n;i++){
		pre1[i] = pre0[i - 1];
		pre0[i] = pre1[i - 1];
		if(s[i] == '1') pre0[i] += c[i];
		else pre1[i] += c[i];
	}
	for(int i = n - 2;i >= 0;i--){
		suf1[i] = suf0[i + 1];
		suf0[i] = suf1[i + 1];
		if(s[i] == '1') suf0[i] += c[i];
		else suf1[i] += c[i];
	}
	ll ans = INF;
	for(int i = 0;i < n - 1;i++){
		ans = min(ans,suf0[i + 1] + pre0[i]);
		ans = min(ans,suf1[i + 1] + pre1[i]);
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

## [**E - Paint**](https://atcoder.jp/contests/abc346/tasks/abc346_e?lang=en)

* $H*W$ 的画布，一开始的颜色均为 $0$ ，每次给某一行或者某一列涂某种颜色，问最后所有颜色各自的数量

##### 思路：

* 贪心覆盖的思想，我们想到每次我们涂颜色，如果后面再涂色，那么这里填的颜色也可能被覆盖，因此我们应该想到从后往前思考
* 如果我们最后给某一行涂了颜色 $c$ ，那么颜色 $c$ 的数量至少就是 $W$ ，因为一行有 $W$ 个区域
* 假设我们在给某一行涂了颜色 $c$ 之后，又给 $k$ 个不同的列涂色，那么这一行就会有 $k$ 个区域被涂去，因此贡献变为 $W-k$
* 因此我们从后往前考虑时，还要考虑不同类型的涂色次数，减去不同的次数即可

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
	int h,w,m;cin >> h >> w >> m;
	ll sum = (ll)h * w;
	map<int,ll> mp;
	vector<bool> useh(h + 1,0),usew(w + 1,0);
	vector<tuple<int,int,int>> oper(m);
	for(auto &[x,y,z] : oper){
		cin >> x >> y >> z;
	}
	for(int i = m - 1;i >= 0;i--){
		auto [op,x,c] = oper[i];
		if(op == 1){
			if(!useh[x]){
				useh[x] = 1;
				h--;
				mp[c] += w;
			}
		}else if(op == 2){
			if(!usew[x]){
				usew[x] = 1;
				w--;
				mp[c] += h;
			}
		}
	}
	queue<pair<int,ll>> qu;
	for(auto [c,num] : mp){
		if(num != 0){
			if(c != 0)
				qu.push({c,num});
			sum -= num;
		}
	}
	mp[0] += sum;
	if(mp[0] == 0){
		cout << (int)qu.size() << '\n';
	}else{
		cout << (int)qu.size() + 1 << '\n';
		cout << 0 << ' ' << mp[0] << '\n';
	}
	while(!qu.empty()){
		auto [c,num] = qu.front();
		cout << c << ' ' << num << '\n';
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


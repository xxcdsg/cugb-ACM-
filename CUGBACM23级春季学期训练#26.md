## [A. Everyone Loves to Sleep](https://codeforces.com/contest/1714/problem/A)

* 给出一个时间，然后给出另外的 $n$ 个时间，问离最近的下一个时间有多少时间
* 先转化为分钟，如果时间在给定时间之前，那么 $+24*60$

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
	int n,h,m;cin >> n >> h >> m;
 
	m += h * 60;
 
	int ans = inf;
 
	for(int i = 1;i <= n;i++){
		int hh,mm;cin >> hh >> mm;
		mm += hh * 60;
		if(mm < m) mm += 24*60;
		ans = min(ans,mm - m);
	}
	cout << ans / 60 << ' ' << ans % 60 << '\n';
}
 
int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [B. Remove Prefix](https://codeforces.com/contest/1714/problem/B)

* 删除最少的前缀，使得剩下的数都不重复
* 从后向前遍历找重复即可

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
	set<int> se;
	int ans = 0;
	for(int i = n;i >= 0;i--){
		ans = i;
		if(i == 0) break;
		if(se.find(a[i]) == se.end()){
			se.insert(a[i]);
		}else
			break;
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

## [C. Minimum Varied Number](https://codeforces.com/contest/1714/problem/C)

* 找出最小的数，这个数每一位都不同，并且每一位的和等于 $s$

##### 思路：

* 我们要让位高的数尽量小，那么低位的就是尽量大，从九开始枚举

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
	int s;cin >> s;
	int no = 9;
	string ss;
	while(s){
		int tem = min(no,s);
		s -= min(no,s);
		ss = (char)(tem + '0') + ss;
		no--;
	}
	cout << ss << '\n';
}
 
int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [D. Color with Occurrences](https://codeforces.com/contest/1714/problem/D)

* 给出一个字符串 $t$ 与一组字符串 $s_1,s_2..s_n$
* 你的目标是让字符串 $t$ 的所有字符均被染色，并且操作次数尽量小
* 每次操作可以找出 $t$ 中的一个连续子序列，并且该连续子序列与每个字符串 $s$ 相同，然后给这个序列染色

##### 思路：

* 我们贪心的想，我们每次都找最远能染色到的位置，然后取这个最远能染色到的位置
* 保证上一次取到的是最远染色的位置，那么也就保证了本次可作为起点的位置最多，因此每次都贪心取最远即可
* 我们记录 $dp[i]$ 为如果以这个位置为起点染色，最远能染色到的位置与所用的字符串
* 然后贪心，每次取最大的 $dp$ 即可，如果贪心了一轮，但是最远位置没有更新，那么说明不可能全部染色

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
 
int dp[101];
pair<int,int> tem[101];
 
void test(){
	string t;cin >> t;
	int len = t.size();
 
	for(int i = 0;i < len;i++) dp[i] = 0;
 
	int n;cin >> n;
	for(int i = 1;i <= n;i++){
		string s;cin >> s;
		int len2 = s.size();
		for(int j = 0;j <= len - len2;j++){
			bool f = 1;
			for(int k = 0;k < len2;k++){
				if(t[j + k] != s[k]) f = 0;
			}
			if(f){
				if(dp[j] < j + len2){
					dp[j] = j + len2;
					tem[j] = {i,j + 1};
				}
			}
		}
	}
 
	// for(int i = 0;i < len;i++) cout << dp[i] << ' ';
	// cout << '\n';
 
	int ans = 0,to = 0;
 
	queue<pair<int,int>> qu;
 
	while(to < len){
		int tto = to;
		int id = -1;
		for(int i = 0;i <= to;i++){
			if(dp[i] > tto){
				tto = dp[i];
				id = i;
			}
		}
		if(tto == to){
			ans = -1;
			break;
		}else
			qu.push(tem[id]);
		to = tto;
		ans++;
	}
	cout << ans << '\n';
	if(ans != -1){
		while(!qu.empty()){
			auto [x,y] = qu.front();
			qu.pop();
			cout << x << ' ' << y << '\n';
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

## [E. Add Modulo 10](https://codeforces.com/contest/1714/problem/E)

* 给出 $n$ 个数，每次可以选一个数 $a[i]$ ，使得 $a[i]:=a[i]+a[i]\ mod\ 10$
* 问能否让所有数都相等

##### 思路：

* 我们看个位数的循环 $5\to0\to0$， $2\to4\to8\to^{10}6\to^{10}2$， $3\to6\to...$ ， $7\to^{10}4\to..$ ， $9\to^{10}8\to..$
* 我们发现，有两个循环，一个是 $5,0$ 的循环，一个是 $2,4,8,6$ 的循环
* 因此如果出现了个位不在同一循环之中的情况，那么肯定不能让所有数相同
* 然后对于 $5,0$ ,我们让所有个位为 $5$ 的执行因此操作，然后看是否全部相同即可
* $2,4,8,6$ 循环来说，首先将 $3,7,9$ 执行因此操作，进入循环之中
* 然后对于 $2\to4\to8\to^{10}6\to^{10}2$ 循环来说，我们看到想让每个数的个位不变，至少要 $+20$ ，也就是说，十位数之后的数奇偶性相同
* 同时 $6$ 夹在两个 $+10$ 之间，因此，它的十位数后的奇偶性与其他的数不同

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
	set<int> se;
	for(int i = 1;i <= n;i++){
		cin >> a[i];
		int tem = a[i] % 10;
		if(tem == 3 || tem == 7 || tem == 9 || tem == 5){
			a[i] += a[i] % 10;
		}
		se.insert(a[i]);
	}
	if(se.size() != 1){
		int x = (a[1] / 10) % 2;
		if(a[1] % 10 == 6) x = 1 - x;
		bool f = 1;
		for(int i = 1;i <= n;i++){
			int y = (a[i] / 10) % 2;
			if(a[i] % 10 == 6) y = 1 - y;
			if(x != y) f = 0;
			if(a[i] % 10 == 0) f = 0;
		}
		if(f) cout << "Yes\n";
		else cout << "No\n";
	}else{
		cout << "Yes\n";
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

## [F. Build a Tree and That Is It](https://codeforces.com/contest/1714/problem/F)

* 给出一个树的总端点数 $n$ ，点 $1,2/2,3/1,3$ 的距离 $d_{12},d_{23},d_{31}$ ，问该树是否存在，如果存在就输出一个

##### 思路：

* 分成两种情况，第一种就是链的情况，那么有 $d_{12}=d_{23}+d_{31}或者d_{23}=d_{31}+d_{12}或者d_{31}=d_{12}+d_{23}$
* 另外一种情况是三个点不在同一条链上，假设它们两两对应的链的交点是点 $4$ ，那么有 $d_{14}=(d_{31}+d_{12}-d_{23})/2,d_{24}=(d_{23}+d_{12}-d_{31})/2,d_{34}=(d_{31}+d_{23}-d_{12})/2$
* 然后模拟即可，多写几个函数来输出链与连接

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

int no;

void pt(int x,int num){
	for(int i = 1;i <= num;i++){
		cout << x << ' ' << no << '\n';
		x = no;
		no++;
	}
}

void Pt(int x,int y,int num){
	if(num == 0) return;
	if(num == 1)
		cout << x << ' ' << y << '\n';
	else{
		pt(x,num - 1);
		cout << y << ' ' << no - 1 << '\n';
	}
}

void test(){
	int n,d12,d23,d31;
	no = 4;
	cin >> n >> d12 >> d23 >> d31;
	if(d12 == d23 + d31){
		if(n <= d12){
			cout << "No\n";
			return;
		}
		cout << "Yes\n";
		Pt(2,3,d23);
		Pt(3,1,d31);
		Pt(1,n,n - no + 1);
	}else if(d23 == d31 + d12){
		if(n <= d23){
			cout << "No\n";
			return;
		}
		cout << "Yes\n";
		Pt(3,1,d31);
		Pt(1,2,d12);
		Pt(1,n,n - no + 1);
	}else if(d31 == d23 + d12){
		if(n <= d31){
			cout << "No\n";
			return;
		}
		cout << "Yes\n";
		Pt(2,3,d23);
		Pt(2,1,d12);
		Pt(1,n,n - no + 1);
	}else{
		int d14 = (d12 + d31 - d23),d24 = (d23 + d12 - d31),d34 = (d31 + d23 - d12);
		if(d14 % 2 || d24 % 2 || d34 % 2 || d14 <= 0 || d24 <= 0 || d34 <= 0){
			cout << "No\n";
		}else{
			d14 /= 2;
			d24 /= 2;
			d34 /= 2;
			no++;
			int cos = d14 + d24 + d34 - 3;
			if(cos + 4 > n){
				cout << "No\n";
			}else{
				cout << "Yes\n";
				Pt(1,4,d14);
				Pt(2,4,d24);
				Pt(3,4,d34);
				Pt(1,n,n - no + 1);
			}
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

## [G. Path Prefixes](https://codeforces.com/contest/1714/problem/G)

* 给出一个根为 $1$ 的树，每条边有两个值 $a_i,b_i$ ，我们令每个点 $i$ 到点 $1$ 的 $a_i$ 的和为 $A_i$
* 点 $1$ 到每个点 $i$ 的路径，求其最大前缀，使得其 $b_i$ 的和小于 $A_i$

##### 思路：

* 首先， $A_i$ 很容易直接搜索得出
* 然后对于每个点的路径，我们如果能维护出每个点的 $b$ 的前缀和，那么用二分即可很容易得出最大前缀
* 而维护这个 $b$ 的前缀和也可以在搜索中完成
* 因此只要搜索，并且搜索到每个点维护 $b$ 的前缀和，然后二分即可

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
	ll suma = 0;
	int len = 0;
	vector<ll> b(n + 1,0);
	vector<int> ans(n + 1);
	vector<vector<tuple<int,int,int>>> ed(n + 1);
	for(int i = 2;i <= n;i++){
		tuple<int,int,int> tu;
		auto &[son,a,_b] = tu;
		int fa;
		son = i;
		cin >> fa >> a >> _b;
		ed[fa].push_back(tu);
	}
	function<void(int)> dfs = [&](int x){
 
		int l = 1,r = len;
		while(l <= r){
			int mid = (l + r) / 2;
			if(suma >= b[mid])
				l = mid + 1;
			else
				r = mid - 1;
		}
		ans[x] = r;
 
		for(auto [son,a,_b] : ed[x]){
			len++;
			suma += a;
			b[len] = _b + b[len - 1];
			dfs(son);
			len--;
			suma -= a;
		}
	};
 
	dfs(1);
 
	for(int i = 2;i <= n;i++)
		cout << ans[i] << ' ';
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


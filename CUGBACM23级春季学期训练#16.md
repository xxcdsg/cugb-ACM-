## [**C - Airport Code**](https://atcoder.jp/contests/abc349/tasks/abc349_c)

* 长度为 $3$ 的由大写英文字母组成的字符串 $T$ 是由小写英文字母组成的字符串 $S$ 的**机场代码**，当且仅当 $T$ 可以通过以下方法之一从 $S$ 推导出来：
* 从 $S$ 中取出长度为 $3$ 的子序列（不一定连续），变为大写字母为 $T$
* 或者从 $S$ 中取出长度为 $2$ 的子序列（不一定连续），变为大写字母再在最后加入一个 $'X'$ 变为 $T$
* 问 $S$ 能否变为 $T$

##### 思路：

* 贪心，从前往后依次匹配字母，因为只有前面的字母匹配了才能匹配后面的字母，因此每次找字母都要找最前面的可选字母
* 如果最后一个字母为 $'X'$ ，那么只要匹配两个即可

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
	string s,t;cin >> s >> t;
	for(auto &c : s) c = c + 'A' - 'a';
	// cout << s << '\n';
	if(t[2] == 'X'){
		bool f = 0,ff = 0;
		for(auto c : s){
			if(f && c == t[1]) ff = 1;
			if(c == t[0]) f = 1;	
		}
		if(ff){
			cout << "Yes\n";
			return;
		}
	}else{
		bool f[3] = {0};
		for(auto c : s){
			if(f[0] && f[1] && t[2] == c) f[2] = 1;
			if(f[0] && t[1] == c) f[1] = 1;
			if(c == t[0]) f[0] = 1;
		}
		if(f[0] && f[1] && f[2]){
			cout << "Yes\n";
			return;
		}
	}
	cout << "No\n";
}

int main(){
	IOS
	int t = 1;//cin >> t;
	while(t--){
		test();
	}
}
```

## [**D - Divide Interval**](https://atcoder.jp/contests/abc349/tasks/abc349_d)

*  $S(l,r)$ 代表 $(l,l+1,...r-1)$ 序列
* 我们称存在非负整数 $i,j$ 使得 $l=2^ij,r=2^i(j+1)$ ，则称 $S(l,r)$ 为好序列
* 我们要将 $L,R$ 分为最少的 $M$ 份好序列
* 即 $L= l_1 < r_1 = l_2 < r_2 =...= l_M < r_M =R$
*  $S(l_1,r_1)...S(l_M,r_M)$ 均为好序列

##### 思路：

* 每次 $i$ 的可取值均为 $[0,log_2(lowbit(l))]$ ，每次取最大的二进制即可

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

ll lowbit(ll x){
	if(x == 0) return (ll)1 << 62;
	return x&-x;
}
ll highbit(ll x){
	ll no = 1;
	x >>= 1;
	while(x){
		x >>= 1;
		no *= 2;
	}
	return no;
}

void test(){
	ll l,r;cin >> l >> r;
	queue<pair<ll,ll>> qu;
	while(l < r){
		ll x = min(lowbit(l),highbit(r - l));
		qu.push({l,l+x});
		l += x;
	}
	cout << (ll)(qu.size()) << '\n';
	while(!qu.empty()){
		auto [x,y] = qu.front();
		cout << x << ' ' << y << '\n';
		qu.pop();
	}
}

signed main(){
	IOS
	int t = 1;//cin >> t;
	while(t--){
		test();
	}
}
```

## [**E - Weighted Tic-Tac-Toe**](https://atcoder.jp/contests/abc349/tasks/abc349_e)

* 井字棋加最后如果平局则根据选中数字和大小比较判断

##### 思路：

* 暴力搜索复杂度为 $9!$ 和每次判断是否三连 $9$ ， $9!*9=3,265,920$

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

ll a[4][4];
int sel[4][4];

ll sum,sumo;

bool check(int no){
	for(int i = 1;i <= 3;i++){
		bool f = 1;
		for(int j = 1;j <= 3;j++)
			if(sel[i][j] != no) f = 0;
		if(f) return 1;
	}
	for(int j = 1;j <= 3;j++){
		bool f = 1;
		for(int i = 1;i <= 3;i++)
			if(sel[i][j] != no) f = 0;
		if(f) return 1;
	}
	bool f = 1;
	for(int i = 1;i <= 3;i++){
		if(sel[i][i] != no) f = 0;
	}
	if(f) return 1;
	f = 1;
	for(int i = 1;i <= 3;i++){
		if(sel[i][4 - i] != no) f = 0;
	}
	if(f) return 1;
	return 0;
}

bool find(int num,int no){
	bool f = 0;
	if(num == 10) return sum > sumo;
	for(int i = 1;i <= 3;i++)
		for(int j = 1;j <= 3;j++){
			if(sel[i][j] == 0){
				sel[i][j] = no;
				sum += a[i][j];
				swap(sum,sumo);
				if(check(no)){
					f = 1;
				}else
					f |= !find(num+1,-no);
				swap(sum,sumo);
				sum -= a[i][j];
				sel[i][j] = 0;
			}
		}
	return f;
}

void test(){
	for(int i = 1;i <= 3;i++)
		for(int j = 1;j <= 3;j++)
			cin >> a[i][j];
	if(find(1,1)){
		cout << "Takahashi\n";
	}else
		cout << "Aoki\n";
}

int main(){
	IOS
	int t = 1;//cin >> t;
	while(t--){
		test();
	}
}
```

## [**F - Subsequence LCM**](https://atcoder.jp/contests/abc349/tasks/abc349_f)

* 有点难，还没做

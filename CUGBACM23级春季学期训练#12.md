## [A. Farmer John's Challenge](https://codeforces.com/problemset/problem/1942/A)

* 给出 $n$ 与 $k$ ，要求构造一个长度为 $n$ 的序列，使得它循环的所有循环移动序列中恰好有 $k$ 个是非递减的
* 循环移动的意思为 $a[1],a[2],...a[n] \to a[n],a[1],...a[n-1] \to ... \to a[2],a[3],...a[n],a[1]$

##### 思路：

*  $k$ 不等于 $0$ ，因此我们构造的序列肯定可以移动到非递减的状态，假设 $a[n]>a[1]$
* 那么它的所有循环移动，除了本身，都有 $a[n]>a[1]$ 但 $a[n]$ 在 $a[1]$ 之前，因此此时 $k=1$
* 当 $a[n]=a[1]$ 时，那么所有循环移动都一样，因此 $k=n$
* 其他情况均不可能，因此输出 $-1$

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
	int n,k;cin >> n >> k;
	if(k == n){
		for(int i = 1;i <= n;i++)
			cout << 1 << ' ';
	}else if(k == 1){
		for(int i = 1;i <= n;i++)
			cout << i << ' ';
	}else{
		cout << -1;
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

## [**B - Substring**](https://atcoder.jp/contests/abc347/tasks/abc347_b?lang=en)

* 给一个字符串，问这个字符串有多少不同的连续子串
* 数量范围较小，暴力 $set<string>$ 统计

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
	set<string> se;
	int len = s.size();
	for(int i = 0;i < len;i++){
		for(int j = 1;j <= len - i + 1;j++){
			se.insert(s.substr(i,j));
		}
	}
	cout << (int)se.size() << '\n';
}

int main(){
	IOS
	int t = 1;//cin >> t;
	while(t--){
		test();
	}
}
```

## [B. Bessie and MEX](https://codeforces.com/problemset/problem/1942/B)

* 给出序列 $a$ ，其中 $a[i]=MEX(p[1],p[2],...p[i])-p[i]$
* 保证序列 $p$ 肯定存在，输出一个序列可能即可

##### 思路：

* 实际上，如果序列 $p$ 存在，那么它肯定是唯一的
* 我们假设 $MEX(p[1],p[2],...p[i])=no_i$ ，那么 $p[i]=no_i-a[i]$
* 同时，如果 $a[i] >= 1$ ，说明 $MEX(p[1],p[2],...p[i])>p[i]$ ，这就说明 $p[1],p[2],...p[i]$ 至少都有 $1,2,...p[i]$，因此上一轮的 $no_{i-1}=MEX(p[1],p[2],...p[i-1])=p[i]$
* 我们再用一个 $vector<bool>$ 来记录所有数是否出现，用循环来更新 $no_i$ 即可

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
	vector<bool> p(n + 1,0);
	int no = 0;
	queue<int> qu;
	for(int i = 1;i <= n;i++){
		int x;cin >> x;
		if(x >= 1){
			p[no] = 1;
			cout << no << ' ';
			while(p[no])
				no++;
		}else{
			cout << no - x << ' ';
			p[no - x] = 1;
		}
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

## [**C - Ideal Holidays**](https://atcoder.jp/contests/abc347/tasks/abc347_c?lang=en)

* 新的周由 连续的 $A$ 天假期与连续的 $B$ 天工作日组成
* 有 $N$ 个计划，第 $i$ 个计划在第 $D_i$ 天后执行
* 不知道今天是星期几，问是否有可能将所有计划再假期执行

##### 思路：

* 假设我们直到今天是星期几，假设为 $k$ ，那么为了满足条件，那么必须有 $(D_i+k-1)\ mod\ (A+B)+1 \leq A,1 \leq i \leq N$
* 那么我们先求出所有的 $d_i=D_i\ mod\ (A+B)$，然后排序
* 假设 $d_n-d_1+1 \leq A$ ，那么就说明以 $d_1$ 为星期一是能将所有计划放在假期的
* 但是星期一不一定是 $d_1$ ，因此枚举，同时对于 $d_i$ 来说，离他最远的是 $d_{i-1}$ ，因此取 $min(\{d_{i-1}+(A+B)-d_i+1\},d_n-d_1+1) \leq A$

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
	ll n,a,b;cin >> n >> a >> b;
	vector<ll> d(n + 1);
	for(int i = 1;i <= n;i++) cin >> d[i];
	for(int i = 1;i <= n;i++){
		d[i] %= (a + b);
	}
	sort(d.begin(),d.end());
	ll mi = (d[n] - d[1] + 1);
	for(int i = 2;i <= n;i++){
		mi = min(a + b + d[i - 1] - d[i] + 1,mi);
	}
	if(mi <= a)
		cout << "Yes\n";
	else
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

## [**D - Popcount and XOR**](https://atcoder.jp/contests/abc347/tasks/abc347_d?lang=en)

* 找出一组 $X$ 与 $Y$ ，使得它们的异或和为 $C$ 
* $popcount(X)=a,popcount(Y)=b$，其中 $popcount$ 代表二进制为 $1$ 的位数
* 并且 $0\leq X < 2^{60},0 \leq Y < 2^{60}$ 

##### 思路：

* 我们先得出 $c=popcount(C)$
* 我们假设 $X$ 与 $Y$ 重复的二进制位数量位 $same$ ，那么 $2*same=a+b-c$
* 因此，这就要求 $a+b \geq c$ 且 $(a+b-c)\ mod\ 2=0$
* 然后对于相同的位置，对应的 $C$ 位置的二进制为 $0$ ，因此还需要 $C$ 在 $2^{60}$ 之内（不包括 $2^{60}$ ）存在 $same$ 个二进制为 $0$ 的位置，也就是 $60-c \geq same$
* 然后对应着给两个数放二进制位即可

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;

#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;
#define int long long

const int inf = 1e9 + 10;
const ll INF = 1e18 + 10;

int countbit(ll x){
	int res = 0;
	while(x){
		if(x&1) res++;
		x >>= 1;
	}
	return res;
}

void test(){
	int a,b;ll c;
	cin >> a >> b >> c;
	int num = countbit(c);
	if(a + b >= num && ((a + b) % 2 == num % 2) && abs(a - b) <= num){
		int same = (a + b - num) / 2;
		a -= same;
		b -= same;
		ll x = 0,y = 0;
		ll tem = 1;
		while(c){
			if(c & tem){
				c -= tem;
				if(a){
					x += tem;
					a--;
				}else{
					y += tem;
				}
			}else if(same){
				same--;
				x += tem;
				y += tem;
			}
			tem *= 2;
		}
		while(same){
			if(tem == ((ll)1 << 60)){
				cout << -1 << '\n';
				return;
			}
			x += tem;
			y += tem;
			tem *= 2;
			same--;
		}
		cout << x << ' ' << y << '\n';
	}else{
		cout << -1 << '\n';
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

## [C1. Bessie's Birthday Cake (Easy Version)](https://codeforces.com/problemset/problem/1942/C1)

* 正 $n$ 边形，已经选出 $x$ 个点，它们这些点可以与同样是选出的点不相交的相连，问这个正 $n$ 边形最多被切出多少个三角形

##### 思路：

* 首先，无论这个 $n$ 边形是正 $n$ 边形，只要它是凸多边形，那么它的形状就无关紧要
* 那么我们先将那 $k$ 个点挑出，考虑点都选上的 $k$ 边形能产生多少三角形
* 答案是 $k-2$ ，为什么？我也不知道，你可以看看 https://codeforces.com/blog/entry/126942 英文题解，三角剖分归纳法什么的
* 然后对于外面的形状，如果选中的点中间正好夹着一个没被选中的点，那么连上这两个点，正好也能形成一个三角形

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
	int n,x,y;cin >> n >> x >> y;
	vector<int> p(x + 1);
	for(int i = 1;i <= x;i++)
		cin >> p[i];
	sort(p.begin(),p.end());
	int ans = 0;
	for(int i = 1;i < x;i++){
		if(p[i + 1] - p[i] == 2){
			ans++;
		}
	}
	if(p[x] + 2 == p[1] + n) ans++;
 
	//对x分解
 
	ans += max(x - 2,0);
 
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


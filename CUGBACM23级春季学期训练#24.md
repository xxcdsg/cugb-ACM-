## [A. My First Sorting Problem](https://codeforces.com/contest/1971/problem/A)

* 输出

## [B. Different String](https://codeforces.com/contest/1971/problem/B)

* 判断字符串是否由同一个字符组成

## [C. Clock and Strings](https://codeforces.com/contest/1971/problem/C)

* 判断两对数在时钟上是否交叉

##### 思路：

* 从一个数开始走，走到对应的数的位置，那么中间必须有且只有一个非对应的数

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
	int a,b,c,d;cin >> a >> b >> c >> d;
	if(a > b) swap(a,b);
	int num = 0;
	while(a < b){
		if(a == c) num++;
		if(a == d) num++;
		a++;
		if(a == 13) a = 1;
	}
	if(num == 1) cout << "Yes\n";
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

## [D. Binary Cut](https://codeforces.com/contest/1971/problem/D)

* 将二进制数分块，问最少分多少块可以让所有 $0$ 都在 $1$ 前面

##### 思路：

* 我们先根据连续的 $0$ 与 $1$ 分块
* 然后如果存在 $01$ 这样的形式，那么这个块可以不分，让 $0$ 放在前面， $1$ 放在它后面

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
	int num = 1;
	int len = s.size();
	bool f = 0;
	for(int i = 1;i < len;i++){
		if(s[i] != s[i - 1]) num++;
		if(s[i] == '1' && s[i - 1] == '0') f = 1;
	}
	if(f) num--;
	cout << num << '\n';
}
 
int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [E. Find the Car](https://codeforces.com/contest/1971/problem/E)

* 在 $0\to n$ 上有 $k$ 个标志 $a_i$ ，在标志之间是匀速
* 到达每个标志的时间是 $b_i$
* 每次给出一个位置 $d$ ，问到达该位置需要的时间，向下取整

##### 思路：

* 先用二分找出是哪一段区间，然后再得出平均速度求
* 精度要求高，最后再验证一下

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
const double ep = 1e-8;
 
void test(){
	int k,n,q;cin >> n >> k >> q;
	vector<int> a(k + 1,0);
	vector<double> b(k + 1,0);
	vector<double> t(k + 1,0);
	for(int i = 1;i <= k;i++) cin >> a[i];
	for(int i = 1;i <= k;i++){
		cin >> b[i];
		t[i] = b[i];
		b[i] -= t[i - 1];
	}
	while(q--){
		int d;cin >> d;
		int p = lower_bound(a.begin(),a.end(),d) - a.begin();
		if(p == 0) p++;
 
		ll tem = b[p] * (d - a[p - 1] * 1.0) / (a[p] - a[p - 1]) + ep;
 
		if(tem * (a[p] - a[p - 1]) > b[p] * (d - a[p - 1] * 1.0) + ep)
			tem--;
 
		cout << (int)(t[p - 1] + tem) << ' ';
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

## [F. Circle Perimeter](https://codeforces.com/contest/1971/problem/F)

> 给定一个整数 $r$ ，找到与 $(0, 0)$ **大于或等于** $r$ 但**严格小于** $r+1$ 的欧氏距离的格点数量。
>
> 格点是具有整数坐标的点。从 $(0, 0)$ 到点 $(x,y)$ 的欧几里得距离是 $\sqrt{x^2 + y^2}$ 。

##### 思路：

* 写一个函数来求距离小于 $x$ 的格点的数量
* 因为是个圆，因此我们求 $\frac{1}{4}$ 即可
* 我们按着圆的边缘运动，画出圆的轮廓，那么轮廓内部的点就是距离小于 $x$ 的格点

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
 
ll f(ll x){
	ll i = x,j = 1;
	ll num = x - 1;
	while(i > 1){
		i--;
		while(j*j + i*i < x*x){
			j++;
			num += i;
		}
	}
	return num * 4 + 1;
}
 
void test(){
	int r;cin >> r;
	cout <<	f(r + 1) - f(r) << '\n';
}
 
int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [G. XOUR](https://codeforces.com/contest/1971/problem/G)

>如果两个数异或小于 $4$ ，那么这两个数就可以交换
>
>给出一个数组，求按上述条件任意次数交换，数组的最小字典序是什么

##### 思路：

* 实际上，我们可以发现，如果两个数异或小于 $4$ ，第三个数与其中一个数异或小于 $4$ ，那么我们可以推出另外一个数也与第三个数异或值小于 $4$
* 因此我们可以将可交换的数分成多组，组内排序即可
* 实际上对于 $x=2^k$ ，两个数异或小于 $x$ 都有这个性质

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
	vector<int> b(n + 1);
	map<int,int> mp;
	vector<vector<int>> ve;
	int ptop = -1;
	for(int i = 1;i <= n;i++){
		cin >> a[i];
		b[i] = a[i] / 4;
		if(!mp.count(b[i])){
			ptop++;
			mp[b[i]] = ptop;
			vector<int> tem;
			tem.push_back(a[i]);
			ve.push_back(tem);
		}else{
			ve[mp[b[i]]].push_back(a[i]);
		}
	}
	for(auto &vve : ve){
		sort(vve.begin(),vve.end());
	}
 
	map<int,int> mmp;
 
	for(int i = 1;i <= n;i++){
		cout << ve[mp[b[i]]][mmp[b[i]]] << ' ';
		mmp[b[i]]++;
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

## [H. ±1]()

>给一个 $3*n$ 矩阵，内部的数由 $a_i与-a_i$ 组成
>
>给每个 $a_i$ 赋值 $1或者-1$ 
>
>然后给矩阵的每一列排序，如果中间行都是 $1$ 那么胜利

##### 思路：

* 也就是你要让每一列 $-1$ 的数量最多为 $1$
* 我们让点 $i$ 代表数 $i$ 赋值为 $1$ ， $2n-i+1$ 代表点 $i$ 赋值为 $-1$
*  $2-SAT$ 来判断是否矛盾

```cpp
#include<bits/stdc++.h>
using namespace std;
 
const int N = 5e2 + 10;
 
vector<int> ed[N*2];
 
stack<int> st;
bool in[N*2];//是否在栈内
int dfs[N*2],low[N*2];
int ptop = 0;
int num = 0;//记录强连通分量的数量
int c[N*2];
void tarjan(int x){
	dfs[x] = low[x] = ++ptop;
	st.push(x);
	in[x] = 1;
	for(auto y : ed[x]){
		if(!dfs[y])
			tarjan(y);
		else if(in[y])
			low[x] = min(low[x],low[y]);
	}
	if(low[x] == dfs[x]){
		int y = x;
		num++;
		do{
			c[y] = num;
			y = st.top();
			st.pop();
			in[y] = 0;
		}while(y != x);
		c[x] = num;
	}
}
int n,m;
 
int main(){
	int t;cin >> t;
	while(t--){
		int n;cin >> n;
 
		ptop = 0;
		num = 0;
		for(int i = 0;i <= 2*n;i++){
			c[i] = in[i] = dfs[i] = low[i] = 0;
			ed[i].clear();
		}
 
		vector<vector<int>> ve(n + 1,vector<int>(4,0));
 
		for(int j = 1;j <= 3;j++)
			for(int i = 1;i <= n;i++)
				cin >> ve[i][j];
 
		for(int i = 1;i <= n;i++){
			for(int j = 1;j <= 3;j++){
				int x = abs(ve[i][j]);
				if(ve[i][j] < 0) x = 2 * n - x + 1;
				for(int k = 1;k <= 3;k++){
					if(k == j) continue;
					int y = abs(ve[i][k]);
					if(ve[i][k] < 0) y = 2 * n - y + 1;
					y = 2 * n - y + 1;
					ed[x].push_back(y);
 
					y = 2 * n - y + 1;
					x = 2 * n - x + 1;
					ed[y].push_back(x);
					x = 2 * n - x + 1;
				}
			}
		}
 
		bool f = 1;
		for(int i = 1;i <= n*2;i++) {
			if(!dfs[i]) {
				tarjan(i);
			}
		}
		for(int i = 1;i <= n;i++){
			if(c[i] == c[2*n + 1 - i]) f = 0;
		}
		if(!f) cout << "No" << '\n';
		else{
			cout << "Yes" << '\n';
		}
	}
}
```

## [**C - Sort**](https://atcoder.jp/contests/abc350/tasks/abc350_c?lang=en)

* 给一个序列，每次交换两个位置上的数，最后排成有序，问操作方式

##### 思路：

* 记录每个位置上的数与数对应的位置在哪里，维护即可

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
	vector<int> a(n + 1),whe(n + 1);
	for(int i = 1;i <= n;i++){
		cin >> a[i];
		whe[a[i]] = i;
	}
	queue<pair<int,int>> qu;
	for(int i = 1;i <= n;i++){
		if(whe[i] != i){
			qu.push({i,whe[i]});
			swap(a[i],a[whe[i]]);
			swap(whe[i],whe[a[whe[i]]]);
		}
	}
	cout << (int)qu.size() << '\n';
	while(!qu.empty()){
		auto [x,y] = qu.front();
		cout << x << ' ' << y << '\n';
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

## [**D - New Friends**](https://atcoder.jp/contests/abc350/tasks/abc350_d?lang=en)

* 朋友关系有双向传递性，给出一些朋友关系（不重复），问产生多少新的朋友关系

##### 思路：

* 双向传递性相当于集合，我们用并查集统计出每个集合有多少个人，一个集合内的朋友关系相当于完全图，边数就是朋友关系的数量，那么我们只要知道所有的朋友关系数量，再减去原本的即可得到新增的数量

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
const int N = 2e5 + 10;

int a[N];

int find(int x){
	if(a[x] == x) return x;
	else return a[x] = find(a[x]);
}

void test(){
	int n,m;cin >> n >> m;
	for(int i = 1;i <= n;i++) a[i] = i;
	for(int i = 1;i <= m;i++){
		int x,y;cin >> x >> y;
		a[find(x)] = find(y);
	}
	map<int,int> mp;
	for(int i = 1;i <= n;i++){
		mp[find(i)]++;
	}
	ll ans = 0;
	for(auto [x,y] : mp){
		ans += (ll)y * (y - 1) / 2;
	}
	cout << ans - m << '\n';
}

int main(){
	IOS
	int t = 1;//cin >> t;
	while(t--){
		test();
	}
}
```

## [**E - Toward 0**](https://atcoder.jp/contests/abc350/tasks/abc350_e?lang=en)

* 给出 $N,A,X,Y$ ，每次可选择支付 $X$ 代价使得 $N:=\lfloor\frac{N}{A}\rfloor$ 或者每次选择支付 $Y$ 代价使得 $N:=\lfloor \frac{N}{rand} \rfloor$ ， $rand$ 为 $[1,6]$ 等可能的正整数

##### 思路：

* 每次除一个数，有可能到达的数不多，记忆化搜索即可

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

unordered_map<ll,double> mp;

int x,y,a;

double find(ll no){
	if(no == 0) return 0;
	if(mp.count(no)) return mp[no];
	else{
		double mi = find(no / a) + x;
		double tem = 0;
		for(int i = 2;i <= 6;i++){
			tem += find(no / i) / 5.0;
		}
		tem += 6.0 * y / 5.0;
		mi = min(tem,mi);
		mp[no] = mi;
	}
	return mp[no];
}

void test(){
	ll n;
	cin >> n >> a >> x >> y;
	printf("%.7lf",find(n));
}

int main(){
	IOS
	int t = 1;//cin >> t;
	while(t--){
		test();
	}
}
```

## [**F - Transpose**](https://atcoder.jp/contests/abc350/tasks/abc350_f?lang=en)

给你一个由大写和小写英文字母 `(`, 和 `)` 组成的字符串 $S = S_1 S_2 S_3 \dots S_{|S|}$ 。 
字符串 $S$ 中的小括号已正确匹配。

重复下面的操作，直到不能再进行操作为止：

- 首先，选择一对满足以下所有条件的整数 $(l, r)$ ：
  - $l < r$
  - $S_l =$ `(`
  - $S_r =$ `)`
  - 每个字符 $S_{l+1}, S_{l+2}, \dots, S_{r-1}$ 都是一个大写或小写英文字母。
- 让 $T = \overline{S_{r-1} S_{r-2} \dots S_{l+1}}$ .
  - 这里， $\overline{x}$ 表示通过切换 $x$ 中每个字符的大小写（从大写到小写，反之亦然）得到的字符串。
- 然后，删除 $S$ 中的 $l$ 至 $r$ 字符，并在删除位置插入 $T$ 。

可以证明，使用上述操作可以删除字符串中的所有 `(`s和`)`s，而且最终的字符串与操作的方式和顺序无关。 
确定最终字符串。

$S$ 中的括号正确匹配意味着什么？首先，正确的括号序列定义如下：

- 正确的括号序列是满足以下条件之一的字符串：

- 它是空字符串。
- 存在一个正确的括号序列 $A$ ，且该字符串是由按此顺序连接的 `(`, $A$ ,`)`组成的。
- 存在非空的正确括号序列 $A$ 和 $B$ ，字符串由按此顺序连接的 $A$ 和 $B$ 构成。

当且仅当从 $S$ 中提取的 `（`s 和 `）`s 在不改变顺序的情况下构成一个正确的小括号序列时， $S$ 中的小括号才能正确匹配。

* 括号内的字母大小写变换并且顺序颠倒，问最后的字符串是什么

##### 思路：

* 首先，大小写很容易，看每个字母被几个括号包上即可，我们处理每个字母被几个括号包上就看左边的 $($ 多几个即可
* 其次，我们考虑两个字母之间的顺序，我们假设两个字母同时被奇数个括号包括，那么它们的位置就要交换，也就是原本位置在前面的要放在后面
* 那么怎么得出两个字母之间几个括号对包括呢
* 那就是两个字母之间字母几个括号包上的最小值
* 用线段树得出即可

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

const int inf = 1e9 + 10;
const ll INF = 1e18 + 10;

const int MAXN = 5e5 + 10;

int _l[MAXN];

int a[MAXN];
class tree{
	public:
	ll mi[MAXN*4];
	ll l[MAXN*4],r[MAXN*4];
	void build(int _l,int _r,int x)
	{
		l[x] = _l,r[x] = _r;
		if(_l == _r)
		{
			mi[x] = a[_l];
		}
		else
		{
			int mid = (_l+_r)/2;
			build(_l,mid,lson);
			build(mid + 1,_r,rson);
			update(x);
		}
	}
	void update(int x){
		mi[x] = min(mi[lson],mi[rson]);
	}
	void down(int x){
		;
	}
	ll findmi(int _l,int _r,int x)
	{
		if(_l <= l[x] && _r >= r[x])
			return mi[x];
		else
		{
			down(x);
			int mid = (l[x] + r[x]) / 2;
			ll mi = 1e9 + 10;
			if(_l <= mid)
				mi = findmi(_l,_r,lson);
			if(_r >= mid + 1)
				mi = min(mi,findmi(_l,_r,rson));
			update(x);
			return mi;
		}
	}
}tr;

bool cmp(int x,int y){
	if(x > y){
		swap(x,y);
		if(tr.findmi(x,y,1) % 2 == 0)
			return x > y;
		else
			return x < y;
	}
	if(tr.findmi(x,y,1) % 2 == 0)
		return x < y;
	else
		return x > y;
}

void test(){
	string s;cin >> s;
	int len = s.size();
	s = ' ' + s;
	for(int i = 1;i <= len;i++){
		a[i] = a[i - 1];
		if(s[i] == '(') a[i]++;
		else if(s[i] == ')') a[i]--;
	}
	tr.build(1,len,1);
	vector<int> ord;
	for(int i = 1;i <= len;i++){
		if(s[i] != '(' && s[i] != ')') {
			ord.push_back(i);
		}
	}
	sort(ord.begin(),ord.end(),cmp);
	for(auto x : ord){
		if(a[x] % 2 == 1){
			if(s[x] >= 'a' && s[x] <= 'z') cout << (char)(s[x] - 32);
			else cout << (char)(s[x] + 32);
		}else
			cout << s[x];
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

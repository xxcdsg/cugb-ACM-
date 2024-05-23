## [A. Phone Desktop](https://codeforces.com/problemset/problem/1974/A)

> $2*2$ 的块不能横跨不同屏幕，只能放在同一块屏幕上

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
	int x,y;cin >> x >> y;

	int ans = (y + 1) / 2;
 
	int tem = ans * 15 - y * 4;
 
	ans += (max(0,x - tem) + 14) / 15;
 
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

## [B. Symmetric Encoding](https://codeforces.com/contest/1974/problem/B)

* 模拟即可

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
	set<char> se;
	for(auto c : s){
		se.insert(c);
	}
	string t;
	for(auto x : se){
		t = t + x;
	}
	int len = t.size();
	unordered_map<char,char> mp;
	for(int i = 0;i < len;i++){
		mp[t[i]] = t[len - i - 1];
	}
	for(auto c : s){
		cout << mp[c];
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

## [C. Beautiful Triple Pairs](https://codeforces.com/contest/1974/problem/C)

>对于所有的三元组 $[a_j,a_{j+1},a_{j+2}]$ 来说，求满足以下其中一个条件的三元组对数
>
> $b_1\neq c_1,b_2= c_2,b_3= c_3$
>
> $b_1= c_1,b_2\neq c_2,b_3= c_3$
>
> $b_1= c_1,b_2= c_2,b_3\neq c_3$

##### 思路：

* 容斥原理

* 满足上述条件相当于满足

*  $b_2= c_2,b_3= c_3或$

   $b_1= c_1,b_3= c_3或$

   $b_1= c_1,b_2= c_2$

  并且

  $[b_1,b_2,b_3]\neq [c_1,c_2,c_3]$

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
	map<pair<int,int>,int> mp12,mp23,mp13;
	map<tuple<int,int,int>,int> mp;
	ll ans = 0;
	for(int i = 3;i <= n;i++){
		pair<int,int> pa12,pa23,pa13;
		tuple<int,int,int> tu;
		auto &[x,y,z] = tu;
		x = a[i - 2],y = a[i - 1],z = a[i];
		pa12 = {a[i - 2],a[i - 1]};
		pa13 = {a[i - 2],a[i]};
		pa23 = {a[i - 1],a[i]};
		ans += mp12[pa12]++;
		ans += mp13[pa13]++;
		ans += mp23[pa23]++;
		ans -= 3 * mp[tu]++;
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

## [D. Ingenuity-2](https://codeforces.com/contest/1974/problem/D)

>给一个由字符 $'N','S','E','W'$ 字符组成的字符串
>
>分别代表向北、向南、向东、向西运动一米
>
>你需要将字符串按字符分配给两个人，使得两个人运动到的位置相同
>
>两个人初始位置相同
>
>每个人至少分配一个字符

##### 思路：

* 首先，我们很容易知道，如果有答案，那么应该移动到的位置
* 如果要移动的位置不是 $(0,0)$ ，那么我们先让第一个人移动到目标位置，其他字符都给另外一个人即可
* 然后因为每个人至少分配一个字符，因此如果要移动的位置是 $(0,0)$ ，那么就有可能一个人不移动就到要移动的位置
* 因此特殊处理最终位置为 $(0,0)$ 的情况，具体就是让第一个字符分配给第一个人，然后让这个人只接受一个反方向的字符，其他字符都给另外一个人

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
	s = ' ' + s;
	int N = 0,E = 0;
	for(int i = 1;i <= n;i++){
		if(s[i] == 'N') N++;
		else if(s[i] == 'S') N--;
		else if(s[i] == 'E') E++;
		else if(s[i] == 'W') E--;
	}
 
	if(N == 0 && E == 0 && n == 2){
		cout << "NO\n";
	}else{
		if(N % 2 == 0 && E % 2 == 0){
			if(N == 0 && E == 0){
				cout << 'R';
				char c = s[1];
				if(c == 'N') N--;
				if(c == 'S') N++;
				if(c == 'E') E--;
				if(c == 'W') E++;
				for(int i = 2;i <= n;i++){
					bool f = 0;
					c = s[i];
					if(N > 0 && c == 'N') N--,f = 1;
					if(N < 0 && c == 'S') N++,f = 1;
					if(E > 0 && c == 'E') E--,f = 1;
					if(E < 0 && c == 'W') E++,f = 1;
					if(f) cout << 'R';
					else cout << 'H';
				}
			}else{
				N /= 2;
				E /= 2;
				for(int i = 1;i <= n;i++){
					char c = s[i];
					bool f = 0;
					if(N > 0 && c == 'N') N--,f = 1;
					if(N < 0 && c == 'S') N++,f = 1;
					if(E > 0 && c == 'E') E--,f = 1;
					if(E < 0 && c == 'W') E++,f = 1;
					if(f) cout << 'R';
					else cout << 'H';
				}
			}
			cout << '\n';
		}else
			cout << "NO\n";
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

## [E. Money Buys Happiness](https://codeforces.com/contest/1974/problem/E)

> 每个月得到 $x$ 元
>
> 第 $i$ 月可花费 $c_i$ 来获得 $h_i$ 幸福感 (每月至多一次)
>
> 第 $i$ 月获得的钱只能在下面的月份使用
>
> 问最大能获得的幸福感

##### 思路：

*  $\sum h_i\leq 1e5$ ，我们设 $dp[i]$ 代表幸福值为 $i$ 时，最少花费的钱
* 那么我们有转移方程 $dp[j+c_i]=max(dp[j+c_i],dp[j]+h_i)$
* 根据单次背包的方法，我们从后往前维护更新背包，即可只创建一个dp数组

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
 
const int N = 1e5 + 10;
 
bool f[N];
ll dph[N];//获得该幸福值剩下的最多的钱
 
void test(){
 
	for(int i = 1;i <= N - 10;i++){
		dph[i] = 0;
		f[i] = 0;
	}
 
	int m,x;cin >> m >> x;
 
	dph[0] = -x;
	ll sumh = 0;
	f[0] = 1;
	int ma = 0;
 
	for(int i = 1;i <= m;i++){
		int c,h;cin >> c >> h;
		for(int j = sumh;j >= 0;j--){
			if(f[j] && x + dph[j] >= c){
				dph[j + h] = max(x + dph[j] - c,dph[j + h]);
				f[j + h] = 1;
				ma = max(ma,j + h);
			}
			if(f[j])
				dph[j] += x;
		}
		sumh += h;
	}
	cout << ma << '\n';
}
 
int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [F. Cutting Game](https://codeforces.com/contest/1974/problem/F)

>每次操作四个方向切割网格，两个人轮流操作
>
>网格上有一些特殊点，当操作将特殊点切割时，得分
>
>两个人轮流操作并给出操作方向与长度
>
>问两个人各自的得分

##### 思路：

* 如果只有一维，那么我们用双指针即可简单解决
* 那么二维我们也用双指针，将一维分为二维
* 那么这两维相互影响，表现在有可能其中一维将点去除，但另一维没有去除，导致重复计算
* 那么我们将每个被去除的点标记，被标记的点不参与计数，那么就保证了不重复计算

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
 
bool vis[N];
 
void test(){
	int a,b,n,m;cin >> a >> b >> n >> m;
	
	for(int i = 1;i <= n;i++) vis[i] = 0;
	
	vector<pair<int,int>> ud(n + 1),lr(n + 1);
 
	int U = n,D = 1,L = 1,R = n;
 
	for(int i = 1;i <= n;i++){
		cin >> ud[i].first >> lr[i].first;
		ud[i].second = i;
		lr[i].second = i;
	}
	sort(ud.begin(),ud.end());
	sort(lr.begin(),lr.end());
 
	int nou = a,nod = 1,nol = 1,nor = b;
 
	int ans[2] = {0,0};
	int op = 1;
 
	for(int i = 1;i <= m;i++){
		char c;cin >> c;
		int x;cin >> x;
		if(c == 'U') nod += x;
		else if(c == 'D') nou -= x;
		else if(c == 'L') nol += x;
		else nor -= x;
		bool f = 1;
		while(U >= D && f){
			f = 0;
			if(nou < ud[U].first){
				f = 1;
				if(!vis[ud[U].second]){
					ans[op]++;
					vis[ud[U].second] = 1;
				}
				U--;
			}
			if(nod > ud[D].first){
				f = 1;
				if(!vis[ud[D].second]){
					ans[op]++;
					vis[ud[D].second] = 1;
				}
				D++;
			}
		}
		f = 1;
		while(R >= L && f){
			f = 0;
			if(nor < lr[R].first){
				f = 1;
				if(!vis[lr[R].second]){
					ans[op]++;
					vis[lr[R].second] = 1;
				}
				R--;
			}
			if(nol > lr[L].first){
				f = 1;
				if(!vis[lr[L].second]){
					ans[op]++;
					vis[lr[L].second] = 1;
				}
				L++;
			}
		}
		op = 1 - op;
	}
	cout << ans[1] << ' ' << ans[0] << '\n';
}
 
int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [G. Money Buys Less Happiness Now](https://codeforces.com/contest/1974/problem/G)

> 同 E
>
> 但是 $h_i=1$ ，并且月份最多到 $2e5$

##### 思路：

* 贪心，每次都看是否能买，如果能就买
* 否则将之前买的最贵的与现在的比较，如果现在的比较便宜，那么说明前面的我们可以不买，到这次再买

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
	int m,x;cin >> m >> x;
	vector<int> c(m + 1);
	ll sum = -x;
	priority_queue<int,vector<int>,less<int> > qu;
	for(int i = 1;i <= m;i++){
		cin >> c[i];
		sum += x;
		if(sum >= c[i]){
			qu.push(c[i]);
			sum -= c[i];
		}else if(!qu.empty() && c[i] < qu.top()){
			qu.push(c[i]);
			sum -= c[i];
			sum += qu.top();
			qu.pop();
		}
	}
	cout << qu.size() << '\n';
}
 
int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

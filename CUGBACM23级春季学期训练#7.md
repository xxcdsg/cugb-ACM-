## [A - B. 找机厅](https://www.luogu.com.cn/problem/P10234)

* 经典记录操作的搜索，我们记录到每个点的前一个操作，那么反向递归回去再反向输出即可得到一条可行路径

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;

#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;

const int N = 2e3 + 10;

int a[N][N];

pair<int,int> op[N][N];

int dis[N][N];

bool vis[N][N];

void test(){
	int n,m;cin >> n >> m;
	for(int i = 1;i <= n;i++)
		for(int j = 1;j <= m;j++){
			vis[i][j] = 0;
			char c;cin >> c;
			a[i][j] = c - '0';
		}
	queue<pair<int,int>> qu;
	qu.push({1,1});
	vis[1][1] = 1;
	while(!qu.empty()){
		int x,y;
		x = qu.front().first;
		y = qu.front().second;
		qu.pop();
		for(int i = -1;i <= 1;i++)
			for(int j = -1;j <= 1;j++){
				if(i != 0 && j != 0 || i == 0 && j == 0) continue;
				int xx = x + i,yy = y + j;
				if(xx >= 1 && xx <= n && yy >= 1 && yy <= m && !vis[xx][yy] && a[xx][yy] == 1 - a[x][y]){
					qu.push({xx,yy});
					vis[xx][yy] = 1;
					dis[xx][yy] = dis[x][y] + 1;
					op[xx][yy] = {i,j};
				}
			}
	}
	if(vis[n][m]){
		cout << dis[n][m] << '\n';
		stack<char> st;
		int x = n,y = m;
		while(x != 1 || y != 1){
			if(op[x][y] == (pair<int,int>){-1,0})
				st.push('U');
			else if(op[x][y] == (pair<int,int>){1,0})
				st.push('D');
			else if(op[x][y] == (pair<int,int>){0,1})
				st.push('R');
			else
				st.push('L');
			pair<int,int> opp = op[x][y];
			x = - opp.first + x;
			y = - opp.second + y;
		}
		while(!st.empty()){
			cout << st.top();
			st.pop();
		}
		cout << '\n';
	}else
		cout << -1 << '\n';
}

int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [B - 滑雪](https://www.luogu.com.cn/problem/P1434)

* 记忆化搜索，通过记录每个点的最大下滑距离来减少搜索的复杂度

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;

#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;

const int N = 1e2 + 10;

int a[N][N];

int ma[N][N] = {0};

int mvi[] = {1,-1,0,0},mvj[] = {0,0,1,-1};

int r,c;

int find(int x,int y){
	if(ma[x][y]) return ma[x][y];
	else{
		ma[x][y] = 1;
		for(int i = 0;i < 4;i++){
			int _x = x + mvi[i],_y = y + mvj[i];
			if(_x > r || _x <= 0 || _y > c || _y <= 0 || a[x][y] <= a[_x][_y]) continue;
			ma[x][y] = max(find(_x,_y) + 1,ma[x][y]);
		}
	}
	return ma[x][y];
}

void test(){
	cin >> r >> c;
	for(int i = 1;i <= r;i++)
		for(int j = 1;j <= c;j++)
			cin >> a[i][j];
	int ans = 0;
	for(int i = 1;i <= r;i++)
		for(int j = 1;j <= c;j++){
			ans = max(ans,find(i,j));
		}
	cout << ans << endl;
}

int main(){
	IOS
	int t = 1;//cin >> t;
	while(t--){
		test();
	}
}
```

## [A. Setting up Camp](https://codeforces.com/contest/1945/problem/A)

* 简单数学，明显当 $c$ 不足以和多余的 $b$ 组成 $3$ 时不能满足所有人的愿望
* 数量就是 $a+\lceil(b+c)/3\rceil$

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
	int a,b,c;cin >> a >> b >> c;
	int num = a + (b + c + 2) / 3;
	if(c < ((3 - b % 3) == 3 ? 0 : 3 - b % 3)) cout << -1 << '\n';
	else cout << num << '\n';
}

int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [B. Fireworks](https://codeforces.com/contest/1945/problem/B)

* 有两个烟花发射装置，一个每隔 $a$ 发射一次，另一个每隔 $b$ 发射一次
* 烟花持续时间为 $m+1$ 
* 问最多能看到多少烟花

##### 思路：

* 对于一种烟花来说，出现的数量最多为 $(a+m)/a$
* 那么答案一定小于等于 $(a+m)/a+(b+m)/b$
* 又当经过足够长时间后，每次烟花发射后，对于一种烟花来说出现数量就为最多的出现次数
* 同时我们可以取最小公倍数来对齐烟花发射时间，因此肯定能取到最大值
* 因此答案就为  $(a+m)/a+(b+m)/b$

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
	ll a,b,m;cin >> a >> b >> m;
	cout << (a + m) / a + (b + m) / b << '\n';
}

int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [C. Left and Right Houses](https://codeforces.com/contest/1945/problem/C)

* 你要找一个位置将左右分开， $a[i]=0$ 代表它想要在左边， $a[i]=1$ 代表它想要在右边，让左边的 $0$ 的数量大于等于 $\lceil i/2\rceil$ ，右边 $1$ 的数量大于等于 $\lceil (n-i)/2 \rceil$
* 问满足上面条件 $|n/2-i|$ 最小时， $i$ 的取值，如果有多个，取较小的

##### 思路：

* 从小到大枚举 $i$ ，同时动态维护左边 $0$ 的数量与右边 $1$ 的数量，按题意更新答案

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
	int l = 0,r = n;
	int tl = 0,tr = 0;
	for(auto c : s){
		if(c == '1') tr++;
	}
	int ans = -1;
	if(tr >= (r + 1) / 2) ans = l;
	for(auto c : s){
		l++;
		r--;
		if(c == '1') tr--;
		if(c == '0') tl++;
		if(tr >= (r + 1) / 2 && tl >= (l + 1) / 2 && abs(n - 2*l) < abs(n - 2*ans))
			ans = l;
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

## [D. Seraphim the Owl](https://codeforces.com/problemset/problem/1945/D)

* 你前面有 $n$ 个人，每个人都有 $a[i]$ 与 $b[i]$ 两个数
* 假设你在 $j$ ，如果你与前面的人 $i$ 交换位置，那么你需要支付 $a[i]+\sum_{k=i+1}^jb[k]$ 的代价，也就是于要交换的支付 $a$ ，中间的支付 $b$
* 你可以与前面的人交换任意次
* 问到前 $m$ 位置的最小代价

##### 思路：

* 如果我们要的 $i$ 位置，那么最后一步操作肯定是与 $i$ 位置交换，因此 $a[i]$ 我们肯定是要取到的，但是中间的是取 $a$ 还是 $b$ 就不确定
* 因为交换是任意次的，因此除了 $i$ 位置，中间是取 $a$ 还是 $b$ 就是任意的
* 那么我们都取最小的即可

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
	int n,m;cin >> n >> m;
	ll mi = INF;
	vector<int> a(n),b(n);
	for(int &x : a) cin >> x;
	for(int &x : b) cin >> x;
	ll no = 0;
	m--;
	for(int i = n - 1;i >= 0;i--){
		if(i <= m) mi = min(mi,a[i] + no);
		no += min(a[i],b[i]);
	}
	cout << mi << '\n';
}

int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [E. Binary Search](https://codeforces.com/problemset/problem/1945/E)

* 对一个非有序排列二分查找某个数，你可以最多交换两次位置，使得最后能够使得 $l$ 落在那个数的位置，问交换方法

##### 思路：

* 如果没有数 $x$ ，那么我们跑二分查找，最后停下的 $l$ 位置我们再将 $x$ 与那个位置交换，那么我们就投机取巧地"找到"了数 $x$ 
* 但是我们要考虑交换数 $x$ 是否会对查找产生影响
* 我们先考虑不产生影响的，那就是根本不查找 $x$ 的位置
* 我们一开始 $l=1,r=n+1$ ， $mid=\lfloor(l+r)/2 \rfloor$ ，假设 $p[mid]<=x$ ，那么 $l'=mid,r=n+1$，此时如果 $x$ 在 $mid$ 前面（或者就是 $mid$ ），也就是 $xpos<=mid$ ，那么 $x$ 就不会被搜索到，那么在二分最后交换 $p[lend],p[xpos]$，就能实现最后查找到 $x$
* 同理如果 $p[mid]>=x$ ，并且 $x$ 在 $mid$ 后面...
* 如果不满足上面两条，那么我们也可以加一次交换使得上面的满足
* 当然有特殊情况就是 $x=1$ 与 $x=n$ 时，那么我们就是找不到小于或者大于 $x$ 的数，此时我们让 $x=1$ 放在 $1$ 位置，因为后面所有数都大于 $x$ 因此也能找到 $x$ ，同理如果 $x=n$ 那么放在最后即可

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
	vector<int> p(n + 1);
	int xpos = 0;
	for(int i = 1;i <= n;i++){
		cin >> p[i];
		if(p[i] == x) xpos = i;
	}
	int mid = (2 + n) / 2;
	bool f = 0;
	queue<pair<int,int>> qu;
	if(xpos <= mid){
		for(int i = 1;i <= n;i++){
			if(p[i] < x && i != xpos){
				f = 1;
				qu.push({i,mid});
				swap(p[i],p[mid]);
				if(xpos == mid) xpos = i;
				break;
			}
		}
	}else{
		for(int i = 1;i <= n;i++){
			if(p[i] >= x && i != xpos){
				f = 1;
				qu.push({i,mid});
				swap(p[i],p[mid]);
				if(xpos == mid) xpos = i;
				if(xpos == i) xpos = mid;
				break;
			}
		}
	}
	if(f){
		int l = 1,r = n + 1;
		while(l + 1 != r){
			int mid = (l + r) / 2;
			if(p[mid] > x)
				r = mid;
			else
				l = mid;
		}
		qu.push({l,xpos});
		cout << 2 << '\n';
		while(!qu.empty()){
			cout << qu.front().first << ' ' << qu.front().second << '\n';
			qu.pop();
		}
	}else if(x == 1){
		cout << 1 << '\n';
		cout << xpos << ' ' << 1 << '\n';
	}else{
		cout << 1 << '\n';
		cout << xpos << ' ' << n << '\n';
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


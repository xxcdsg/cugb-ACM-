## [**C - One Time Swap**](https://atcoder.jp/contests/abc345/tasks/abc345_c?lang=en)

* 给字符串一次交换，问这个字符串可以变成多少种不同的字符串

##### 思路：

* 也就是计算 $| { (i,j),s[i]\neq s[j]并且i\neq j } |$ 加上字符串能否不变
* 从前往后边维护不同字母数量，边统计即可，最后找有没有两个字符相同来判断字符串能不能不变

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

const int N = 1e6 + 10;

int num[27] = {0};

void test(){
	string s;cin >> s;
	ll ans = 0;
	bool f = 0;
	for(auto c : s){
		for(int j = 0;j < 26;j++){
			if(c - 'a' != j && num[j])
				ans+=num[j];
			else if(c - 'a' == j && num[j])
				f = 1;
		}
		num[c - 'a']++;
	}
	if(f) ans++;
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

## [**D - Tiling**](https://atcoder.jp/contests/abc345/tasks/abc345_d)

* 给 $n$ 个长方形，问能否不重叠，不超出范围的放满大长方形范围

##### 思路：

* $n$ 很小，长方形范围也很小，直接爆搜，我每次找最左上角的空位然后放
* 我没试过每个位置都当作左上角得放置方法，不知道会不会T

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

const int N = 11;

pair<int,int> pa[N];

int belong[N][N];

bool use[N];

int n;

int h,w;

pair<int,int> find(){
	for(int i = 1;i <= h;i++)
		for(int j = 1;j <= w;j++)
			if(belong[i][j] == 0)
				return {i,j};
	return {-1,-1};
}

bool put(){
	auto [x,y] = find();
	if(x == -1) return 1;
	else{
		for(int i = 1;i <= n;i++){
			if(use[i]) continue;
			if(pa[i].first + x - 1 <= h && pa[i].second + y - 1 <= w){
				bool f = 1;
				for(int j = x;j <= pa[i].first + x - 1;j++)
					for(int k = y;k <= pa[i].second + y - 1;k++)
						if(belong[j][k]) f = 0;
				if(f){
					for(int j = x;j <= pa[i].first + x - 1;j++)
						for(int k = y;k <= pa[i].second + y - 1;k++)
							belong[j][k] = i;
					use[i] = 1;
					if(put()) return 1;
					else{
						use[i] = 0;
						for(int j = x;j <= pa[i].first + x - 1;j++)
							for(int k = y;k <= pa[i].second + y - 1;k++)
								belong[j][k] = 0;
					}
				}
			}
			swap(pa[i].first,pa[i].second);
			if(pa[i].first + x - 1 <= h && pa[i].second + y - 1 <= w){
				bool f = 1;
				for(int j = x;j <= pa[i].first + x - 1;j++)
					for(int k = y;k <= pa[i].second + y - 1;k++)
						if(belong[j][k]) f = 0;
				if(f){
					for(int j = x;j <= pa[i].first + x - 1;j++)
						for(int k = y;k <= pa[i].second + y - 1;k++)
							belong[j][k] = i;
					use[i] = 1;
					if(put()) return 1;
					else{
						use[i] = 0;
						for(int j = x;j <= pa[i].first + x - 1;j++)
							for(int k = y;k <= pa[i].second + y - 1;k++)
								belong[j][k] = 0;
					}
				}
			}
		}
	}
	return 0;
}

void test(){
	cin >> n;
	cin >> h >> w;
	int sum = h*w;
	for(int i = 1;i <= n;i++){
		cin >> pa[i].first >> pa[i].second;
		sum -= pa[i].first * pa[i].second;
	}
	if(sum > 0)
		cout << "No\n";
	else{
		if(put())
			cout << "Yes\n";
		else
			cout << "No\n";
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

## [E. Arrow Path](https://codeforces.com/problemset/problem/1948/C)

*  $2*n$ 的$<>$矩阵，从 $(1,1)$ 开始走，每次任意移动一格（不可以不动，不可以移出矩阵），然后根据移动后的 $<>$ 再移动一位，问能否移动到 $(2,n)$

##### 思路：

* 也是搜，但是标记点，如果走过就不再走

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

string s[2];

bool vis[2][N];

bool f = 0;

int n;

void dfs(int x,int y){
	if(abs(x - 1) + abs(y - n) <= 1) f = 1;
	if(vis[x][y]) return;

	vis[x][y] = 1;


	int yy = y;
	if(s[1 - x][yy] == '>') yy++;
	else yy--;
	yy = min(max(1,yy),n);
	dfs(1-x,yy);

	if(y < n){
		yy = y;
		yy++;
		yy = min(max(1,yy),n);
		if(s[x][yy] == '>') yy++;
		else yy--;
		yy = min(max(1,yy),n);
		dfs(x,yy);
	}
	
	if(y > 1){
		yy = y;
		yy--;
		yy = min(max(1,yy),n);
		if(s[x][yy] == '>') yy++;
		else yy--;
		yy = min(max(1,yy),n);
		dfs(x,yy);
	}
}

void test(){
	cin >> n;
	cin >> s[0] >> s[1];
	s[0] = ' ' + s[0];
	s[1] = ' ' + s[1];
	f = 0;
	for(int i = 0;i <= n;i++)
		vis[0][i] = vis[1][i] = 0;
	dfs(0,1);
	if(f) cout << "Yes\n";
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

## [F. Tandem Repeats?](https://codeforces.com/problemset/problem/1948/D)

* 给一个字符串，找最大偶数长度连续子序列 $s[be ... be+2 * len-1]$ 满足 $s[be...be+len-1]=s[be+len...be+2 * len-1]$
* 并且 $?$ 字符可为任意的字符

##### 思路：

* 假设 $s[i...i+len-1]与s[i+len...i+len * 2-1]$ 前 $k$ 个字母相同，那么 $s[i+1...i+len]与s[i+len+1...i+2 * len]$ 就至少有 $k-1$ 个字母相同
* 那么我们枚举 $len$ 的长度与 $be$ 复杂度为 $O(n^2)$ ，对于同一个 $len$ 来说，相同字母增加的数量不超过 $n$ ，因此不会增加复杂度，因此复杂度为 $O(n^2)$

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

const int N = 5e3 + 10;

int ma = 0;

int ans[N];

void test(){
	string s;cin >> s;
	int len = s.size();
	int ma = 0;
	for(int i = 1;i <= len / 2;i++){//长度
		int no = 0;//相同长度
		for(int j = 0;j <= len - i * 2;j++){//起始位置
			while(no < i && (s[j + no] == s[j + i + no] || s[j + no] == '?' || s[j + i + no] == '?')) no++;
			if(no == i){
				ma = i;
				break;
			}
			no--;
			no = max(no,0);
		}
	}
	cout << ma * 2 << '\n';
}

int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [**G - Colorful Subsequence**](https://atcoder.jp/contests/abc345/tasks/abc345_e?lang=en)

* $N$ 个球，每个球有相应的颜色与权值，删除确切的 $K$ 个球，使得相邻的球颜色互不相同，求剩下的球的最大权值和

##### 思路：

* 假设 $dp[i][j]$ 代表取第 $i$ 个球，前面丢掉 $k$ 个球，剩下球的最大总值
* 那么假设 $dp[i_1][j_1]$ 要转移到 $dp[i_2][j_2]$ ,那么我们有

$$
\begin{cases}
i_2>i_1\\
j_2-j_1=i_2-i_1-1\\
C[j_1]\neq C[j_2]
\end{cases}
$$

* 对于第一个条件，我们从前往后枚举即可
* 对于第二个条件我们转化一下

$$
\to j_2-i_2+1=j_1-i_1
$$

* 那么我们把所有 $j-i$ 相同的放在一起，取最大的即可
* 对于第三个条件，有可能满足前面两个条件最大的 $dp$ 值，它的颜色与要转移的相同，那么我们在记录 $dp$ 值时不仅要记录 $dp$ 值，还要记录颜色，因为一种颜色不能与两种不同颜色都相同，那么我们只要给每个 $dp$ 值记录两个不同颜色的最大 $dp$ 值，那么就可以实现转移

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

const int N = 2e5 + 10;
const int K = 5e2 + 10;

int dp[N][K];

int c[N],v[N];
int cc[N],vv[N];

unordered_map<int,tuple<int,int,int,int>> mp;

void add(int val,int c,int x){
	if(mp.count(x)){
		auto & [val1,c1,val2,c2] = mp[x];
		if(c1 == c){
			if(val > val1) val1 = val;	
		}else if(c2 == c){
			if(val > val2) val2 = val;
			if(val2 > val1){
				swap(val2,val1);
				swap(c2,c1);
			}
		}else if(val2 < val || val2 == 0){
			val2 = val;
			c2 = c;
			if(val2 > val1){
				swap(val2,val1);
				swap(c2,c1);
			}
		}
	}else{
		auto & [val1,c1,val2,c2] = mp[x];
		val1 = val;
		c1 = c;
	}
}

int find(int x,int c){
	if(mp.count(x)){
		auto & [val1,c1,val2,c2] = mp[x];
		if(c1 != c) return val1;
		else if(val2 != 0 && c != c2)
			return val2;
		else
			return 0;
	}else
		return 0;
}

void test(){
	int ptop,k;cin >> ptop >> k;
	for(int i = 1;i <= ptop;i++)
		cin >> cc[i] >> vv[i];
	if(k >= 0){
		for(int i = 1;i <= ptop;i++){
			for(int j = 0;j <= min(k,i - 1);j++){
				if(j == i - 1){//前面全丢了
					dp[i][j] = vv[i];
					add(dp[i][j],cc[i],j - i);
				}else{
					int x = find(j - i + 1,cc[i]);
					if(x != 0){
						dp[i][j] = x + vv[i];
						add(dp[i][j],cc[i],j - i);
					}
				}
			}
		}
		if(mp.count(k - ptop)){
			auto & [val1,c1,val2,c2] = mp[k - ptop];
			cout << val1 << '\n';
		}
		else
			cout << -1 << '\n';
	}else
		cout << -1 << '\n';
}

signed main(){
	IOS
	int t = 1;//cin >> t;
	while(t--){
		test();
	}
}
```


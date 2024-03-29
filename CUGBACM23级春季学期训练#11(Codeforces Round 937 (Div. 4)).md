## [A. Stair, Peak, or Neither?](https://codeforces.com/contest/1950/problem/A)

* 按题意即可

## [B. Upscaling](https://codeforces.com/contest/1950/problem/B)

* 利用奇偶性

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
	for(int i = 1;i <= 2*n;i++){
		for(int j = 1;j <= 2*n;j++){
			if(((((i - 1) / 2) + (j - 1) / 2) % 2) == 0)
				cout << '#';
			else
				cout << '.';
		}
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

## [C. Clock Conversion](https://codeforces.com/contest/1950/problem/C)

* 判断然后给小时-12

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
	int h = (s[0] - '0') * 10 + (s[1] - '0');
	int m = (s[3] - '0') * 10 + (s[4] - '0');
	string ss = "AM";
	if(h >= 12){
		h -= 12;
		ss = "PM";
	}
	if(h == 0)
		h = 12;

	cout << h / 10 << h % 10 << ':' << m / 10 << m % 10 << ' ' << ss << '\n';
}

int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [D. Product of Binary Decimals](https://codeforces.com/contest/1950/problem/D)

* 判断一个小于等于 $1e5$ 的数能否分解为 $1与0$ 组成的十进制数的乘积（可分解为不止两个）

##### 思路：

* 我们先预处理出所有的数能否组成即可
* 首先，如果一个数如果就是由 $10$ 组成的，那么直接标记为可以
* 否则，就找它的因子，因为因子肯定小于等于它，而如果我们从小到大遍历，那么找的那些因子肯定已经被判断过了
* 复杂度 $O(n\sqrt{n}+t)$

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
 
bool check(int x){
	while(x){
		if(x % 10 == 0 || x % 10 == 1){
			x /= 10;
		}else
			return 0;
	}
	return 1;
}
 
bool vis[N];
 
void init(){
	for(int i = 1;i <= N - 10;i++){
		if(check(i)){
			vis[i] = 1;
			continue;
		}
		int sq = sqrt(i);
		for(int j = 1;j <= sq;j++){
			if(i % j == 0 && vis[j] && vis[i / j]){
				vis[i] = 1;
				break;
			}
		}
	}
}
 
void test(){
	int n;cin >> n;
	if(vis[n]) cout << "YES\n";
	else
		cout << "NO\n";
}
 
int main(){
	IOS
	init();
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [E. Nearly Shortest Repeating Substring](https://codeforces.com/contest/1950/problem/E)

* 给一个长度为 $n$ 的字符串
* 求一个最短字符串的长度 $k$ ，使得它的复制到恰好为长度为 $n$ 时与所给的字符串相差的字符至多为 $1$

##### 思路：

*  $k$ 显然只能取 $n$ 的因子，而 $1e9$ 以内的数拥有最多因子的数最多就 $1344$ 个，那么 $2e5$ 内拥有最多因子的数肯定更小，反正 $k$ 的取值不多，保留枚举每个 $k$ 即可
* 那么假设出了 $k$ ，那么我们就开始复制，我们每一位每一位判断，如果这一位上对应原字符串的字符都相同，那么这一位肯定取这个字符最好
* 如果只有一个字符与其他字符不同，那么标记一下，我们用掉了相差一个字符，如果下次还有与其他对应字符不同的情况，就说明不行
* 如果差不止一个，那么肯定不行

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
	int sq = sqrt(n);
	vector<int> ve;
	for(int i = 1;i <= sq;i++){
		if(n % i == 0){
			ve.push_back(i);
			ve.push_back(n / i);
		}
	}
 
	function<bool(int)> check = [&](int x){
		bool ff = 1;
		for(int i = 1;i <= x;i++){
			map<char,int> mp;
			for(int k = 1;k <= n / x;k++){
				mp[s[i + (k - 1) * x - 1]]++;
			}
			bool fff = 1;
			if(mp.size() > 2)
				return 0;
			for(auto [z,y] : mp){
				if(y != n/x){
					if(y == 1 || y == n/x - 1) fff = 0;
					else
						return 0;
				}
			}
			if(fff == 0 && ff == 0)
				return 0;
			ff &= fff;
		}
		return 1;
	};	
 
	sort(ve.begin(),ve.end());
	int ans = n;
	for(auto it = ve.begin();it != ve.end();it++){
		if(check(*it)){
			ans = *it;
			break;
		}
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

## [F. 0, 1, 2, Tree!](https://codeforces.com/contest/1950/problem/F)

* 分别给出有两个子节点的节点个数，一个子节点的节点个数，与没有子节点的节点个数，问这个树是否存在，如果存在输出这棵树的最小深度

##### 思路：

* 我们肯定是拿子节点个数为 $0$ 的节点来放在最下面，那么如果  $c$ 的数量与上面的要匹配上的位置数量不匹配，那么这棵树肯定不存在
* 我们认为根节点要匹配 $1$ 个节点，每放入一个子节点个数为 $2$ 的节点，那么这棵树需要的子节点个数为 $0$ 的节点明显会加 $1$ （因为它用掉了一个，需要两个），同理 $1$ 无影响
* 因此这棵树存在的条件就是 $a+1=c$
* 为了让这颗树深度最低，那么我们肯定要让 $2$ 节点尽量放在上面，让这棵树看起来尽量扁
* 那么你可以用数学方法来算出放入所有的 $2$ 后的情况，然后放入 $1$
* 不过由于所给的数据较小，用队列存入 $深度，需要的子节点数量$ ，然后模拟即可

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
	if(a + 1 != c){
		cout << -1 << '\n';
		return;
	}
	queue<pair<int,int>> qu;
	qu.push({-1,1});
	int madep = -1;
	while(!qu.empty()){
		auto [dep,num] = qu.front();
		qu.pop();
		madep = max(madep,dep);
		for(int i = 1;i <= num;i++){
			if(a){
				qu.push({dep + 1,2});
				a--;
			}else if(b){
				qu.push({dep + 1,1});
				b--;
			}else{
				qu.push({dep + 1,0});
				c--;
			}
		}
	}
	cout << madep << '\n';
}

int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [G. Shuffling Songs](https://codeforces.com/contest/1950/problem/G)

* 每个歌曲有流派和作者，我们需要让相邻的歌曲有相同的流派或者相同的作者（或者两个都相同），问最少删除的歌曲数量
* 歌曲数量小于等于 **16**

##### 思路：

* 首先，比较字符串复杂度较高，我们先离散化，用 $map$ 什么的实现即可
* 然后我们将相同流派与相同作者当作这两个歌曲中间有路径双向边
* 那么这题就变成给一张图，从一个点开始走，不能经过相同的点，问最多能经过的点的数量
*  与 $2023PTA$ 校赛，逛遍CUGB很像，思路也基本相同，用二进制来表示走过的点，然后加一个现在在那个点的 $tag$ ，当然本题记忆化搜索也是可以的，因为我们不要求路径最短
* 最后统计最多能经过的点即可
* 记忆化搜索版

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

const int N = 17;

int countbit(int x){
	int num = 0;
	while(x){
		if(x&1) num++;
		x >>= 1;
	}
	return num;
}

map<string,int> mp;

void test(){
	int n;cin >> n;
	
	vector<vector<bool>> vis(1<<n,vector<bool>(n+1,0));
	vector<pair<int,int>> pa(n + 1);
	
	mp.clear();
	int ptop = 0;

	for(int i = 1;i <= n;i++){
		string s;cin >> s;
		if(mp[s] == 0) mp[s] = ++ptop;
		pa[i].first = mp[s];
		cin >> s;
		if(mp[s] == 0) mp[s] = ++ptop;
		pa[i].second = mp[s];
	}

	function<void(int,int)> dfs = [&](int bt,int x){
		if(vis[bt][x]) return;
		vis[bt][x] = 1;
		for(int i = 1;i <= n;i++){
			if(i != x && ((bt & (1 << (i - 1))) == 0) && ((pa[x].first == pa[i].first) || (pa[x].second == pa[i].second))){
				dfs(bt|(1 << (i - 1)),i);
			}
		}
	};
	for(int i = 1;i <= n;i++){
		dfs(1 << (i - 1),i);
	}

	int ans = 1;
	for(int i = 1;i < (1 << n);i++){
		bool f = 0;
		for(int j = 1;j <= n;j++){
			if(vis[i][j]){
				f = 1;
				break;
			}
		}
		if(f)
			ans = max(ans,countbit(i));
	}
	cout << n - ans << '\n';
}

int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

* 直接枚举二进制版

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
 
const int N = 17;
 
int countbit(int x){
	int num = 0;
	while(x){
		if(x&1) num++;
		x >>= 1;
	}
	return num;
}
 
map<string,int> mp;
 
void test(){
	int n;cin >> n;
	
	vector<vector<bool>> vis(1<<n,vector<bool>(n+1,0));
	vector<pair<int,int>> pa(n + 1);
	
	mp.clear();
	int ptop = 0;
 
	for(int i = 1;i <= n;i++){
		string s;cin >> s;
		if(mp[s] == 0) mp[s] = ++ptop;
		pa[i].first = mp[s];
		cin >> s;
		if(mp[s] == 0) mp[s] = ++ptop;
		pa[i].second = mp[s];
	}
 
	for(int i = 1;i <= n;i++)
		vis[(1 << (i - 1))][i] = 1;
 
	for(int i = 1;i < (1 << n);i++){
		for(int j = 1;j <= n;j++){
			if(vis[i][j]){
				for(int k = 1;k <= n;k++){
					if(((i & (1 << (k - 1))) == 0) && ((pa[j].first == pa[k].first) || (pa[j].second == pa[k].second)))
						vis[i | (1 << (k - 1))][k] = 1;
				}
			}
		}
	}
 
	int ans = 1;
	for(int i = 1;i < (1 << n);i++){
		bool f = 0;
		for(int j = 1;j <= n;j++){
			if(vis[i][j]){
				f = 1;
				break;
			}
		}
		if(f)
			ans = max(ans,countbit(i));
	}
	cout << n - ans << '\n';
}
 
int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

* 注意我们必须要判断新的点是否走过，不然会走回去导致 $wa$

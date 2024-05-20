## [**A - Exponential Plant**](https://atcoder.jp/contests/abc354/tasks/abc354_a)

* 问 $\sum_{i=0}^n2^i>H$ ， $n$ 最小取多少
* 暴力累加，或者 $\sum_{i=0}^n2^i=2^{n+1}-1>H$ ，都行

## [**B - AtCoder Janken 2**](https://atcoder.jp/contests/abc354/tasks/abc354_b)

* 每个用户有词序与分数，词序从 $0\to N-1$ ，将词按字典序排词序，问分数之和模 $N$ 的词序的词为什么
* 模拟

``` cpp
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
	vector<string> s(n);
	ll sum = 0;
	for(int i = 0;i < n;i++){
		cin >> s[i];
		int x;cin >> x;
		sum += x;
	}
	sort(s.begin(),s.end());
	sum %= n;
	cout << s[sum] << '\n';
}

int main(){
	IOS
	int t = 1;//cin >> t;
	while(t--){
		test();
	}
}
```

## [**C - AtCoder Magics**](https://atcoder.jp/contests/abc354/tasks/abc354_c)

*  $N$ 张牌，每张牌有强度 $A_i$ ，与成本 $C_i$
* 我们将那些相对来说肯定差的牌去除
* 也就是说如果有两张牌有 $A_i > A_j\  AND\  C_i < C_j$ ，那么牌 $j$ 就会被去除
* 问最后牌组中剩下的牌是什么

##### 思路：

* 首先我们假设所有牌的强度都不同，我们将牌按强度排序
* 然后按强度从大到小遍历，那么对于后面遍历的牌来说，前面的牌的强度都比它大，那么只要考虑成本即可，因此我们记录遍历过来的最小成本，后面遍历的牌只能让成本小于等于它
* 在考虑强度相同的情况，我们将强度相同的牌统一处理，同一记录强度即可
* 或者我们当强度相同时，先遍历成本较大的也可以，那么强度相同，但成本不同的也不会被去除了

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
	vector<tuple<int,int,int>> ve(n);
	for(int i = 0;i < n;i++){
		auto &[x,y,z] = ve[i];
		cin >> x >> y;
		z = i;
	}
	sort(ve.begin(),ve.end());
	int mi = inf;
	vector<int> ans;
	for(int i = n - 1;i >= 0;i--){
		auto [x,y,z] = ve[i];
		int mii = mi;

		while(i >= 0){
			auto [_x,_y,_z] = ve[i];
			if(_x != x) break;
			if(_y <= mi) ans.push_back(_z + 1);
			mii = min(mii,_y);
			i--;
		}

		mi = mii;
		i++;
	}
	cout << ans.size() << '\n';
	sort(ans.begin(),ans.end());
	for(auto x : ans){
		cout << x << ' ';
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

## [**D - AtCoder Wallpaper**](https://atcoder.jp/contests/abc354/tasks/abc354_d)

* 看图，难度不大，但是要考虑很多点
* 首先，无论在哪里，我们横着走 $4$ 格子，黑色的面积肯定是 $2$
* 那么我们就可以将横坐标的差减少到小于 $4$
* 然后我们再枚举横坐标，根据横坐标的情况来看纵坐标的循环情况

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

vector<int> tem[4] = {{3,2,1},{3,1,2},{1,0,1},{1,1,0}};

ll f(int op,int y){
	ll res = 0;

	res = tem[op][0] * (y / 2);
	
	if(y < 0)
		res += tem[op][2] * (y % 2);
	else
		res += tem[op][1] * (y % 2);

	return res;
}

void test(){
	int a,b,c,d;cin >> a >> b >> c >> d;
	ll tem = d - b;
	ll ans = (c - a) / 4;
	a = (c - a) / 4 * 4 + a;

	ans *= tem * 4;

	while(a < c){
		ans += f((a % 4 + 4) % 4,d) - f(((a % 4) + 4) % 4,b);
		a++;
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

## [**E - Remove Pairs**](https://atcoder.jp/contests/abc354/tasks/abc354_e)

* 桌子上有 $n$ 张牌，每张牌有正面与背面的数字
* 每次操作可以去掉两张正面数字相同的牌，或者背面数字相同的牌
* 问先手胜利还是后手胜利

##### 思路：

* 牌的数量 $n$ 小于等于 $18$ ，那么很容易想到用二进制来存储所有状态
* 我们可以先处理出所有牌可以同时去除的牌
* 状态的转移就是去掉两张牌，也就是二进制两位变为 $0$
* 用记忆化搜索来优化，每个状态遍历一次，复杂度就是 $O(2^{n})$
* 只有可能转移到输的状态，该状态才能胜利

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
const int N = 3e5 + 10;

bool f[N],ans[N];

vector<int> ed[20];
pair<int,int> pa[20];

int n;

bool check(int x){
	bool ok = 0;
	if(f[x]) return ans[x];
	f[x] = 1;
	for(int j = 0;j < n && !ok;j++){
		if(x & (1 << j)){
			for(auto y : ed[j]){
				if(x & (1 << y)){
					ok |= !check(x - (1 << j) - (1 << y));
				}
			}
		}
	}
	return ans[x] = ok;
}

void test(){
	cin >> n;
	for(int i = 1;i <= n;i++) cin >> pa[i].first >> pa[i].second;
	for(int i = 1;i <= n;i++){
		for(int j = i + 1;j <= n;j++){
			if(pa[i].first == pa[j].first || pa[i].second == pa[j].second)
				ed[i - 1].push_back(j - 1);
		}
	}
	if(check((1 << n) - 1))
		cout << "Takahashi\n";
	else
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

## [**F - Useless for LIS**](https://atcoder.jp/contests/abc354/tasks/abc354_f)

* 给出一个序列 $A$ ，问每个位置的数，是否可以是最长递增子序列的一员

##### 思路：

* 首先，我们可以用二分 $LIS$ 的做法先得出所有位置的数作为最长递增子序列的末端，最远可以到的位置
* 然后我们再反向搜索，将可能是最长递增子序列的数记录
* 首先我们将最远可以到的位置就是整个数组最长递增子序列的长度的记录
* 然后对于最远可以到的位置不是数组最长递增子序列的长度的数，我们要考虑后面的数，并且最远可以到的位置是要考虑的数的最远可以到的位置+1，并且已经确定是可能是最长递增子序列其中一员的数，如果那个数大于我们正在考虑的数，那么这个数就可以接再后面那个数的前面
* 因此我们就维护最远可以到的位置并且确定可能是最长递增子序列其中一员的最大值即可

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
	vector<int> a(n + 1),dp(n + 1);
	vector<int> tem;

	vector<int> ok(n + 1,-1);

	int len = 0;
	for(int i = 1;i <= n;i++){
		cin >> a[i];
		if(tem.empty()){
			dp[i] = 1;
			len = 1;
			tem.push_back(a[i]);
		}else{
			auto it = lower_bound(tem.begin(),tem.end(),a[i]);
			if(it == tem.end()){
				len++;
				dp[i] = len;
				tem.push_back(a[i]);
			}else{
				dp[i] = it - tem.begin() + 1;
				tem[dp[i] - 1] = a[i];
			}
		}
	}

	vector<int> ans;

	for(int i = n;i >= 1;i--){
		if(dp[i] == len){
			ok[dp[i]] = max(ok[dp[i]],a[i]);
			ans.push_back(i);
		}
		else{
			if(ok[dp[i] + 1] > a[i]){
				ok[dp[i]] = max(ok[dp[i]],a[i]);
				ans.push_back(i);
			}
		}
	}
	cout << ans.size() << '\n';
	reverse(ans.begin(),ans.end());
	for(int x : ans){
		cout << x << ' ';
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


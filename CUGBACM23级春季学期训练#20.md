## [**A - The bottom of the ninth**](https://atcoder.jp/contests/abc351/tasks/abc351_a?lang=en)

* 输出 $\sum a - \sum b + 1$

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
	int ans = 0;
	for(int i = 1;i <= 9;i++){
		int x;cin >> x;
		ans += x;
	}
	for(int i = 1;i <= 8;i++){
		int x;cin >> x;
		ans -= x;
	}
	cout << ans + 1 << '\n';
}

int main(){
	IOS
	int t = 1;//cin >> t;
	while(t--){
		test();
	}
}
```

## [**B - Spot the Difference**](https://atcoder.jp/contests/abc351/tasks/abc351_b?lang=en)

* 输出 $a$ 矩阵与 $b$ 矩阵字母不同的位置

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
const int N = 1e2 + 10;

char c[N][N];

void test(){
	int n;cin >> n;
	for(int i = 1;i <= n;i++)
		for(int j = 1;j <= n;j++)
			cin >> c[i][j];
	pair<int,int> ans;
	for(int i = 1;i <= n;i++)
		for(int j = 1;j <= n;j++){
			char tem;cin >> tem;
			if(tem != c[i][j]){
				ans = {i,j};
			}
		}
	cout << ans.first << ' ' << ans.second;
}

int main(){
	IOS
	int t = 1;//cin >> t;
	while(t--){
		test();
	}
}
```

## [**C - Merge the balls**](https://atcoder.jp/contests/abc351/tasks/abc351_c?lang=en)

* 每次向着序列最后（最右边）放一个球然后执行下列操作
* 1.如果只有一个球，停止操作
* 2.如果最右边的球和最右边第二个球大小不同，停止操作
* 3.如果相同，合成，大小为原来的两倍
* 问最后还剩多少球
* 给出的球的大小为 $2^x$ 

##### 思路：

* 用栈模拟即可

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
	stack<int> st;
	int x;cin >> x;
	st.push(x);
	for(int i = 2;i <= n;i++){
		cin >> x;
		while(!st.empty() && st.top() == x){
			x++;
			st.pop();
		}
		st.push(x);
	}
	cout << (int)st.size() << '\n';
}

int main(){
	IOS
	int t = 1;//cin >> t;
	while(t--){
		test();
	}
}
```

## [**D - Grid and Magnet**](https://atcoder.jp/contests/abc351/tasks/abc351_d?lang=en)

* 给一个 $H*W$ 的网格矩阵， $' \\# '$ 代表磁铁， $'.'$ 代表没有磁铁
* 你可以从任意位置开始移动，如果移动到的位置周围四个有磁铁，那么你就不能继续移动
* 问你从某个位置开始移动，能移动到的位置可能性最多是多少

##### 思路：

* 对于相邻的，没有磁铁在周围的位置来说，它们的可移动的可能性是相同的
* 那么我们广度搜索访问过这种没有磁铁在周围的位置就可以不用再次搜索
* 我们再每次搜索后取消掉有磁铁在周围的位置的访问标签即可

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

int mvx[] = {0,0,-1,1},mvy[] = {1,-1,0,0};

void test(){
	int h,w;cin >> h >> w;
	vector<string> mp(h+1);
	vector<vector<bool>> vis(h+1,vector<bool>(w+1,0));
	for(int i = 1;i <= h;i++){
		cin >> mp[i];
		mp[i] = ' ' + mp[i];
	}
	int ans = 0;
	for(int i = 1;i <= h;i++){
		for(int j = 1;j <= w;j++){
			if(!vis[i][j] && mp[i][j] == '.'){
				ans = max(ans,1);
				if(i == 1 || mp[i - 1][j] == '.')
				if(j == 1 || mp[i][j - 1] == '.')
				if(i == h || mp[i + 1][j] == '.')
				if(j == w || mp[i][j + 1] == '.'){
					int tem = 0;
					queue<pair<int,int>> qu;
					queue<pair<int,int>> qqu;
					qu.push({i,j});
					vis[i][j] = 1;
					while(!qu.empty()){
						auto [x,y] = qu.front();
						qu.pop();
						tem++;
						bool f = 0;
						if(x == 1 || mp[x - 1][y] == '.')
						if(y == 1 || mp[x][y - 1] == '.')
						if(x == h || mp[x + 1][y] == '.')
						if(y == w || mp[x][y + 1] == '.')
							f = 1;
						if(f){
							for(int k = 0;k < 4;k++){
								int _x = x + mvx[k],_y = y + mvy[k];
								if(_x >= 1 && _x <= h && _y >= 1 && _y <= w && !vis[_x][_y]){
									vis[_x][_y] = 1;
									qu.push({_x,_y});
								}
							}
						}else{
							qqu.push({x,y});
						}
					}
					ans = max(ans,tem);
					while(!qqu.empty()){
						auto [x,y] = qqu.front();
						qqu.pop();
						vis[x][y] = 0;
					}
				}
			}
		}
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

## [**E - Jump Distance Sum**](https://atcoder.jp/contests/abc351/tasks/abc351_e?lang=en)

* $N$ 个点 $P_1,P_2,..P_N$ ，其中点 $P_i$ 的坐标为 $(X_i,Y_i)$
* 两个点的距离 $dist(A,B)$ 定义如下

> 从 $A$ 点开始
>
> 在 $(x,y)$ 位置可以移动到 $(x+1,y+1),(x+1,y-1),(x-1,y+1),(x-1,y-1)$
>
>  $dist(A,B)$ 指从 $A$ 点到 $B$ 点最少的移动次数
>
> 如果不能到达，则 $dist(A,B)=0$

##### 思路：

* 如果原本 $x,y$ 两个数的奇偶性相同，那么移动后两个数的奇偶性肯定还是相同的，因此只有 $x,y$ 奇偶性相同的可移动到同样 $x,y$ 奇偶性相同的点
* 同样只有 $x,y$ 奇偶性不同的点可以移动到 $x,y$ 奇偶性不同的点
* 对于同类的点，问题转化为

* 求所有点对的切比雪夫距离之和
* 即求 $\displaystyle\sum_{i=1}^{N-1}\displaystyle\sum_{j=i+1}^N \max(|x_i-x_j|,|y_i-y_j|)$
* 当然这个我们肯定不能直接求出来，我们通过切比雪夫距离与曼哈顿距离的转化，数学知识可得
* 我们假设曼哈顿距离 $D=|x_2-x_1|+|y_2-y_1|$
* 那么有 $D=max(x_2-x_1+y_2-y_1,x_1-x_2+y_2-y_1,x_2-x_1+y_1-y_2,x_1-x_2+y_1-y_2)$
* 也就是 $D=max(|(x_2+y_2)-(x_1+y_1)|,|(x_2-y_2)-(x_1-y_1)|)$
* 我们设 $x_2+y_2=x_i,x_1+y_1=x_j,x_2-y_2=y_i,x_1-y_1=y_j$
* 那么有 $x_1=\frac{x_i+y_i}{2},y_1=\frac{x_i-y_i}{2},x_2=\frac{x_j+y_j}{2},y_2=\frac{x_j-y_j}{2}$
* 因此我们将所有点转化为 $(\frac{x+y}{2},\frac{x-y}{2})$ ，然后求所有点对的曼哈顿距离即可
* 求所有点对的曼哈顿距离分开用前缀和求什么的都可以

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

ll get_sum(vector<ll>& a){
	sort(a.begin(),a.end());
	int len = (int)a.size();
	vector<ll> sum(len,0);
	for(int i = 0;i < len;i++){
		if(i == 0){
			sum[i] = a[i];
			continue;
		}
		sum[i] = sum[i - 1] + a[i];
	}
	ll res = 0;
	for(int i = 0;i < len;i++){
		res += sum[len-1] - sum[i] - a[i] * (len - i - 1);
	}
	return res;
}

void test(){
	int n;cin >> n;
	vector<ll> same_x;
	vector<ll> same_y;
	vector<ll> uns_x;
	vector<ll> uns_y;
	for(int i = 1;i <= n;i++){
		int x,y;cin >> x >> y;
		if(x%2==y%2){
			same_x.push_back(x+y);
			same_y.push_back(x-y);
		}else{
			uns_x.push_back(x+y);
			uns_y.push_back(x-y);
		}
	}
	ll ans = 0;
	ans += get_sum(same_x);
	ans += get_sum(same_y);
	ans += get_sum(uns_x);
	ans += get_sum(uns_y);
	cout << ans / 2 << '\n';
}

int main(){
	IOS
	int t = 1;//cin >> t;
	while(t--){
		test();
	}
}
```

## [P3964 [TJOI2013] 松鼠聚会](https://www.luogu.com.cn/problem/P3964)

* 思路同上，不过是求一个点的最小和

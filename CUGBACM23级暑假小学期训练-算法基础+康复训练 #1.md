## [A - 线性筛素数](https://vjudge.net.cn/problem/洛谷-P3383)

* 板子

 ```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 1e8 + 10,M = 1e7 + 10;

bool vis[N];
int ans[M];
int idx;

void f(){
	for(int i = 2;i <= N - 10;i++){
		if(!vis[i])
			ans[++idx] = i;
		for(int j = 1;j <= idx && ans[j]*i < N - 10;j++){
			vis[ans[j]*i] = 1;
			if(i % ans[j] == 0)
				break;
		}
	}
}

int main(){
	ios::sync_with_stdio(false);//写了using namespace std;
	int n,q;cin >> n >> q;
	f();
	while(q--){
		int x;cin >> x;
		cout << ans[x] << endl;
	}
}
 ```

## [B - 国王游戏](https://vjudge.net.cn/problem/洛谷-P1080)

* 抛开高精度，只谈贪心思路
* 只看两个相邻位置的人
* 交换前 $i->a_{i-1}*b_i+sum_{i-2}*b_i,i-1->sum_{i-2}*b_{i-1}$
* 交换后 $i->a_{i}*b_{i-1}+sum_{i-2}*b_{i-1},i-1->sum_{i-2}*b_{i}$
* 那么就是看 $a_{i-1}b_i与a_ib_{i-1}$ 取小的方案即可

```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long
const int N = 1100;
pair<int,int> a[N];
vector<int> tem,sum,ma;
void next()//高精度进位
{
	int i = 0;
	while(i < sum.size())
	{
		if(sum[i] >= 10)
			if(i == sum.size() - 1)
			{
				sum.push_back(sum[i] / 10);
				sum[i] %= 10;
			}
			else
			{
				sum[i + 1] += sum[i] / 10;
				sum[i] %= 10;
			}
		i++;
	}
}
void  mult(int i)//高精度乘法
{
	for(int j = 0;j < sum.size();j++)
	sum[j] *= i;
	next();
}
void divi(int i)//高精度除法
{
	int tt = 0;//记录数据
	for(int j = tem.size() - 1;j >= 0;j--)//从前往后
	{
		tt *= 10;
		tt += tem[j];
		tem[j] = tt / i;
		tt %= i;
	}
	for(int j = tem.size() - 1;j >= 1;j--)//去0
	if(tem[j] == 0)
	tem.pop_back();
	else
	break;
	if(ma.size() > tem.size());//选最大值
		else if(ma.size() == tem.size())
		for(int i = ma.size() - 1;i >= 0;i--)
		{
			if(tem[i] > ma[i])
			{
				ma = tem;break;
			}
			else if(ma[i] < tem[i])
			break;
		}
		else
		ma = tem;
}
bool cmp(pair<int,int> aa,pair<int,int> bb){
	return aa.first*aa.second < bb.second*bb.first;
}
void test(){
	int n;cin >> n;
	int x = 1,te;
	cin >> x >> te;
	sum.push_back(x);
	for(int i =1;i <= n;i++){
		cin >> a[i].first >> a[i].second;
	}
	sort(a + 1,a + 1 + n,cmp);
	for(int i = 1;i <= n;i++){
		tem = sum;
		divi(a[i].second);
		mult(a[i].first);
	}
	int nn = ma.size();
	for(int i = nn - 1;i >= 0;i--){
		cout << ma[i];
	}
	if(nn == 0)
		cout << 0 << endl;
}
#undef int
int main()
{
	int t = 1;//cin >> t;
	while(t--){
		test();
	}
}
```

## [C - Winnie-the-Pooh and honey](https://vjudge.net.cn/problem/CodeForces-120C)

* 如果吃满三次，那么剩下的就是 $a[i]-3k$
* 如果没吃满三次，那么剩下的就是 $a[i] \%k$
* 取最大的

```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long
const int N = 111;
int a[N];
void test(){
	freopen("input.txt","r",stdin);
	freopen("output.txt","w",stdout);
	int n,k;cin >> n >> k;
	int ans = 0;
	for(int i = 1;i <= n;i++){
		cin >> a[i];
		ans += max(a[i] % k,a[i] - k*3);
	}
	cout << ans << endl;
}
#undef int
int main(){
	test();
}
```

## [D - Basil's Garden](https://vjudge.net.cn/problem/CodeForces-1987C)

* 贪心
* 首先，每个位置要降到 $0$ ，前提必要条件就是前面的变成 $0$
* 我们假设后面的位置变成 $0$ 的时间为 $t$
* 如果该位置经过时间 $t$ 减到 $1$ ，那么它在整个时间 $t$ 内都是一直减少的
* 因为后面的位置可以变成 $0$ ，说明后面位置无论何时都比它小
* 然后时间加上剩下的高度就是现在这个位置变成 $0$ 的时间了

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
	vector<ll> h(n + 1);
	for(int i = 1;i <= n;i++) cin >> h[i];
	ll ans = 0;
	for(int i = n;i >= 1;i--){
		if(i == n){
			ans = h[i];
			h[i] = 0;
		}else{
			h[i] -= min(h[i] - 1,ans);
			ans += h[i];
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

## [E - 数列分段 Section II](https://vjudge.net.cn/problem/洛谷-P1182)

* 我们假设最大值为 $x$
* 那么第一段区间肯定是从 $[1,l]$ ，从 $1$ 开始
* 我们 $l$ 取的越远，剩下的数越少，那么剩下区间的数量肯定也可以越少
* 因此我们贪心的取区间，此时取到的区间数量就是这个 $x$ 对应的最小区间数量
* 我们再二分这个 $x$ 即可

```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long
const int N = 1e5 + 10;
int n,m;
int a[N];
bool pd(int x){
	int no = 0,num = 1;
	for(int i = 1;i <= n;i++){
		if(no + a[i] > x){
			num++;
			no = a[i];
			if(a[i] > x)
				return 0;
		}else{
			no += a[i];
		}
	}
	return num <= m;
}
void test(){
	cin >> n >> m;
	for(int i = 1;i <= n;i++)
		cin >> a[i];
	int l = 1,r = 1e9;
	while(l <= r){
		int mid = (l + r) / 2;
		if(pd(mid)){
			r = mid - 1;
		}else
			l = mid + 1;
	}
	cout << l << endl;
}
signed main()
{
	int t = 1;//cin >> t;
	while(t--){
		test();
	}
}
```

## [F - 三分 | 函数](https://vjudge.net.cn/problem/洛谷-P1883)

* 板子

## [G - 分组](https://vjudge.net.cn/problem/洛谷-P4447)

* 贪心加入
* 给所有数排序，然后如果出现一个新数或者这个数不能加入然后已经存在的序列中了，那么创建新的序列

```cpp
#include<bits/stdc++.h>
using namespace std;
const int MAX = 110000;
int a[MAX];
pair<int,int> se[MAX];//记录数目和最大值 
int main()
{
	int n;
	cin >> n;
	for(int i = 1;i <= n;i++)
	scanf("%d",a + i);
	sort(a + 1,a + n + 1);
	int prebegin = 1,preend = 1,begin = 1,end = 1,pre,now = a[begin],p;//记录前一个元素的开始集合，结束集合，现在元素的开始集合，结束集合，前一个元素的值，现在的值
	while(a[end] == now)//给第一个元素创造集合 
	se[end++] = {1,now};
	prebegin = begin;preend = end - 1;pre = now;begin = end;p = end;
	while(p <= n)//指针还没到结尾 
	{
		now = a[p];//现在的值
		if(now == pre + 1) //如果现在的值为前一个值的+1，那么开始向前面的集合加入
		{
			while(preend >= prebegin && a[p] == now && p <= n)
			{
				p++;
				se[preend].first++;
				se[preend--].second = now;
			}
		}
		while(a[p] == now && p <= n)//不能加入前面的集合就新建集合 
		{
			p++;
			se[end++] = {1,now};
		}
		prebegin = preend + 1;
		preend = end - 1;
		pre = now;
	}
	int min = INT_MAX;
	for(int i = 1;i < end;i++)
	min = min > se[i].first?se[i].first : min;
	printf("%d",min);
}
```

## [H - Tile Distance 2](https://vjudge.net.cn/problem/AtCoder-abc359_c)

* 如果所给位置只在砖块的左下角，那么这题就好做很多
* 那么我们就选将位置转移到砖块的左下角

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
	ll sx,sy,tx,ty;cin >> sx >> sy >> tx >> ty;
	if((sx + sy) % 2 == 1) sx--;
	if((tx + ty) % 2 == 1) tx--;
	ll dis = abs(sy - ty);
	if(sx < tx) sx += min(dis,tx - sx);
	else sx -= min(dis,sx - tx);
	cout << dis + abs(sx - tx) / 2;
}

int main(){
	IOS
	int t = 1;//cin >> t;
	while(t--){
		test();
	}
}
```

## [I - Avoid K Palindrome](https://vjudge.net.cn/problem/AtCoder-abc359_d)

*  $k$ 的值很小
* 那么我们记录前 $k-1$ 个字母，那么我们就可以考虑新增加的字母不让前面的 $k-1$ 个字母拼接之后为回文
* 字母只有 $AB$ ，我们将其看作 $01$
* 转化为 $状压dp$ 也可以
* 复杂度 $O(nk2^k)$

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
const int mod = 998244353;

int k;

bool f(const string& ss){
	if(ss.size() < k) return 0;
	bool ff = 1;
	for(int i = 0;i < k / 2;i++){
		if(ss[i] != ss[k - i - 1]) ff = 0;
	}
	return ff;
}

void test(){
	int n;cin >> n >> k;
	string s;cin >> s;
	s = ' ' + s;
	map<string,ll> mp;
	for(int i = 1;i <= n;i++){
		map<string,ll> mmp;
		if(i == 1){
			if(s[i] == '?'){
				mmp["A"] = 1;
				mmp["B"] = 1;
			}else{
				string ss;
				ss.push_back(s[i]);
				mmp[ss] = 1;
			}
		}else if(i <= k){
			for(auto pa : mp){
				if(s[i] == '?'){
					string ss = pa.first + "A";
					if(!f(ss)) mmp[ss] = (mmp[ss] + pa.second) % mod;
					ss = pa.first + "B";
					if(!f(ss)) mmp[ss] = (mmp[ss] + pa.second) % mod;
				}
				else{
					string ss = pa.first + s[i];
					if(!f(ss)) mmp[ss] = (mmp[ss] + pa.second) % mod;
				}
			}
		}else{
			for(auto pa : mp){
				if(s[i] == '?'){
					string ss = pa.first.substr(1,k-1) + "A";
					if(!f(ss)) mmp[ss] = (mmp[ss] + pa.second) % mod;
					ss = pa.first.substr(1,k-1) + "B";
					if(!f(ss)) mmp[ss] = (mmp[ss] + pa.second) % mod;
				}
				else{
					string ss = pa.first.substr(1,k-1) + s[i];
					if(!f(ss)) mmp[ss] = (mmp[ss] + pa.second) % mod;
				}
			}
		}
		mp = mmp;
	}
	ll ans = 0;
	for(auto pa : mp){
		ans += pa.second;
		ans %= mod;
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

## [J - ABA and BAB](https://vjudge.net.cn/problem/AtCoder-arc180_a)

* 实际上，我们发现如果有两个或者以上的相同字母相连，那么就会将这个字符串断开，也就是左边的变化不会影响到右边
* 而字母不同的位置，我们可以进行的操作次数是可变的，可能性就是可能的次数
* 我们只要将所有断开直接的可能性相乘即能得到答案

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
const int mod = 1e9 + 7;

void test(){
	int n;cin >> n;
	string s;cin >> s;
	ll ans = 1,len = 1;
	for(int i = 0;i < n;i++){
		if(i == 0 || s[i] == s[i - 1]){
			ans *= (len + 2) / 2;
			ans %= mod;
			len = 0;
		}else{
			len++;
		}
	}
	ans *= (len + 2) / 2;
	ans %= mod;
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

## [K - Water Tank](https://vjudge.net.cn/problem/AtCoder-abc359_e)

* 单调栈
* 我们想要知道一个位置开始有水，那么就要知道前面一个位置满水的时间，再加 $1$
* 如果一个位置满水，那么前面要找到第一个比它高或者相同高度的位置来蓄水
* 因此一个位置满水可以表示为 $ans[i]=ans[pre]+(i-pre)*h[i]$
* 那么这个 $pre$ 就可以用单调栈来寻找

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

ll ans[N];

void test(){
	int n;cin >> n;
	stack<pair<int,int>> st;
	st.push({0,inf});
	for(int i = 1;i <= n;i++){
		int h;cin >> h;
		while(st.top().second < h){
			st.pop();
		}
		ans[i] = (ll)h*(i - st.top().first) + ans[st.top().first];
		cout << ans[i] + 1 << ' ';
		st.push({i,h});
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

## [L - Tree Degree Optimization](https://vjudge.net.cn/problem/AtCoder-abc359_f)

* 树的性质 $m=n-1$
* 也就是说度数为 $2m=2n-2$
* 同时，树上的点度数最小为 $1$
* 满足上面的条件，肯定能形成一颗树
* 因此 $\sum_{i=1}^nd_i=2n-2$
* 然后我们考虑，先给每个点都来一个度
* 然后考虑每个点如果增加一度，其代价应该为 $((d_i+1)^2-d_i^2)a_i$
* 我们取 $n-2$ 次这个最小值即可

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

struct pos{
	ll a,d;
	bool operator<(const pos &other)const{
		return (other.d + 1)*(other.d + 1)*other.a - (other.d)*(other.d)*other.a < (d + 1)*(d + 1)*a-d*d*a;
	}
}po[N];
priority_queue<pos> pqu;

void test(){
	int n;cin >> n;
	ll ans = 0;
	for(int i = 1;i <= n;i++){
		cin >> po[i].a;
		po[i].d = 1;
		ans += po[i].a;
		pqu.push(po[i]);
	}
	for(int i = 1;i <= n - 2;i++){
		pos tem = pqu.top();
		pqu.pop();
		ans -= tem.d*tem.d*tem.a;
		tem.d++;
		ans += tem.d*tem.d*tem.a;
		pqu.push(tem);
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

## [M - X Axis](https://vjudge.net.cn/problem/CodeForces-1986A)

* 水

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
	vector<int> ve(3);
	for(int i = 0;i < 3;i++) cin >> ve[i];
	sort(ve.begin(),ve.end());
	cout << ve[2] - ve[0] << '\n';
}

int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [N - Matrix Stabilization](https://vjudge.net.cn/problem/CodeForces-1986B)

* 我们从小到大考虑，因为一个数只会被那些更小的数更新

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

struct pos{
	int i,j,x;
};
bool cmp(pos a,pos b){
	return a.x < b.x;
}

void test(){
	int n,m;cin >> n >> m;
	vector<vector<int>> a(n + 1,vector<int>(m + 1));
	vector<pos> ve(n*m + 1);
	for(int i = 1;i <= n;i++)
		for(int j = 1;j <= m;j++){
			cin >> a[i][j];
			ve[i*m - m + j].x = a[i][j];
			ve[i*m - m + j].i = i;
			ve[i*m - m + j].j = j;
		}
	sort(ve.begin() + 1,ve.end(),cmp);
	for(int i = 1;i <= n*m;i++){
		int ma = 0;
		int _i = ve[i].i,_j = ve[i].j;
		if(_i != 1) ma = max(a[_i - 1][_j],ma);
		if(_i != n) ma = max(a[_i + 1][_j],ma);
		if(_j != 1) ma = max(a[_i][_j - 1],ma);
		if(_j != m) ma = max(a[_i][_j + 1],ma);
		a[_i][_j] = min(a[_i][_j],ma);
	}
	for(int i = 1;i <= n;i++){
		for(int j = 1;j <= m;j++)
			cout << a[i][j] << ' ';
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

## [O - Update Queries](https://vjudge.net.cn/problem/CodeForces-1986C)

* 每个位置最后替换时应尽量取最小的字母，为了让字典序最小，我们还应该让最前面的字母最后替换对应的字母最小

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
	string s;cin >> s;
	vector<int> ve;
	set<int> se;
	for(int i = 1;i <= m;i++){
		int x;cin >> x;
		ve.push_back(x);
		se.insert(x);
	}
	sort(ve.begin(),ve.end());
	string ss;cin >> ss;
	sort(ss.begin(),ss.end());
	int ptop = 0;
	for(auto x : se){
		s[x - 1] = ss[ptop++];
	}
	cout << s << '\n';
}

int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [P - Mathematical Problem](https://vjudge.net.cn/problem/CodeForces-1986D)

* 考虑 $0$ 与二位数的选择， $1$ 用乘法贡献为 $0$ 但是连续的 $1$ 总贡献还是 $1$

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
	int sum = 0;
	int mitem = inf;
	for(int i = 0;i < n - 1;i++){
		sum += s[i] - '0';
		if(s[i] - '0' == 1) sum--;
		int tem = (s[i] - '0') * 10 + s[i + 1] - '0';
		if(mitem > tem) swap(mitem,tem);
		if(mitem / 10 == tem / 10 && mitem % 10 == 1){
			mitem = tem;
		}
	}
	sum += s[n - 1] - '0';
	if(s[n - 1] - '0' == 1) sum--;

	if(n >= 4){
		for(auto c : s){
			if(c == '0'){
				cout << 0 << '\n';
				return;
			}
		}
	}else if(n == 3){
		if(s[0] == '0' || s[2] == '0'){
			cout << 0 << '\n';
			return;
		}
	}
	if(mitem % 10 == 1) sum++;
	if(mitem / 10 == 1) sum++;
	if(mitem == 1) sum--;
	if(n == 2 && mitem == 1) sum++;
	if(s == "101") sum++;
	cout << sum + mitem - mitem / 10 - mitem % 10 << '\n';
}

int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [Q - Beautiful Array](https://vjudge.net.cn/problem/CodeForces-1986E)

* 只有 $\%k$ 相同的数可以相互转移
* 我们按 $\%k$ 相同分开
* 如果是偶数，那么相邻计数就可以
* 如果是奇数，我们就要考虑奇数的数量
* 如果奇数超过 $1$ ，那肯定是不能形成回文串了
* 奇数的计数考虑我们将那个数放中间，其他数正常相邻计数
* 用前缀与后缀和即可

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
	vector<int> a(n + 1);
	map<int,vector<int>> mp;
	for(int i = 1;i <= n;i++){
		cin >> a[i];
		mp[a[i] % k].push_back(a[i]);
	}
	int num1 = 0;
	ll ans = 0;
	for(auto& pa : mp){
		num1 += pa.second.size() % 2;
		sort(pa.second.begin(),pa.second.end());
		int len = pa.second.size();
		if(len % 2 == 0){
			for(int i = 0;i < len;i += 2){
				ans += pa.second[i + 1] - pa.second[i];
			}
		}else{
			vector<ll> l(len + 10),r(len + 10);
			for(int i = 0;i + 1 < len;i += 2){
				l[i + 2] += pa.second[i + 1] - pa.second[i];
				if(i != 0) l[i + 2] += l[i];
			}
			for(int i = len - 1;i - 1 >= 0;i -= 2){
				r[i] += pa.second[i] - pa.second[i - 1];
				if(i != len - 1) r[i] += r[i + 2];
			}
			ll mi = INF;
			for(int i = 0;i - 1 < len;i += 2){
				mi = min(mi,l[i] + r[i + 2]);
			}
			ans += mi;
		}
	}
	if(num1 > 1) cout << -1 << '\n';
	else
		cout << ans / k << '\n';
}

int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

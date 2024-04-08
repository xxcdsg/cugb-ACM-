## [P4782 【模板】2-SAT](https://www.luogu.com.cn/problem/P4782)

* 对于二元条件并且，元素只有两个可能，如果非A，那么就可以推出B，同理非B就可以推出A，那么就可以建图，不可能成立的情况就是A与非A形成了一个环，用tarjan找环，然后tarjan给出的color就是拓扑逆序，那么为了防止A推得非A，那么只要输出color小的可能性就是拓扑后面的位置

```cpp
#include<bits/stdc++.h>
using namespace std;

const int N = 1e6 + 10;

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
		int y;
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
	cin >> n >> m;
	for(int k = 1;k <= m;k++){
		int i,a,j,b;cin >> i >> a >> j >> b;
		if(a) i = 2*n - i + 1;
		if(b) j = 2*n - j + 1;
		i = 2*n - i + 1;
		ed[i].push_back(j);
		i = 2*n - i + 1;
		j = 2*n - j + 1;
		ed[j].push_back(i);
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
	if(!f) cout << "IMPOSSIBLE" << '\n';
	else{
		cout << "POSSIBLE" << '\n';
		for(int i = 1;i <= n;i++)
			cout <<	(c[2*n - i + 1] < c[i]) << ' ';
	}
}
```

## [**F - Well-defined Path Queries on a Namori**](https://atcoder.jp/contests/abc266/tasks/abc266_f?lang=en)

* 基环树模板
* 简单无向图有 $n$ 个点 $n-1$ 条边，那么这就是一颗树
* $n$ 个点 $n$ 条边，相对多一条边，有且仅有一个环
* 可以利用拓扑排序找这个环

```cpp
#include<bits/stdc++.h>
using namespace std;
const int MAXN = 2e5 + 10,MAXM = 2e5 + 10;
int n,q;
struct edge{
	int v;
	edge* nex;
}ed[MAXM * 2];
edge* head[MAXN];
int ptop = 0;
void add(int u,int v)
{
	ed[ptop].v = v;
	ed[ptop].nex = head[u];
	head[u] = &ed[ptop];
	ptop++;
}
int du[MAXN];
bool unable[MAXN];
int co = 1;//颜色
int color[MAXN];
void tp()//拓扑排序,找环
{
	queue<int> qu;
	for(int i = 1;i <= n;i++)
		if(du[i] == 1)
			qu.push(i);
	while(!qu.empty())
	{
		int u = qu.front();qu.pop();
		unable[u] = 1;
		edge* p = head[u];
		while(p != NULL)
		{
			int v = p -> v;
			du[v]--;
			if(du[v] == 1)
				qu.push(v);
			p = p -> nex;
		}
	}
}
bool use[MAXN];
void dfs(int c,int p)//染色
{
	if(use[p] || !unable[p])
		return;
	use[p] = 1;
	color[p] = c;
	edge* pp = head[p];
	while(pp != NULL)
	{
		int v = pp -> v;
		dfs(c,v);
		pp = pp -> nex;
	}
}
int main()
{
	cin >> n;
	for(int i = 1;i <= n;i++)
	{
		int u,v;cin >> u >> v;
		du[u]++,du[v]++;
		add(u,v);
		add(v,u);
	}
	tp();
	for(int i = 1;i <= n;i++)
	{
		if(!unable[i])
		{
			color[i] = co;
			edge* p = head[i];
			while(p != NULL)
			{
				int v = p -> v;
				dfs(co,v);
				p = p -> nex;
			}
			co++;
		}
	}
	cin >> q;
	while(q--)
	{
		int u,v;cin >> u >> v;
		if(color[u] == color[v])
			cout << "Yes" << endl;
		else
			cout << "No" << endl;
	}
}
```

## [**B - Farthest Point**](https://atcoder.jp/contests/abc348/tasks/abc348_b)

* 找每个点最远点，如果有若干最远点取 $id$ 小的
* $n$ 较小，暴力找，浮点数要判断精度，因此我们用平方代替

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

ll dis(pair<int,int> p1,pair<int,int> p2){
	return abs(p1.first-p2.first)*abs(p1.first-p2.first) + abs(p1.second-p2.second)*abs(p1.second-p2.second);
}

void test(){
	int n;cin >> n;
	vector<pair<int,int>> pa(n);
	for(int i = 0;i < n;i++)
		cin >> pa[i].first >> pa[i].second;
	for(int i = 0;i < n;i++){
		ll ma = -1;
		int id = i;
		for(int j = 0;j < n;j++){
			if(i == j) continue;
			if(ma < dis(pa[i],pa[j])){
				ma = dis(pa[i],pa[j]);
				id = j;
			}
		}
		cout << id + 1 << '\n';
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

## [**C - Colorful Beans**](https://atcoder.jp/contests/abc348/tasks/abc348_c?lang=en)

* 给一堆豆子，每个豆子有颜色与美味度，选一个颜色使之最小美味度的豆子的美味度最大
* $map$ 存，然后遍历

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
	map<int,int> mp;
	for(int i = 1;i <= n;i++){
		int x,y;cin >> y >> x;
		if(!mp.count(x))
			mp[x] = y;
		else
			mp[x] = min(mp[x],y);
	}
	int ma = 0;
	for(auto [x,y] : mp){
		ma = max(ma,y);
	}
	cout << ma << '\n';
}

int main(){
	IOS
	int t = 1;//cin >> t;
	while(t--){
		test();
	}
}
```

## [**D - Medicines on Grid**](https://atcoder.jp/contests/abc348/tasks/abc348_d)

*  $H*W$ 的地图，图上有 $起点S$ ， $终点T$ ， $空.$ ， $障碍物\\#$ 
*  每向相邻的位置移动都需要一点能量
*  图上还有能将能量变成特定值的药物
*  起始能量为 $0$ ，问能否到达终点

##### 思路：

* 因为是药物将能量变成特定值，因此吃下药物后能到达的范围是不变的，因此无论是从哪里来，吃下药物前的能量为多少，吃下药物后能到达的范围都不变
* 那么我们就不需要每次访问到同一个药物位置都搜索，只要搜索一遍即可

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

int mvx[] = {1,0,-1,0};
int mvy[] = {0,1,0,-1};

void test(){
	int h,w;cin >> h >> w;
	vector<string> mp(h + 1);
	vector<vector<vector<int>>> p(h + 1,vector<vector<int>>(w + 1));
	for(int i = 1;i <= h;i++){
		cin >> mp[i];
		mp[i] = ' ' + mp[i];
	}
	int n;cin >> n;
	pair<int,int> root;
	for(int i = 1;i <= h;i++){
		for(int j = 1;j <= w;j++){
			if(mp[i][j] == 'S')
				root = {i,j};
			if(mp[i][j] == 'T')
				p[i][j].push_back(n + 1);
		}
	}
	vector<bool> vis(n + 2);//是否访问过
	vector<tuple<int,int,int>> pp(n + 2);
	for(int i = 1;i <= n;i++){
		auto &[r,c,e] = pp[i];
		cin >> r >> c >> e;
		p[r][c].push_back(i);
	}

	vector<vector<bool>> vvis(h + 1,vector<bool>(w + 1,false));
	queue<int> qu;
	function<void(int,int,int)> bfs = [&](int i,int j,int E){

		queue<tuple<int,int,int>> qqu;
		qqu.push({i,j,E});
		while(!qqu.empty()){
			auto [i,j,E] = qqu.front();
			qqu.pop();
			if(E < 0) continue;
			if(vvis[i][j]) continue;
			vvis[i][j] = 1;
			for(auto _x : p[i][j]){
				if(!vis[_x]){
					vis[_x] = 1;
					qu.push(_x);
				}
			}
			for(int k = 0;k < 4;k++){
				int _i = i + mvx[k],_j = j + mvy[k];
				if(_i <= 0 || _i > h || _j <= 0 || _j > w || mp[_i][_j] == '#') continue;
				qqu.push({_i,_j,E - 1});
			}
		}

	};
	bfs(root.first,root.second,0);
	while(!qu.empty()){
		vvis = vector<vector<bool>>(h + 1,vector<bool>(w + 1,false));
		int _x = qu.front();
		qu.pop();
		auto &[r,c,e] = pp[_x];
		bfs(r,c,e);
	}
	if(vis[n + 1])
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

## [**E - Minimize Sum of Distances**](https://atcoder.jp/contests/abc348/tasks/abc348_e)

* 给一颗树与序列 $C$

* $f(x)=\sum_{i=1}^n(C_i*d(x,i))$ ，其中 $d(x,y)$ 代表 $x,y$ 中有多少边

##### 思路：

* 换根：用于解决树上假设每个点均为根的问题

* 跑两遍dfs，第一遍假设一个节点为根，第二遍根据上一遍跑的尝试计算父节点的贡献并得出所有节点为根的情况

* 第二遍dfs一般思路为：如果为根节点，那么记录，否则将父节点纳入考虑中

* 在遍历所有子节点前记录现在的数据，要遍历那个子节点就取消该子节点的数据对该数据的影响再继续dfs，然后还原到原来的数据

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

ll c[N];

vector<int> ed[N];

ll sum[N];

ll ans[N];

ll mi = INF;

void dfs1(int x,int fa){
	sum[x] = c[x];
	ans[x] = 0;
	for(int y : ed[x]){
		if(fa == y) continue;
		dfs1(y,x);
		sum[x] += sum[y];
		ans[x] += ans[y] + sum[y];
	}
}

void dfs2(int x,int fa){
	if(x == 1) mi = ans[x];
	else{
		ans[x] += sum[fa] + ans[fa];
		sum[x] += sum[fa];
		if(ans[x] < mi)
			mi = ans[x];
	}
	ll temans = ans[x],temsum = sum[x];
	for(auto y : ed[x]){
		if(y == fa) continue;
		sum[x] -= sum[y];
		ans[x] -= sum[y] + ans[y];
		dfs2(y,x);
		sum[x] = temsum;
		ans[x] = temans;
	}
}

void test(){
	int n;cin >> n;
	for(int i = 1;i < n;i++){
		int u,v;cin >> u >> v;
		ed[u].push_back(v);
		ed[v].push_back(u);
	}
	for(int i = 1;i <= n;i++) cin >> c[i];
	dfs1(1,0);
	dfs2(1,0);
	cout << mi << '\n';
}

int main(){
	IOS
	int t = 1;//cin >> t;
	while(t--){
		test();
	}
}
```

# CUGBACM23级春季学期训练#3

## [迷宫城堡](https://acm.hdu.edu.cn/showproblem.php?pid=1269)

* tarjan算法可见https://oiwiki.org/graph/scc/
* 题目的问题就是给一个图，问整个图是否完全相互可通，也就是问这个图是否为单一强连通分量

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;

#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;

const int N = 1e4 + 10;

vector<int> ed[N];

int low[N],dfs[N];

stack<int> st;

int ptop = 0;

int sum = 0;

void Dfs(int x){
	low[x] = dfs[x] = ++ptop;
	st.push(x);
	for(auto y : ed[x]){
		if(dfs[y] == 0)
			Dfs(y);
		low[x] = min(low[x],low[y]);
	}
	if(low[x] == dfs[x]){
		sum++;
		while(x != st.top()){
			st.pop();
		}
		st.pop();
	}
}

int main(){
	int n,m;cin >> n >> m;
	while(cin >> n >> m){
		if(n == 0) break;

		for(int i = 1;i <= n;i++){
			ed[i].clear();
			dfs[i] = 0;
			low[i] = 0;
		}
		ptop = 0;
		sum = 0;
		while(!st.empty()){
			st.pop();
		}

		for(int i = 1;i <= m;i++){
			int u,v;cin >> u >> v;
			ed[u].push_back(v);
		}
		for(int i = 1;i <= n;i++){
			if(dfs[i] == 0)
				Dfs(i);
		}
		if(sum == 1) cout << "Yes\n";
		else cout << "No\n";
	}
}
```

## [直径](https://vjudge.net.cn/problem/洛谷-P3304)

* 题意简单，就是求树的直径与直径必须经过的边数

##### 思路：

* 首先，树的直径我们知道可以用两次dfs求最远点得出，第一次得到的点一定会有直径经过，第二次就一定是直径了
* 然后如何求使用直径都经过的边呢？我们需要知道一个结论，就是树的使用直径构成的图是中间直着，端点发散的，因此我们只要找到中间不发散的端点即可找到所有必经过的边
* 那么我们就从左边找右端点，右边找左端点即可
* 中间必定经过，那么内部就不存在最长链与原本的链长相同，找到存在的就是端点了，两边搜索用的条件不同![无标题](C:\Users\Administrator\Desktop\无标题.png)

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;

#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;

const int N = 2e5 + 10;

int ptop = 0;

struct edge{
	int v,len;
	int nex;
}ed[N];

int head[N];

int pre[N];

void add(int u,int v,int len){
	ed[++ptop].nex = head[u];
	head[u] = ptop;
	ed[ptop].v = v;
	ed[ptop].len = len;
}

ll dis[N] = {0};

pair<ll,int> madis;

void dfs(int x,int fa){
	pre[x] = fa;
	for(int p = head[x];p;p = ed[p].nex){
		int y = ed[p].v;
		if(y == x) continue;
		int len = ed[p].len;
		dis[y] = dis[x] + len;
		dfs(y,x);
		if(dis[y] > madis.first){
			madis = {dis[y],y};
		}
	}
}

bool vis[N];

int find(int x,ll aim,int num){
	vis[x] = 1;
	if(aim == 0) return num;
	for(int p = head[x];p;p = ed[p].nex){
		int y = ed[p].v;
		int len = ed[p].len;
		if(vis[y]) continue;
		find(y,aim - len,num + 1);
	}
}

void test(){
	int n;cin >> n;
	for(int i = 1;i <= n - 1;i++){
		int u,v,len;cin >> u >> v >> len;
		add(u,v,len);
	}
	dfs(1,1);
	int l = madis.second;
	madis = {0,0};
	dfs(l,l);
	int r = madis.second;
	ll d = madis.first;

	vector<int> ve;

	int ans = 1;

	while(r != l){
		ans++;
		vis[r] = 1;
		ve.push_back(r);
		r = pre[r];
	}
	vis[l] = 1;
	ve.push_back(l);

	for(auto p : ve){
		ans += find(p,min(d,d - dis[p]),0);
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

## [P3379 【模板】最近公共祖先（LCA）](https://www.luogu.com.cn/problem/P3379)

* 板子
* 利用倍增来找公共祖先

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 5e5 + 10;
int n,m,s;
vector<int> ed[N];
int dp[N][21],dep[N];

void dfs(int x,int fa){
	for(int i = 1;i <= 20;i++)
		dp[x][i] = dp[dp[x][i - 1]][i - 1];
	for(auto y:ed[x]){
		if(y == fa) continue;
		dp[y][0] = x;
		dep[y] = dep[x] + 1;
		dfs(y,x);
	}
}

int lca(int x,int y){
	if(dep[y] > dep[x]) swap(x,y);
	for(int i = 20;i >= 0;i--){
		if(dep[dp[x][i]] >= dep[y]) x = dp[x][i];
	}
	if(x == y) return x;
	for(int i = 20;i >= 0;i--){
		if(dp[x][i] != dp[y][i]) x = dp[x][i],y = dp[y][i];
	}
	return dp[x][0];
}

int main(){
    ios::sync_with_stdio(false);//写了using namespace std;
	cin >> n >> m >> s;
	for(int i = 1;i <= n - 1;i++){
		int u,v;cin >> u >> v;
		ed[u].push_back(v);
		ed[v].push_back(u);
	}
	dp[s][0] = s;
	dep[s] = 1;
	dfs(s,s);
	for(int i = 1;i <= m;i++){
		int x,y;cin >> x >> y;
		cout << lca(x,y) << endl;
	}
}
```

## [D - 单源最短路径（标准版）](https://vjudge.net.cn/problem/洛谷-P4779)

* 参考[单源最短路算法总结（拓扑、dijkstra、Floyd、Bellman-ford、SPFA） 以洛谷P1807 最长路为例](https://blog.csdn.net/xxcdsg/article/details/127737530)，毕竟是我写的

```cpp
#include<bits/stdc++.h>
using namespace std;
const int MAXN = 1e5 + 10,MAXM = 5e5 + 10;
struct edge{
	int v,val;
	edge* nex;
}ed[MAXM];
edge* head[MAXN];
int ptop = 0;
void add(int x,int y,int val)
{
	ed[ptop].v = y,ed[ptop].val = val;
	ed[ptop].nex = head[x];
	head[x] = &ed[ptop];
	ptop++;
}
bool use[MAXN];
struct pos{
	int dis,p;
	bool operator <(const pos &x)const{
		return dis > x.dis;
	}//重载运算符
};
priority_queue<pos> a;//存点
int dis[MAXN];
int main()
{
	int n,m,s;cin >> n >> m >> s;
	a.push({0,s});
	for(int i = 1;i <= n;i++)
		dis[i] = ((long long)1 << 31) - 1;
	for(int i = 1;i <= m;i++)
	{
		int x,y,v;cin >> x >> y >> v;
		add(x,y,v);
	}
	while(!a.empty())
	{
		int x = a.top().p,predis = a.top().dis;
		a.pop();
		if(use[x])
			continue;
		dis[x] = predis;
		use[x] = 1;
		edge* p = head[x];
		while(p != NULL)
		{
			int y = p -> v,val = p -> val;
			a.push({predis+val,y});
			p = p -> nex;
		}
	}
	for(int i = 1;i <= n;i++)
	cout << dis[i] << ' ';
}
```

## [P3385 【模板】负环](https://www.luogu.com.cn/problem/P3385)

* 利用spfa判负环，spfa唯一较大作用，其他就是通过奇奇怪怪的优化来用非正解过题了

```cpp
#include<bits/stdc++.h>
using namespace std;
const int MAXN = 2e3 + 10,MAXM = 3e3 + 10,INF = 0x3f3f3f3f;
struct edge{
	int v,val;
	edge* nex;
}ed[MAXM * 2];
edge* head[MAXN];
int ptop = 0;
void add_edge(int u,int v,int val)
{
	ed[ptop].v = v,ed[ptop].val = val;
	ed[ptop].nex = head[u];
	head[u] = &ed[ptop];
	ptop++;
}
int num[MAXN];
int dis[MAXN];
int n,m;
bool spfa()
{
	queue<int> qu;
	qu.push(1);
	dis[1] = 0;
	while(!qu.empty())
	{
		int x = qu.front();
		qu.pop();
		if(num[x] >= n)
		return true;
		num[x]++;
		edge* p = head[x];
		while(p != NULL)
		{
			int y = p -> v,val = p -> val;
			if(val + dis[x] < dis[y])
			{
				dis[y] = val + dis[x];
				qu.push(y);
			}
			p = p -> nex;
		}
	}
	return false;
}
void pre()
{
	for(int i = 1;i <= n;i++)
	{
		head[i] = NULL;
		num[i] = 0;
		dis[i] = INF;
	}
	ptop = 0;
}
int main()
{
	int t;cin >> t;
	while(t--)
	{
		cin >> n >> m;
		pre();
		for(int i = 1;i <= m;i++)
		{
			int u,v,val;cin >> u >> v >> val;
			if(val >= 0)
			add_edge(v,u,val);
			add_edge(u,v,val);
		}
		if(spfa())
		cout << "YES" << endl;
		else
		cout << "NO" << endl;
	}
}
```

## [P6175 无向图的最小环问题](https://www.luogu.com.cn/problem/P6175)

* 对Floyd原理的考察，最外层是中间点，每次循环一次，就给这个矩阵更新为考虑一个点加入图中的所有点到点的最短边
* 那么在加入这个点之前，我们 $dis[i][j]$ 所代表的边是不包含点 $k$ 的，那么再通过求 $dis[i][j] + d[i][k] + d[k][j](i\neq j\neq k)$ ，就是一个至少包含三个点的环

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;

#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;

const int N = 1e2 + 10;
const int inf = 1e8 + 10;

int dis[N][N];
int d[N][N];

void test(){
	int n,m;cin >> n >> m;
	for(int i = 1;i <= n;i++)
		for(int j = 1;j <= n;j++){
			d[i][j] = inf;
			dis[i][j] = inf;
		}
	for(int i = 1;i <= m;i++){
		int u,v;cin >> u >> v;
		int x;cin >> x;
		d[v][u] = d[u][v] = dis[v][u] = dis[u][v] = min(dis[u][v],x);
	}
	int mi = inf;
	for(int k = 1;k <= n;k++){
		for(int i = 1;i <= n;i++)
			for(int j = 1;j <= n;j++){
				if(i != j)
					mi = min(mi,dis[i][j] + d[i][k] + d[k][j]);
				dis[i][j] = min(dis[i][j],dis[i][k] + dis[k][j]);
			}
	}
	if(mi == inf) cout << "No solution.";
	else cout << mi << '\n';
}

int main(){
	IOS
	int t = 1;//cin >> t;
	while(t--){
		test();
	}
}
```

## [P3366 【模板】最小生成树](https://www.luogu.com.cn/problem/P3366)

* 板子，用prime或者Kruskal都可以

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 5e3 + 10,M = 2e5 + 10;
int pre[N];

int n,m;
int ans;

void init(){
	for(int i = 1;i <= n;i++)
		pre[i] = i;
}

int find(int x){
	if(pre[x] == x) return x;
	else return pre[x] = find(pre[x]);
}

struct edge{
	int u,v,w;
	bool operator<(const edge &other)const{
		return w < other.w;
	}
}ed[M];

void Kruskal(){
	sort(ed + 1,ed + 1 + m);
	for(int i = 1;i <= m;i++){
		if(find(ed[i].u) != find(ed[i].v)){
			ans += ed[i].w;
			pre[find(ed[i].u)] = find(ed[i].v);
		}
	}
}

int main(){
	cin >> n >> m;
	for(int i = 1;i <= m;i++){
		cin >> ed[i].u >> ed[i].v >> ed[i].w;
	}
	init();
	Kruskal();
	bool f = 1;
	for(int i = 2;i <= n && f;i++)
		if(find(i) != find(i - 1))
			f = 0;
	if(!f)
		cout << "orz" << endl;
	else
		cout << ans << endl;
}
```


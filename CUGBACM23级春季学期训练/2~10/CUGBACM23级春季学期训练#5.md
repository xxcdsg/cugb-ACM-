## [P1629 邮递员送信](https://www.luogu.com.cn/problem/P1629)

* 从起点到其他所有位置，然后从所有位置回到起点，问总和的最小值
* 那么我们就是要求从起点到所有点的最短路，还有所有点到起点的最短路
* 所有点到起点的最短路可以通过建立反向边，变成起点到所有点的最短路问题
* 那么我们就正常求一次单源最短路，反转边求一次单源最短路，最后求和即可

```cpp
#include<bits/stdc++.h>
using namespace std;
const int MAXN = 1e3 + 10,MAXM = 1e5 + 10,MAX = INT_MAX;

int n,m;

//快读快写
inline int read() {
    int X=0,w=1; char c=getchar();
    while (c<'0'||c>'9') { if (c=='-') w=-1; c=getchar(); }
    while (c>='0'&&c<='9') X=(X<<3)+(X<<1)+c-'0',c=getchar();
    return X*w;
}
inline void write(int x)
{
    if(x<0){
    	putchar('-');
		x=-x;
	}
    if(x>9) 
		write(x/10);
    putchar(x%10+'0');
}

//构图
int ptop = 1;
struct link{
	int nextp,dis;
	link* nex;
}p[MAXM],p2[MAXM];
link* head[MAXN],*head2[MAXN];
void add(int x,int y,int dis)
{
	p[ptop].nex = head[x];
	p[ptop].nextp = y;
	p[ptop].dis = dis;
	head[x] = &p[ptop];
	p2[ptop].nex = head2[y];
	p2[ptop].nextp = x;
	p2[ptop].dis = dis;
	head2[y] = &p2[ptop];	
	ptop++;
}

bool use[MAXN],use2[MAXN];
int dis[MAXN];
void pre()
{
	for(int i = 2;i <= n;i++)
	dis[i] = MAX;
}
struct node{
	int x,dis;
	bool operator <(const node &x)const{
		return dis > x.dis;
	}//重载运算符
};
priority_queue<node> qu;//存要排的点
void dijkstra()
{
	qu.push((node){1,0});
	while(!qu.empty())
	{
		int x = qu.top().x;
		if(use[x])
		{
			qu.pop();
			continue;
		}
		use[x] = 1;
		qu.pop();
		link* pp = head[x];
		while(pp != NULL)
		{
			int y = pp -> nextp,val = pp -> dis;
			if(dis[y] > dis[x] + val)
			{
				dis[y] = dis[x] + val;
				qu.push((node){y,dis[y]});
			}
			pp = pp -> nex;
		}
	}
}
void dijkstra2()
{
	qu.push((node){1,0});
	while(!qu.empty())
	{
		int x = qu.top().x;
		if(use2[x])
		{
			qu.pop();
			continue;
		}
		use2[x] = 1;
		qu.pop();
		link* pp = head2[x];
		while(pp != NULL)
		{
			int y = pp -> nextp,val = pp -> dis;
			if(dis[y] > dis[x] + val)
			{
				dis[y] = dis[x] + val;
				qu.push((node){y,dis[y]});
			}
			pp = pp -> nex;
		}
	}
}

int main()
{
	n = read(),m = read();
	for(int i = 1;i <= m;i++)
	{
		int x = read(),y = read(),val = read();
		add(x,y,val);
	}
	int sum = 0;
	pre();
	dijkstra();
	for(int i = 2;i <= n;i++)
	sum += dis[i];
	pre();
	dijkstra2();
	for(int i = 2;i <= n;i++)
	sum += dis[i];
	write(sum);
}
```

## [**A - Spoiler**](https://atcoder.jp/contests/abc344/tasks/abc344_a?lang=en)

* 只取两个'|'之间的字符串
* 模拟

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;

#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;

void test(){
	string s;cin >> s;
	bool f = 0;
	for(auto c : s){
		if(c == '|') f = !f;
		if(!f && c != '|') cout << c;
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

## [**B - Delimiter**](https://atcoder.jp/contests/abc344/tasks/abc344_b)

* 输入 $N$ 个数，反转输出，最后一个数为 $0$
* 同模拟

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;

#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;

const int N = 1e2 + 10;

int a[N];

void test(){
	stack<int> st;
	while(1){
		int x;cin >> x;
		st.push(x);
		if(x == 0) break;
	}
	while(!st.empty()){
		cout << st.top() << '\n';
		st.pop();
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

## [**C - A+B+C**](https://atcoder.jp/contests/abc344/tasks/abc344_c)

* 问能否从三个数列中分别选三个数凑出 $x$
* 观察数据范围，三个数列的数量都不超过 $100$ ，那么它们的所有组合不超过 $100^3=1e6$ ，我们将所有组合列出来放在set里，然后看 $x$ 是否在set里即可
* 或者将前两个的所有组合放在set里，最后一个数枚举也可以

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

set<int> se;

void test(){
	se.clear();
	vector<vector<int>> a(3,vector<int>(101,0));
	int n;cin >> n;
	for(int i = 1;i <= n;i++) cin >> a[0][i];
	int m;cin >> m;
	for(int i = 1;i <= m;i++) cin >> a[1][i];
	int k;cin >> k;
	for(int i = 1;i <= k;i++) cin >> a[2][i];
	for(int i = 1;i <= n;i++)
		for(int j = 1;j <= m;j++)
			for(int t = 1;t <= k;t++)
				se.insert(a[0][i] + a[1][j] + a[2][t]);
	int q;cin >> q;
	while(q--){
		int x;cin >> x;
		if(se.find(x) != se.end()) cout << "Yes\n";
		else cout << "No\n";
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

## [**D - String Bags**](https://atcoder.jp/contests/abc344/tasks/abc344_d)

* 因此从字符串数组中选任意一个或者不选，接在我们的字符串上
* 问要得到目标字符串最少需要选多少个字符串拼接
* 因为数据范围很小，分组背包即可
* 注意分组背包时不仅要把vis分开，还需要将dis分开

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;

#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;

const int N = 1e2 + 10;

bool vis[2][N];
int dis[2][N];

void test(){
	string aim;
	cin >> aim;
	int len = aim.size();
	for(int k = 0;k <= 1;k++)
		for(int i = 1;i <= len;i++) dis[k][i] = 99999;
	int n;cin >> n;
	int op = 0;
	vis[1][0] = vis[0][0] = 1;
	for(int i = 1;i <= n;i++){
		for(int j = 0;j <= len;j++){
			vis[op][j] = vis[1 - op][j];
			dis[op][j] = dis[1 - op][j];
		}
			
		int m;cin >> m;
		for(int j = m;j >= 1;j--){
			string s;cin >> s;
			int len2 = s.size();
			for(int k = 0;k <= len - len2;k++){
				if(!vis[op][k]) continue;
				bool f = 1;
				for(int x = 0;x < len2 && f;x++){
					if(aim[k + x] != s[x]) f = 0;
				}
				if(f){
					vis[1 - op][k + len2] = 1;
					dis[1 - op][k + len2] = min(dis[op][k] + 1,dis[1 - op][k + len2]);
				}
			}
		}
	}
	if(dis[1][len] != 99999){
		cout << dis[1][len] << '\n';
	}else
		cout << -1 << '\n';
}

int main(){
	IOS
	int t = 1;//cin >> t;
	while(t--){
		test();
	}
}
```

## [**E - Insert or Erase**](https://atcoder.jp/contests/abc344/tasks/abc344_e)

* 每次往特定数后面插入特定数，或者删去特定数，问最后的数列是什么，保证不会出现相同的两个数

##### 思路：

* 因为不会出现相同的两个数，因此每个数的位置都是唯一的，那么就是往特定位置插入特定数与删去特定位置，为了保证复杂度，我们想到删除与插入复杂度最小的链表来实现
* 因为每个数的位置都是唯一的，因此我们可以用一个map来记录每个数在链表中的下标
* 在删去点的时候，为了保证前面点可以连上后面的点，我们可以直接将后面的点前移

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;

#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;

const int N = 2e5 + 10;

int a[N];

map<int,int> mp;

int ptop = 0;

struct point{
	int x;
	int nex;
}p[N*2];

void test(){
	int n;cin >> n;
	ptop = 0;
	for(int i = 1;i <= n;i++){
		cin >> a[i];
		p[++ptop] = {a[i],-1};
		mp[a[i]] = ptop;
	}
	int q;cin >> q;
	while(q--){
		int op;cin >> op;
		if(op == 1){
			int x,y;cin >> x >> y;
			p[++ptop] = {y,-1};
			int xx = mp[x];
			if(p[xx].nex != -1 && p[p[xx].nex].x != -1){
				p[ptop].nex = p[xx].nex;
			}
			p[xx].nex = ptop;
			mp[y] = ptop;
		}else{
			int x;cin >> x;
			int xx = mp[x];
			if(p[xx].nex == -1){
				p[xx].x = -1;
			}else{
				p[xx].x = p[p[xx].nex].x;
				mp[p[p[xx].nex].x] = xx;
				p[xx].nex = p[p[xx].nex].nex;
			}
		}
	}
	for(int i = 1;i <= n;i++){
		int pp = i;
		while(pp != -1 && p[pp].x != -1){
			cout << p[pp].x << ' ';
			pp = p[pp].nex;
		}
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

## [E. Rudolf and k Bridges](https://codeforces.com/problemset/problem/1941/E)

*  $n$ 行，选连续的 $k$ 行建桥
* 建桥需要桥墩，在 $(i,j)$ 位置建桥的代价是 $a[i][j]+1$ ，并且桥墩间的横向距离不超过 $d$ ，并且如果在 $i$ 行建桥必须在 $a[i][1],a[1][m]$ 建桥墩
* 问最小的代价

##### 思路：

* 如果只是建一座桥，那么用优先队列即可实现
* 那么我们用优先队列的方法求出所有行建桥的最小代价，然后枚举所有长度为 $k$ 的区间的代价和取最小即可

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;

#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;
#define int long long

const int N = 1e2 + 10;
const int M = 2e5 + 10;

int mp[N][M];

ll ans[N];

void test(){
	int n,m,k,d;cin >> n >> m >> k >> d;

	for(int i = 1;i <= n;i++){
		int x;cin >> x;
		priority_queue<pair<int,int>,vector<pair<int,int>>,greater<pair<int,int>>> qu;
		qu.push({1,1});
		for(int j = 2;j <= m - 1;j++){
			cin >> mp[i][j];
			while(qu.top().second < j - d - 1){
				qu.pop();
			}
			qu.push({qu.top().first + mp[i][j] + 1,j});
		}
		cin >> x;
		while(qu.top().second < m - d - 1){
			qu.pop();
		}
		ans[i] = qu.top().first + 1;

	}
	int sum = 1e18 + 10;
	for(int i = 0;i <= n - k;i++){
		int aans = 0;
		for(int j = 1;j <= k;j++)
			aans += ans[i + j];
		sum = min(sum,aans);
	}
		
	cout << sum << '\n';
}

signed main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```


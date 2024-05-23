## [P4549 【模板】裴蜀定理](https://www.luogu.com.cn/problem/P4549)

* 就是个定理
* 假设 $a$ 与 $b$ 的 $gcd(a,b)=gc$ ，那么可以将 $ax+by$ 化为 $(a'x+b'y)*gc$ ，也就是说 $ax+by$ 无论如何都是 $gc$ 的倍数，同理对一堆数来说，就是它们的 $gcd$
* 那么本题的解就是所有数的 $gcd$ 了

```cpp
#include<bits/stdc++.h>
using namespace std;

int gcd(int x,int y){
	if(y == 0) return x;
	return gcd(y,x%y);
}

int main(){
	int n;cin >> n;
	int x;cin >> x;
	for(int i = 2;i <= n;i++){
		int y;cin >> y;
		x = gcd(x,y);
	}
	cout << abs(x);
}
```

## [B3611 【模板】传递闭包](https://www.luogu.com.cn/problem/B3611)

* Floyd弱化版

```cpp
#include<bits/stdc++.h>
using namespace std;

const int N = 1e2 + 10;

int a[N][N];

int main(){
	int n;cin >> n;
	for(int i = 1;i <= n;i++)
	for(int j = 1;j <= n;j++)
		cin >> a[i][j];
	for(int k = 1;k <= n;k++)
	for(int i = 1;i <= n;i++)
	for(int j = 1;j <= n;j++)
		if(a[i][k] && a[k][j])
			a[i][j] = 1;
	for(int i = 1;i <= n;i++){
		for(int j = 1;j <= n;j++)
			cout << a[i][j] << ' ';
		cout << endl;
	}
}
```

## [P5367 【模板】康托展开]()

* 模板
* 对于一个排列来说，如果我们要求这个排列的排名，那么就是求比这个排列小的排列的数量
* 假设第 $[1,i-1]$ 位不变，第 $i$ 位变小，那么这个排列肯定比原本的排列小，我们假设这个的数量为 $f[i]$ ，那么我们就是求 $\sum^n_{i=1}f[i]$
* 那么 $f[i]$ 怎么求呢，因为 $[1,i-1]$ 位不变，因此找小的数肯定从 $[i+1,n]$ 去找，那么我们从后往前枚举 $i$ 即可，用树状数组来维护每个数之前有多少个数
* 然后假设 $i$ 位之后有 $k$ 个比 $i$ 位小的数，那么 $f[i]=kA_{n-i}^{n-i}$ ， $k$ 代表取哪个数到前面， $A_{n-i}^{n-i}$ 代表给剩下的数全排列

```cpp
#include<bits/stdc++.h>
using namespace std;
#define ll long long
#define lowbit(x) (x&-x)
const int N = 1e6 + 10,p = 998244353;
int n,a[N];
ll fac[N],tr[N];
void init(){
    fac[1] = 1;
    for(int i = 2;i <= N - 10;i++)
        fac[i] = (fac[i - 1] * i) % p;
}
void add(int x,int num){
    for(;x <= n;x += lowbit(x)) tr[x] += num;
}
int query(int x){
    int ans = 0;
    for(;x >= 1;x -= lowbit(x)) ans += tr[x];
    return ans;
}
ll cantor(int *a){
    ll ans = 0;
    for(int i = 0;i < n;i++)
        add(a[i],1);
   	for(int i = 0;i < n;i++){
        add(a[i],-1);
        ans = (ans + fac[n - i - 1] * query(a[i])) % p;
    }
    return ans;
}
int main(){
	init();
	cin >> n;
	for(int i = 0;i < n;i++)
		cin >> a[i];
	cout << cantor(a) + 1;
}
```

## [E. Building an Aquarium](https://codeforces.com/problemset/problem/1873/E)

* 一个水缸内有 $n$ 个珊瑚排成一行，给出每个的高度，我们可以用 $x$ 个单位的水来填充水缸，问高度 $h$ 最大多少，如果选择了一个高度，那么所有位置的水必须至少高于 $h$

##### 思路：

* 二分答案

```cpp
#include<bits/stdc++.h>
using namespace std;

#define ll long long

int n;
ll x;

const int N = 2e5 + 10;

int a[N];

bool check(ll y){
    ll ans = 0;
    for(int i =1;i <= n;i++){
        ans += max(0ll,y - a[i]);
    }
    return x >= ans;
}

int main(){
    int t;cin >> t;
    while(t--){
        cin >> n >> x;
        for(int i = 1;i <= n;i++) cin >> a[i];
        ll l = 0,r = 2e9;
        while(l <= r){
            ll mid = (l + r) / 2;
            if(check(mid)) l = mid + 1;
            else r = mid - 1;
        }
        cout << r << endl;
    }
}
```

## [F. Money Trees](https://codeforces.com/problemset/problem/1873/F)

*  $n$ 棵树，每棵树有两个值，高度 $h[i]$ 与果实数 $a[i]$ ，我们要找到最长的 $[l,r]$ 使得 $h[i]|h[i+1](l \leq i < r)$ 并且 $\sum_{i=l}^{r}a[i]\leq k$

##### 思路：

* 我们假设 $f[i]$ 为以 $i$ 结尾的最大长度
* 那么我们容易推断出 $f[i]\leq f[i-1]+1$
* 因为当 $r+1$ ， $l$ 要么也向左边移动，要么不动
* 在移动 $i$ 的过程中，我们维护区间的和，如果 $\sum_{i=l}^{r}a[i]> k$ ，那么我们就移动左端点 $l$ 直到满足条件
* 如果 $h[i-1]\ mod\ h[i] \neq 0$ ，那么 $l$ 就必须直接移动到 $i$ 位置
* 用队列模拟也可以

```cpp
#include<bits/stdc++.h>
using namespace std;
#define ll long long
const int N = 2e5 + 10;
pair<int,int> pa[N];

int main(){
    int t;cin >> t;
    while(t--){
        int n;
        ll k;
        cin >> n >> k;
        for(int i = 1;i <= n;i++) cin >> pa[i].first;
        for(int i = 1;i <= n;i++) cin >> pa[i].second;
        queue<int> qu;
        int ans = 0;
        ll sum = 0;
        for(int i = 1;i <= n;i++){
            if(qu.empty()){
                if(pa[i].first <= k){
                    qu.push(pa[i].first);
                    sum += pa[i].first;
                }
            }else{
                if(pa[i - 1].second % pa[i].second == 0){
                    while(!qu.empty() && sum + pa[i].first > k){
                        sum -= qu.front();
                        qu.pop();
                    }
                    if(pa[i].first <= k){
                        qu.push(pa[i].first);
                        sum += pa[i].first;
                    }
                }else{
							while(!qu.empty())
								qu.pop();
                    sum = 0;
			if(pa[i].first <= k){
                    qu.push(pa[i].first);
                    sum += pa[i].first;
                }
                }
            }
            ans = max(ans,(int)qu.size());
        }
        cout << ans << endl;
    }
}
```

## [G. ABBC or BACB](https://codeforces.com/problemset/problem/1873/G)

* 一开始有一个由 $A$ 与 $B$ 组成的字符串，每次可进行两个操作
* 将一个 $AB$ 变为 $BC$ ，得到一个硬币
* 将一个 $BA$ 变为 $CB$ ，得到一个硬币

##### 思路：

* 观察对 $AAAAAAB$ 的变化
* $AAAAAAB \to AAAAABC \to AAAABCC \to AAABCCC \to AABCCCC\to ABCCCCC \to BCCCCCC$
* 同理观察 $BAAAAAA$ 的变化
* $BAAAAAA \to CBAAAAA \to CCBAAAA \to CCCBAAA \to CCCCBAA \to CCCCCBA \to CCCCCCB$
* 首先我们观察到，得到硬币的数量最多为 $A$ 的数量，因为两个操作对会减少一个 $A$ 增加一个 $C$
* 其次我们观察到，第一个操作是会将形如 $AAAAAB$ 全变成 $BCCCCC$ ，第二个操作会将 $BAAAAA$ 变为 $CCCCCB$
* 那么假设第一个字母为 $B$ 或者最后一个字母为 $B$ ，那么所有的 $A$ 均被去除就是可能的
* 否则，我们就要抛弃一个连续的 $A$ ，为了得到的硬币最多，我们取最少的连续 $A$ 去除即可

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
	int n = s.size();
	int sum = 0,num = 0;
	bool B2 = 0;
	int mi = 1e9 + 10;
	for(int i = 0;i < n;i++){
		if(s[i] == 'A'){
			sum++;
			num++;
		}else{
			mi = min(mi,num);
			num = 0;
			if(i && s[i - 1] == 'B') B2 = 1;
		}
	}
	mi = min(mi,num);
	if(B2) cout << sum << endl;
	else cout << sum - mi << endl;
}

int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [H. Mad City](https://codeforces.com/problemset/problem/1873/H)

* 两个人一个跑，一个追，两个人都知道另外一个人的下一步，一个初始在 $a$ 另一个初始在 $b$ ，问跑的那个人能否永远跑下去不被捉住
* 两个人在同一条路上相遇或者在同一个城市均认为被捉住

##### 思路：

* 思考什么时候永远不会被捉住，那就是在环上
* 如果是树结构，那么以追的人为根节点，那么跑的那个人只会被逼入越来越小的子树中去，肯定会被捉住
* 只有在环上时，跑的人一直往与追的人相反的时针方向跑，这样才永远不被捉住
* 但是我们要判断在跑的人跑到环上之前，追的人是否能堵住它
* 那么我们除了跑环还需要跑两趟最短路
* 环用 $tarjan$ 判断，最短路因为本题距离均为 $1$ 因此用 $bfs$ 即可

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;
 
#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;
 
const int N = 2e5 + 10,inf = 1e9 + 10;
 
vector<int> ed[N];
 
bool vis[N];
int dis[2][N];
 
void find(int x,int op){
	memset(vis,0,sizeof(vis));
	dis[op][x] = 0;
	vis[x] = 1;
	queue<int> qu;
	qu.push(x);
	while(!qu.empty()){
		int x = qu.front();
		qu.pop();
		for(auto y : ed[x]){
			if(vis[y]) continue;
			vis[y] = 1;
			dis[op][y] = dis[op][x] + 1;
			qu.push(y);
		}
	}
}
 
bool huan[N];
int low[N];
int dfs[N];
int ptop = 0;
stack<int> st;
 
void tarjan(int x,int dad){
	low[x] = dfs[x] = ++ptop;
	st.push(x);
	for(auto y : ed[x]){
		if(!dfs[y])
			tarjan(y,x);
		if(y != dad)
			low[x] = min(low[x],low[y]);
	}
	if(low[x] == dfs[x]){
		if(st.top() != x) huan[x] = 1;
		while(st.top() != x){
			st.pop();
		}
		st.pop();
	}else
		huan[x] = 1;
}
 
void test(){
	int n,a,b;cin >> n >> a >> b;
	ptop = 0;
	for(int i = 1;i <= n;i++){
		dfs[i] = 0;
		huan[i] = 0;
		ed[i].clear();
		dis[0][i] = dis[1][i] = inf;
	}
 
	for(int i = 1;i <= n;i++){
		int u,v;cin >> u >> v;
		ed[u].push_back(v);
		ed[v].push_back(u);
	}
 
	find(a,0);
	find(b,1);
 
	for(int i = 1;i <= n;i++)
		if(!dfs[i])
			tarjan(i,0);
	
	bool able = 0;
	for(int i = 1;i <= n;i++){
		if(huan[i] && dis[1][i] < dis[0][i]){
			able = 1;
			break;
		}
	}
	if(able) cout << "yes" << endl;
	else cout << "no" << endl;
}
 
int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```


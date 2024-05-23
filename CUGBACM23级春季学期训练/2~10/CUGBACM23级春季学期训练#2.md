# CUGBACM23级春季学期训练#2

## [P1536 村村通](https://www.luogu.com.cn/problem/P1536)

* $n$ 个点， $m$ 条路，问最少再加多少路径可以使得所有点互通

##### 思路：

* 明显，这是一个并查集题目，我们把所有共同的点放在同一集合内，那么最后统计集合数再减一就是最少的需要再加的路径的个数
* 当然，我们还可以看集合合并的次数，将所有点合入同一集合需要 $n-1$ 次，那么每一次合并集合就减一，最后输出答案也可以

```cpp
#include<bits/stdc++.h>
using namespace std;
int p[1100];
int find(int x)
{
	while(p[x] != x)
	x = p[x];
	return x;
}
void pre(int n)
{
	for(int i = 1;i <= n;i++)
		p[i] = i;
}
int main()
{
	int n,m,x,y;
	while(cin >> n,n != 0)
	{
		cin >> m;
		pre(n);
		int sum = n - 1;
		for(int j = 1;j <= m;j++)
		{
			cin >> x >> y;
			sum--;
			if(find(x) == find(y))
			{
				sum++;continue;
			}
			p[find(x)] = find(y);
		}
		cout << sum << endl;
	}
}
```

## [A. Problemsolving Log](https://codeforces.com/problemset/problem/1914/A)

* 给出字符串，从'A'到'Z'，每出现一次代表消耗一次时间，需要的时间从1到26，问解决的问题有多少
* 统计，没难度

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;

#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;

int a[27];

void test(){
	int ans = 0;
	int n;cin >> n;
	for(int i = 1;i <= 26;i++) a[i] = 0;
	string s;cin >> s;
	for(auto c : s){
		a[c - 'A' + 1]++;
	}
	for(int i = 1;i <= 26;i++){
		if(a[i] >= i) ans++;
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

## [B. Preparing for the Contest](https://codeforces.com/problemset/problem/1914/B)

* 排列 $1到n$ 的数列，使之递增的位置有 $k$ 个

##### 思路：

* 构造题，随便构造即可，比如需要 $k$ 个递增，那么我们拿出前 $k+1$ 个排 $1,2,3,...,k+1$ 即可

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;

#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;

void test(){
	int n,k;cin >> n >> k;
	for(int i = n;i >= k + 2;i--){
		cout << i << ' ';
	}
	for(int i = 1;i <= k + 1;i++){
		cout << i << ' ';
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

## [C. Quests](https://codeforces.com/problemset/problem/1914/C)

* 有 $n$ 个任务可完成，第一次完成获得 $a[i]$ 经验值，之后每次完成获得 $b[i]$ 经验值
* 只有完成前面的所有任务至少一次才能完成该任务
* 问完成不超过 $k$ 次任务能获得的最大经验值

##### 思路：

* 获得最多经验值，当然要取最多完成任务次数，因为获得的经验值都是正的

* 暴力贪心，我们枚举最远完成的任务，那么之内的任务都要完成一次，然后为了最大化经验值，再一直取最大的 $b[i]$ 即可

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;

#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;

const int N = 2e5 + 10;

int a[N],b[N];

void test(){
	int n,k;cin >> n >> k;
	for(int i = 1;i <= n;i++) cin >> a[i];
	for(int i = 1;i <= n;i++) cin >> b[i];
	int lim = min(n,k);
	int ma = 0,no = 0;
	int rangema = 0;
	for(int i = 1;i <= lim;i++){
		rangema = max(rangema,b[i]);
		no += a[i];
		ma = max(ma,no + rangema * (k - i));
	}
	cout << ma << '\n';
}

int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [D. Three Activities](https://codeforces.com/problemset/problem/1914/D)

* 从三个同长度序列中选三个数，它们的下标互不相同，问最大和为多少

##### 思路：

* 假设我们已经取了两个数，那么最后一个数在那个序列中我们肯定要选最大的，但是下标不能被选，那么最多有两个数被选中了，我们假设最坏情况，那么最大两个数都被选了，那么我们选的就是第三大的
* 那么按如此分析，我们每个序列能取的数实际上十分有限，就是最大、次大，与第三大，那么我们将这九个数拿出来，搜索或者其他办法都可
* $3^3=27$

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;

#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;

const int N = 1e5 + 10;

pair<int,int> x[N];
pair<int,int> y[N];
pair<int,int> z[N];

int ans = 0,tem = 0;

bool vis[N];

int n;

void dfs(int op){
    if(op == 4){
        ans = max(ans,tem);
        return;
    }else{
        if(op == 1){
            for(int i = n;i >= n - 2;i--){
                vis[x[i].second] = 1;
                tem += x[i].first;
                dfs(op + 1);
                tem -= x[i].first;
                vis[x[i].second] = 0;
            }
        }else if(op == 2){
            for(int i = n;i >= n - 2;i--){
                if(vis[y[i].second]) continue;
                vis[y[i].second] = 1;
                tem += y[i].first;
                dfs(op + 1);
                tem -= y[i].first;
                vis[y[i].second] = 0;
            }
        }else{
            for(int i = n;i >= n - 2;i--){
                if(vis[z[i].second]) continue;
                vis[z[i].second] = 1;
                tem += z[i].first;
                dfs(op + 1);
                tem -= z[i].first;
                vis[z[i].second] = 0;
            }
        }
    }
}

void test(){
    cin >> n;
    for(int i = 1;i <= n;i++)
        cin >> x[i].first;
    for(int i = 1;i <= n;i++)
        cin >> y[i].first;
    for(int i = 1;i <= n;i++)
        cin >> z[i].first;
    for(int i = 1;i <= n;i++)
        x[i].second = y[i].second = z[i].second = i;
    sort(x + 1,x + 1 + n);
    sort(y + 1,y + 1 + n);
    sort(z + 1,z + 1 + n);
    dfs(1);
    cout << ans << '\n';
    ans = 0;
}

int main(){
    IOS
    int t = 1;cin >> t;
    while(t--){
        test();
    }
}
```

## [E2. Game with Marbles (Hard Version)](https://codeforces.com/problemset/problem/1914/E2)

* E1与E2同理
* $n$ 种不同的弹珠，A有 $a[1]$个第一种.... $a[n]$个第n种，B同理 存在 $b$中
* A先行动
* 每次行动，需要选中两个人都有的一种颜色，然后自己丢掉1个，对方丢掉所有
* 当没有颜色双方都有时游戏结束
* 每个人都想使得自己的弹珠减去对方的弹珠数尽量大
* 问最后A的弹珠减去B的弹珠数为多少

##### 思路：

* 博弈论，贪心
* 在我们选中一种颜色进行操作的时候，我们不仅仅是让对面的弹珠全丢掉，还使得对方回合无法选中该颜色，保住了我方的该颜色弹珠，因此对于选中一种颜色的收益，实际上应该为对方的所有与我方的所有减去1
* 那么对于同种元素来说对方的所有与我方的所有减去1，对于双方都是一样的，也就是无论A还是B，它们的最优选法都是相同的，就是和最大的
* 排序然后模拟即可

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;

#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;

const int N = 2e5 + 10;

pair<int,int> pa[N];

bool cmp(pair<int,int> a,pair<int,int> b){
    return a.first + a.second < b.first + b.second;
}

void test(){
    int n;cin >> n;
    for(int i = 1;i <= n;i++) cin >> pa[i].first;
    for(int i = 1;i <= n;i++) cin >> pa[i].second;
    sort(pa + 1,pa + 1 + n,cmp);
    int op = 1;
    ll ans = 0;
    for(int i = n;i >= 1;i--){
        // cout << pa[i].first << ' ' << pa[i].second << '\n';
        if(op)
            ans += pa[i].first - 1;
        else
            ans -= pa[i].second - 1;
        op = 1 - op;
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


## [A. Thorns and Coins](https://codeforces.com/problemset/problem/1932/A)

* 从位置 $1$ 开始，每次可向前跳一步或者两步，不能走到 $*$ 上，问最多走过多少硬币

##### 思路：

* 最简单的 $dp$ 

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
	vector<int> dp(n + 1,0);
	int ans = 0;
	for(int i = 1;i < n;i++){
		int ad = 0;
		if(s[i] == '@') ad++;
		if(s[i] == '*'){
			if(s[i - 1] == '*')
				break;
			continue;
		}
		if(i != 1)
			dp[i] = max(dp[i - 1],dp[i - 2]) + ad;
		else
			dp[i] = dp[i - 1] + ad;
		ans = max(ans,dp[i]);
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

## [B. Chaya Calendar](https://codeforces.com/problemset/problem/1932/B)

* 事件 $i$ 发生在时间 $a[i],2a[i],3a[i],...$ ，问事件依次发生到事件 $n$ 的时间为多少

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
	ll ans = 0;
	for(int i = 1;i <= n;i++){
		int x;cin >> x;
		ans = (ans / x + 1) * x;
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

## [C. LR-remainders](https://codeforces.com/contest/1932/problem/C)

* 给一个长度为 $n$ 的数组，然后给一个长度为 $n$ 的命令字符串
* 一个命令过程为
* 首先输出数组 $a$ 的所有元素的乘积除以 $m$ 的余数
* 然后如果字符为 $'L'$ 就删除最左边元素，否则删除最右边元素

##### 思路：

* 删除数维护乘积余数很难处理，但是增加数维护乘积就很简单
* 那么我们反过来按删除的顺序将数组排序，一个一个加入，最后反向输出答案即可

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
	vector<int> a(n + 1);
	vector<int> b(n + 1);
	int l = 1,r = n;
	for(int i = 1;i <= n;i++)
		cin >> a[i];
	string s;cin >> s;
	for(int i = 1;i <= n;i++){
		if(s[i - 1] == 'L')
			b[i] = a[l++];
		else
			b[i] = a[r--];
	}
	vector<int> ans(n + 1);
	int no = 1;
	for(int i = n;i >= 1;i--){
		no *= b[i];
		no %= m;
		ans[i] = no;
	}
	for(int i = 1;i <= n;i++)
		cout << ans[i] << ' ';
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

## [D. Card Game](https://codeforces.com/contest/1932/problem/D)

*  $2n$ 张不同的牌，每张牌有一个花色与等级
*  每次选一张牌，然后需要选一张比上一张大的牌，然后都丢到弃牌堆
*  规定同花色的大小比较看等级
*  同时规定一个花色为王牌花色，王牌花色的牌比非王牌花色的牌都大
*  王牌花色的牌内部还是看等级排序
*  给出弃牌堆的牌（任意顺序），问是否有一种可能出牌顺序。有则输出

##### 思路：

* 首先，王牌花色的牌能和所有牌相比较（因为所有牌都不同）
* 然后非王牌花色的牌不能与同为非王牌花色但花色不同的牌比较
* 那么不成立的情况就是王牌花色很少，同时非王牌花色的牌内部无法比较
* 我们尽量将非王牌花色的同花色牌一对一对去掉，那么如果数量为奇数，就会多出一张牌出来
* 那么只要王牌花色的数量比非王牌花色数量为奇数的数量多即可完全匹配
* 其实这也可以直接看成一个二分图匹配问题，不过我们的贪心思路也可以解决

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
	map<char,vector<string>> mp;
	char big;cin >> big;
	for(int i = 1;i <= 2*n;i++){
		string s;cin >> s;
		mp[s[1]].push_back(s);
	}
	for(auto &[x,ve] : mp){
		sort(ve.begin(),ve.end());
	}
	int tem = 0;
	for(auto &[x,ve] : mp){
		if(x != big) tem += ((int)ve.size()) % 2;
	}
	if((int)mp[big].size() < tem){
		cout << "IMPOSSIBLE\n";
	}else{
		for(auto &[x,ve] : mp){
			if(x != big){
				while((int)ve.size() >= 2){
					string s1 = ve.back();
					ve.pop_back();
					cout << ve.back() << ' ' << s1 << ' ';
					ve.pop_back();
					cout << '\n';
				}
				if(!ve.empty()){
					cout << ve[0] << ' ' << mp[big].back() << ' ';
					mp[big].pop_back();
					cout << '\n';
				}
			}
		}
		while(!mp[big].empty()){
			string s1 = mp[big].back();
			mp[big].pop_back();
			cout << mp[big].back() << ' ' << s1;
			mp[big].pop_back();
			cout << '\n';
		}
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

## [E. Final Countdown](https://codeforces.com/contest/1932/problem/E)

* 问 $n$ 递减到 $0$ ，所有位改变的次数之和

##### 思路：

* 每一位改变的次数为 $\lfloor n/10^k \rfloor$
* 因此答案为 $\sum_{i=0}^{log_{10}^n}\lfloor n/10^i \rfloor$
* 直接统计肯定不行，我们采用前缀和的方式
* 我们每一位每一位计算，那么第 $k$ 位的值应该为
* $\sum_{i=0}^{log_{10}^n-k}(\lfloor n/10^{i+k} \rfloor \ mod \ 10)$
* 我们发现， $k=log_{10}^n\to0$ 的过程中，这个数是一项一项数加上的，那么我们就逐项加项统计即可
* 最后采用高精度的进位

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
	vector<int> ans(n);
	int sum = 0;
	for(int i = 0;i < n;i++){
		sum += s[i] - '0';
		ans[i] += sum;
	}
	for(int i = n - 1;i >= 1;i--){
		ans[i - 1] += ans[i] / 10;
		ans[i] %= 10;
	}
	bool f = 0;
	for(int i = 0;i < n;i++){
		if(ans[i] != 0 || f){
			cout << ans[i];
			f = 1;
		}
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

## [F. Feed Cats](https://codeforces.com/contest/1932/problem/F)

* 每只猫出现在 $[l_i,r_i]$  时间段，你可以选一些时间点喂猫，出现的猫都会被喂，同时，你不能喂一只猫两次及以上，问你最多能喂多少猫

##### 思路：

* 假设你在时间 $i$ 喂猫，能喂到 $num[i]$ ，出现的猫离开最晚时间为 $R[i]$ ，那么我们就可以列出 $dp$ 转移方程

$$
dp[i]=max(dp[i],dp[i-1])\\
$$

$$
dp[R[i]+1]=max(dp[R[i]+1],dp[i]+num[i])
$$

*  $R[i]$ 用优先队列维护， $num[i]$ 求前缀最大值即可

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
	vector<pair<int,int>> pa(m);
	vector<int> r(n + 2);
	vector<int> num(n + 2);
	for(int i = 0;i < m;i++){
		cin >> pa[i].first >> pa[i].second;
	}
	sort(pa.begin(),pa.end());
	priority_queue<int,vector<int>,greater<int>> pqu;
	int ma = 0;
	for(int no = 0,i = 1;i <= n;i++){
		while(no < m && pa[no].first == i){
			pqu.push(pa[no].second);
			ma = max(ma,pa[no].second);
			no++;
		}
		while(!pqu.empty() && pqu.top() < i){
			pqu.pop();
		}
		num[i] = (int)pqu.size();
		r[i] = ma;
	}
	vector<int> dp(n + 2,0);
	for(int i = 1;i <= n + 1;i++){
		dp[i] = max(dp[i - 1],dp[i]);
		dp[r[i] + 1] = max(dp[r[i] + 1],dp[i] + num[i]);
	}
	cout << dp[n + 1] << '\n';
}

int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

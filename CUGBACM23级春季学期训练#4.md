## [P1044 [NOIP2003 普及组] 栈](https://www.luogu.com.cn/problem/P1044)

## [P1375 小猫](https://www.luogu.com.cn/problem/P1375)

* 详见卡特兰数https://www.bilibili.com/video/BV1td4y1F7v1/?vd_source=857d53a194983cba3a075def75e96078

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;

#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;

const int N = 2e5 + 10;
const int mod = 1e9 + 7;

ll A[N];

ll qpow(int x,int y){
	ll res = 1;
	while(y){
		if(y&1) res = res * x % mod;
		x = ((ll)x*x) % mod;
		y >>= 1;
	}
	return res;
}

void init(){
	A[0] = 1;
	for(int i = 1;i <= N - 10;i++){
		A[i] = A[i - 1] * i % mod;
	}
}

ll C(int n,int m){
	return A[n] * (qpow(A[m],mod - 2) * qpow(A[n - m],mod - 2) % mod) % mod;
}

void test(){
	int n;cin >> n;
	cout << (C(n*2,n) - C(n*2,n-1) + mod) % mod << '\n';
}

int main(){
	IOS
	init();
	int t = 1;//cin >> t;
	while(t--){
		test();
	}
}
```



## [A. Recovering a Small String](https://codeforces.com/contest/1931/problem/A)

* a代1，b代2，求一个字典序最小的三字母字符串使得和为n

##### 思路：

* 为了字典序小，那么就要让前面字母尽量小，因此从后往前分配即可

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;

#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;

void test(){
	int n;cin >> n;
	n -= 3;
	string s;
	for(int i = 1;i <= 3;i++){
		s.push_back('a' + min(25,n));
		n -= min(25,n);
	}
	reverse(s.begin(),s.end());
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

## [B. Make Equal](https://codeforces.com/contest/1931/problem/B)

* 小序号的水可移任意量到大序号，问能否将所有位置的水量都相等

##### 思路：

* 求出平均值，也就是最后所有位置所需要达到的水量
* 求前缀和，因为水无法往前面移动，因此前缀和就是前面所有水的和的最大值
* 如果有前缀和不足以得到平均值*数量，那么无论如何分配，都无法使得所有位置的水量达到所需平均值，因此不行

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

void test(){
	int n;cin >> n;
	int sum = 0;
	for(int i = 1;i <= n;i++){
		cin >> a[i];
		sum += a[i];
	}
	int aim = sum / n;
	int ad = 0;
	bool f = 1;
	for(int i = 1;i <= n;i++){
		ad += a[i] - aim;
		if(ad < 0) f = 0;
	}
	if(f) cout << "Yes\n";
	else cout << "No\n";
}

int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [C. Make Equal Again](https://codeforces.com/contest/1931/problem/C)

* 只能将一个区域的数全变成一个任意的值，使得所有数都相同，问这个区域的最小长度

##### 思路：

* 设这个区域的范围为 $[l,r]$ ，那么就要求 $[1,l-1]$ ， $[r+1,n]$ 位置的数都相同，为了尽量让 $[l,r]$ 尽量小，那么我们就要尽量拓展 $[1,l-1]$ 与 $[r+1,n]$ 这两个区间，也就是我们找出前面完全相同的区间大小与后面完全相同的区间大小
* 如果前面区间与后面区间的数也是相同的，那么这两个就同时考虑，否则哪个区间大，取哪种方案

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

void test(){
	int n;cin >> n;
	for(int i = 1;i <= n;i++){
		cin >> a[i];
	}
	int ma = 0;
	for(int i = 1;i <= n;i++){
		if(a[i] == a[1]){
			ma = i;
		}else
			break;
	}
	int maa = 0;
	for(int i = n;i >= 1;i--){
		if(a[n] == a[i]){
			maa = n - i + 1;
		}else
			break;
	}
	if(a[1] == a[n]) ma = maa = min(n,ma + maa);
	cout << n - max(ma,maa) << '\n';
}

int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [D. Divisible Pairs](https://codeforces.com/contest/1931/problem/D)

$$
\begin{cases}
(a_i+a_j)\mod x=0
\\
(a_i-a_j)\mod y=0
\end{cases}
$$

* 求对数

##### 思路：

* 转化一下

$$
\begin{cases}
a_i\mod x=(x-a_j)\mod x
\\
a_i\mod y=a_j\mod y
\end{cases}
$$

* 用map存pair<int,int>计数枚举即可解决

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

map<pair<int,int>,int> mp;

void test(){
	int n,x,y;cin >> n >> x >> y;

	mp.clear();

	ll ans = 0;

	for(int i = 1;i <= n;i++){
		cin >> a[i];

		ans += mp[make_pair(a[i] % x,a[i] % y)];

		mp[make_pair(((x - a[i]) % x + x) % x,a[i] % y)]++;
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

## [E. Anna and the Valentine's Day Gift](https://codeforces.com/contest/1931/problem/E)

* A可反转数字，使得其后缀0全消失，A目标是使最后的值尽量小
* B可拼接数字，任意拼接两个数字，ab或者ba，目标是使得最后的值尽量大
* 问最后缩小的数是否能大于等于 $10^m$

##### 思路：

* 给 $10^m$ 这个条件就是问最后的数的位数能否大于 $m+1$
* 我们设第 $i$ 个数的位数为 $a[i]$ ，后缀0的数量为 $b[i]$
* A对一个数存在后第 $i$ 个数的后缀0的数量就变成了0，它的位数变成了 $a[i]-b[i]$ 
* B对一对数存在后，假设前面的数为第 $i$ 个数，那么那个数的后缀0就肯定不会被消除，因为它后面还有一个非0的数
* 也就是说B的一个操作会让一个 $b$ ，不会被A消除
* 那么就是A拿一个b，B拿一个b，两个人都采取最佳策略，那么就都取大的，排序模拟即可

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

void test(){
	int n,m;cin >> n >> m;
	int sum = 0;
	for(int i = 1;i <= n;i++){
		cin >> a[i];
		int x = 0;
		while(a[i] % 10 == 0){
			x++;
			a[i] /= 10;
		}
		sum += x;
		while(a[i]){
			sum++;
			a[i] /= 10;
		}
		a[i] = x;
	}
	sort(a + 1,a + 1 + n);
	for(int i = n;i >= 1;i -= 2){
		sum -= a[i];
	}
	if(sum >= m + 1) cout << "Sasha\n";
	else cout << "Anna\n";
}

int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [F. Chat Screenshots](https://codeforces.com/contest/1931/problem/F)

* 给出 $k$ 个长度为 $n$ 的 $[1,n]$ 的排列
* 每个排列的第一个位置都是没用的
* 每个排列除了第一个位置的数，其他数都是根据某个总排列进行排列的
* 问所有排列是否有同一个总排列

##### 思路：

* 如果长度为1，那么无论怎么排列都是可以的
* 如果长度为2，那么只要去掉那两个开头的数，剩下的排列完全相同，那么这个排列就是可以的
* 如果长度大于2，那么从头（第二位）或者尾开始枚举，如果这个位置的数没有被提取出来，那么这个位置的数都是相同的，就是这个位置的数（比如第一个位置的数没有在第一个，那么所有排列的第二位肯定是第一个位置的数），但是如果有排列将这个数提取出来到了第一位，那么这个位置就与其他 $k-1$ 个位置的数不同，我们就只跳过其他 $k-1$ 个位置的数即可，直到较多的数的数量不是 $k-1$ 或者不同位置的排列的提取出来的数与其他 $k-1$ 个排列的数不同就说明不成立

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;

#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;

const int N = 2e5 + 10;

stack<int> st[N];

bool f[N];

void test(){
	int n,k;cin >> n >> k;
	for(int i = 1;i <= n;i++) f[i] = 0;
	for(int i = 1;i <= k;i++){
		while(!st[i].empty())
			st[i].pop();
	}
	for(int i = 1;i <= k;i++){
		int x;cin >> x;
		st[i].push(x);
		f[x] = 1;
		for(int j = 1;j <= n - 1;j++){
			cin >> x;
			st[i].push(x);
		}
	}
	if(n <= 2 || k == 1){
		cout << "Yes\n";
	}else{
		bool ff = 1;
		for(int i = (k == 2 ? 3 : 1);i <= n;i++){
			int x;
			if(k == 2){
				if(f[st[1].top()]) st[1].pop();
				if(f[st[2].top()]) st[2].pop();
				x = st[1].top();
			}else if(st[1].top() == st[2].top() || st[1].top() == st[3].top()){
				x = st[1].top();
			}else{
				x = st[2].top();
			}
			int num = 0;
			for(int j = 1;j <= k;j++){
				if(st[j].top() == x){
					st[j].pop();
					num++;
				}
			}
			if(!((f[x] && num == k - 1) || num == k))
				ff = 0;
		}
		if(ff) cout << "Yes\n";
		else cout << "No\n";
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


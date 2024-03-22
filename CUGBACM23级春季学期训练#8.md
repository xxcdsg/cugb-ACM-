## [A P3382 三分](https://www.luogu.com.cn/problem/P3382)

* 三分模板题
* 对于单峰函数，已经确定极值点的范围，我们要求最高点(最低点)
* 对于范围 $[l,r]$ 我们取两个点 $lmid=l+(r-l)/3.0,rmid=l+2*(r-l)/3.0$ ，就是取三分点，那么比较两个点那个更接近极值点，那么另外一个点的三分之一部分肯定不包括极值点，因此每次都可以将范围减少到原来的三分之二
* 当然你也可以取四等分点，根据中间三个点的比较，每次可以去掉一半的范围
* 也可以用稀奇古怪的取点比较法，只要每次将范围缩小，那么一般都可以

```cpp
#include<bits/stdc++.h>
using namespace std;
double a[15],eps = 1e-7;
int n;

double f(double x){
	double ans = 0;
	double t = 1;
	for(int i = n;i >= 1;i--){
		ans += t * a[i];
		t *= x;
	}
	return ans;
}

int main(){
	double l,r;
	cin >> n >> l >> r;
	n++;
	for(int i = 1;i <= n;i++)
		cin >> a[i];
	while(r-l > eps){
		double mid = (l + r) / 2;
		double lmid = (l + mid) / 2,rmid = (r + mid) / 2;
		double _m = f(mid),_l = f(lmid),_r = f(rmid);
		if(_m > _l && _m > _r)
			l = lmid,r = rmid;
		else if(_m > _l)
			l = mid;
		else
			r = mid;
	}
	printf("%.5lf",l);
}
```

## [A. Two Vessels](https://codeforces.com/problemset/problem/1872/A)

* 数学题

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;
 
#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;
 
void test(){
	int a,b,c;cin >> a >> b >> c;
	c *= 2;
	int add = 0;
	if((a - b) % c != 0) add = 1;
	cout << abs(a - b) / c + add << endl;
}
 
int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [B. The Corridor or There and Back Again](https://codeforces.com/problemset/problem/1872/B)

* 一个人要从房间 $1$ 走到最远能到的房间再回到房间 $1$，有房间内存在陷阱，陷阱会在进入存在陷阱的房间内激活，如何经过 $s[i]$ 秒发动，陷阱发动后，该房间就无法进入或离开
* 问能到最远的房间是

##### 思路：

* 如果只有一个房间有陷阱，假设位置为 $x$ ，时间为 $s$ ，那么你最多到的房间就是 $x+(s-1)/2$ ，那么我们将所有的限制都加上，求出的最小值就是答案

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
	int ans = 1e9 + 10;
	for(int i = 1;i <= n;i++){
		int d,s;cin >> d >> s;
		s--;
		ans = min(ans,d + s / 2);
	}
	cout << ans << endl;
}
 
int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [C. Non-coprime Split](https://codeforces.com/problemset/problem/1872/C)

* 如题意，求 $l\leq a+b \leq r,gcd(a,b) \neq 1$

##### 思路：

* 我们假设 $gcd(a,b) = x$ ，那么有 $l \leq kx \leq r$
* 当 $k\neq 1$ 时，我们可令 $a=x,b=(k-1)x$
* 枚举 $x$ 最大到 $\sqrt{r}$ ，因此复杂度为 $\sqrt {r}$
* 为了让 $k$ 尽量大，取 $k = r/x$ ，同时要满足 $l\leq kx$

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;
 
#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;
 
void test(){
	int l,r;cin >> l >> r;
	int sq = sqrt(r);
	for(int i = 2;i <= sq;i++){
		int aim = r / i;
		if(aim*i >= l && aim >= 2){
			cout << i << ' ' << i * (aim - 1) << endl;
			return;
		}
	}
	cout << -1 << endl;
}
 
int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [D. Plus Minus Permutation](https://codeforces.com/problemset/problem/1872/D)

* 下标为 $x$ 的倍数加入得分，下标为 $y$ 的倍数从得分中减去
* 问得分的最大值

##### 思路：

* 那么有贡献的下标分三种：只为 $x$ 的倍数，只为 $y$ 的倍数，为 $lcm(x,y)$ 的倍数
* 为了得分最大，只为 $x$ 的倍数取大的值，只为 $y$ 的倍数取小的值，为 $lcm(x,y)$ 的倍数无所谓
* 然后用等差数列求和公式即可

```cpp
// xxc
#include<bits/stdc++.h>
using namespace std;
 
#define endl '\n'
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
#define ll long long
#define debug(x) cout << #x << ' ' << x << endl;
 
int gcd(int x,int y){
	if(y == 0) return x;
	else return gcd(y,x % y);
}
 
ll lcm(int x,int y){
	return 1ll * x * y / gcd(x,y);
}
 
void test(){
	int n,x,y;cin >> n >> x >> y;
	int r1 = n / x,r2 = n / y;
	ll lc = lcm(x,y);
	int recover = n / lc;
	r1 -= recover;
	r2 -= recover;
	ll tem1 = 1ll * (2*n - r1 + 1) * r1 / 2;
	ll tem2 = 1ll * (r2 + 1) * r2 / 2;
	cout << tem1 - tem2 << endl;
}
 
int main(){
	IOS
	int t = 1;cin >> t;
	while(t--){
		test();
	}
}
```

## [A.MEX Game 1](https://codeforces.com/contest/1943/problem/A)

* 两个人博弈，第一个人从 $a$ 中挑一个数放到 $b$ 中，第二个人从 $a$ 中挑一个数丢掉
* 第一个人先走，第二个人后动
* 第一个人目的是让 $b$ 中的 $MEX$ 最大，第二个人相反
* 问最后 $b$ 中的 $MEX$ 为多少

##### 思路：

* 如果第一个人的目的为 $x$ ，那么如果 $a$ 中存在一个数它只有一个并且小于 $x$ ，那么它肯定先选，不然就被后一个人截胡了
* 然后后一个人选，那么如果它选了一个不止一个的数，那么前一个人在下一回合就可以选和它相同的数，来使得后一个人的操作无效
* 因此我们得出， $MEX$ 最大值满足，前面的数的数量都大于 $2$ ，或者至多有一个 $1$

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

int num[N];

void test(){
	int n;cin >> n;
	for(int i = 0;i <= n;i++) num[i] = 0;
	for(int i = 1;i <= n;i++){
		int x;cin >> x;
		num[x]++;
	}
	bool f = 0;
	for(int i = 0;i <= n;i++){
		if(num[i] == 0 || (num[i] == 1 && f)){
			cout << i << '\n';
			return;
		}else if(num[i] == 1)
			f = 1;
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

## [E. Data Structures Fan](https://codeforces.com/problemset/problem/1872/E)

* 给一个长度为 $n$ 的序列 $a$ ，再给一个同样长度的 $01$ 字符串
*  $q$ 次操作，有两种操作
* 第一种为将 $[l,r]$ 区间内 $01$ 字符串转化， $0$ 变 $1$ ， $1$ 变 $0$
* 第二种为求所有 $0$ 对于的 $a$ 的异或和（或者 $1$ 的）

##### 思路：

* 我们假设 $0$ 对应的异或和为 $sum0$
* 当 $[l,r]$ 字符串内转化时，内部所有的 $0$ 要对 $sum0\ XOR\ a[i_0]$，取消原本的 $0$ ，同时我们还有异或上所有的 $1$ 因为 $1$ 变成了 $0$ ，因此相当于异或区间内的所有数，那么内部的 $01$ 分布就不重要，我们求一个异或前缀和即可维护 $sum0$ 与 $sum1$

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
	int sum0 = 0,sum1 = 0;
	vector<int> xorsum(n + 1,0);
	vector<int> a(n+1);
	for(int i = 1;i <= n;i++){
		cin >> a[i];
		xorsum[i] = xorsum[i - 1] ^ a[i];
	}
	string s;cin >> s;
	for(int i = 0;i < n;i++){
		if(s[i] == '0') sum0 ^= a[i + 1];
		else sum1 ^= a[i + 1];
	}
	int q;cin >> q;
	while(q--){
		int op;cin >> op;
		if(op == 1){
			int l,r;cin >> l >> r;
			int tem = xorsum[r] ^ xorsum[l - 1];
			sum0 ^= tem;
			sum1 ^= tem;
		}else{
			int oop;cin >> oop;
			if(oop == 1) cout << sum1 << ' ';
			else cout << sum0 << ' ';
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

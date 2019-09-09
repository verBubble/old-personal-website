# Kickstart 2017 Round F

## Problem A Kicksort

### Problem

Here at Kickstart, we are fans of the well-known [Quicksort](https://en.wikipedia.org/wiki/Quicksort) algorithm, which chooses a *pivot* value from a list, moves each other value into one of two new lists depending on how it compares with the pivot value, and then recursively sorts each of those new lists. However, the algorithm might choose a pivot that causes all of the other values to end up in only one of the two new lists, which defeats the purpose of the divide-and-conquer strategy. We call such a pivot a *worst-case pivot*.

To try to avoid this problem, we have created our own variant, Kicksort. Someone told us that it is good to use a value in the middle as a pivot, so our algorithm works as follows:

```
  Kicksort(A): // A is a 0-indexed array with E elements
    If E ≤ 1, return A.
    Otherwise:
      Create empty new lists B and C.
      Choose A[floor((E-1)/2)] as the pivot P.
      For i = 0 to E-1, except for i = floor((E-1)/2):
        If A[i] ≤ P, append it to B.
        Otherwise, append it to C.
    Return the list Kicksort(B) + P + Kicksort(C).
```

For practice, we are trying Kicksort out on lists that are permutations of the numbers 1 through **N**. Unfortunately, it looks like Kicksort still has the same problem as Quicksort: it is possible for every pivot to be a worst-case pivot!

For example, consider the list `1 4 3 2`. Kicksort will choose `4` as a pivot, and all of the other values `1 3 2` will end up in one of the two new lists. Then, when Kicksort is called on that list `1 3 2`, it will choose `3`, and once again, all of the other values will end up in one of the two new lists. Finally, it will choose `1` from the list `1 2`, and the other value `2` will of course end up in only one of the two new lists. In every case, the algorithm will choose a worst-case pivot. (Notice that when Kicksort is called on a list with 0 or 1 elements, it does not choose a pivot at all.)

Please help us investigate this further! Given a permutation of the numbers 1 through **N**, determine whether Kicksort will choose only worst-case pivots.

### Input

The first line of the input gives the number of test cases, **T**. **T** test cases follow; each consists of two lines. The first line has one integer **N**: the number of elements in the permutation. The second line contains **N** integers **Ai**, which are a permutation of the values from 1 through **N**.

### Output

For each test case, output one line containing `Case #x: y`, where `x` is the test case number (starting from 1) and `y` is `YES` if Kicksort will choose only worst-case pivots when sorting this list, or `NO` otherwise.

### Limits

The values **Ai** are a permutation of the values from 1 to **N**.

#### Small dataset

1 ≤ **T** ≤ 32.
2 ≤ **N** ≤ 4.

#### Large dataset

1 ≤ **T** ≤ 100.
2 ≤ **N** ≤ 10000.

### Sample

| input   | output       |
| ------- | ------------ |
| 4       | Case #1: YES |
| 4       | Case #2: NO  |
| 1 4 3 2 | Case #3: YES |
| 4       | Case #4: NO  |
| 2 1 3 4 |              |
| 2       |              |
| 2 1     |              |
| 3       |              |
| 1 2 3   |              |

### 分析

问Kicksort是不是最坏的情况。首先要注意到数字序列是从1~N之间取的值，所以最大最小值不用排序就可以得到。

另外注意找一下每一次取的数字在队列中位置的规律，然后就可以写出来代码了。

```C++
#include<iostream>
#include<algorithm>
using namespace std;

int num[10010];

int main(){
	freopen("A-large-practice.in","r",stdin);
	freopen("answer.out","w",stdout);
	int T;
	cin>>T;
	int n;
	int c=1;
	while(T--){
		cin>>n;
		for(int i=0;i<n;i++){
			cin>>num[i];
		}
		int ans=1;
		int p=int((n-1)/2);
		int mmax=n;
		int mmin=1;
		for(int i=0;i<n-1;i++){
			if(num[p]!=mmax && num[p]!=mmin){
				ans=0;
				break;
			}
			if(num[p]==mmax) mmax-=1;
			if(num[p]==mmin) mmin+=1;
			if(n%2==0){
				if(i%2==0){
					p+=i+1;
				}
				else{
					p-=i+1;
				}
			}
			else{
				if(i%2==0){
					p-=i+1;
				}
				else{
					p+=i+1;
				}
			}
		}
		cout<<"Case #"<<c<<": ";
		if(ans==1) cout<<"YES"<<endl;
		else cout<<"NO"<<endl;
		c++;
	}
	return 0;
}
```



## Problem B Dance Battle

### Problem

Your team is about to prove itself in a dance battle! Initially, your team has **E** points of energy, and zero points of honor. There are **N** rival teams who you must face; the i-th of these teams is the i-th in a lineup, and has a dancing skill of **Si**.

In each round of battle, you will face the next rival team in the lineup, and you can take one of the following actions:

1. *Dance*: Your team loses energy equal to the dancing skill of the rival team, and that team does not return to the lineup. You gain one point of honor. You cannot take this action if it would make your energy drop to 0 or less.
2. *Delay*: You make excuses ("our shoes aren't tied!") and the rival team returns to the back of the lineup. Your energy and honor do not change.
3. *Truce*: You declare a truce with the rival team, and that team does not return to the lineup. Your energy and honor do not change.
4. *Recruit*: You recruit the rival team onto your team, and that team does not return to the lineup. Your team gains energy equal to the dancing skill of the rival team, but you lose one point of honor. You cannot take this action if it would make your honor drop below 0.

The battle is over when there are no more rival teams in the lineup. If you make optimal decisions, what is the maximum amount of honor you can have when the battle is over?

### Input

The first line of the input gives the number of test cases, **T**. **T** test cases follow; each consists of two lines. The first line consists of two integers **E** and **N**: your team's energy, and the number of rival teams. The second line consists of **N** integers **Si**; the i-th of these represents the dancing skill of the rival team that is i-th in line at the start of the battle.

### Output

For each test case, output one line containing `Case #x: y`, where `x` is the test case number (starting from 1) and `y` is the maximum amount of honor you can have when the battle is over.

### Limits

1 ≤ **T** ≤ 100.
1 ≤ **E** ≤ 106.
1 ≤ **Si** ≤ 106, for all i.

#### Small dataset

1 ≤ **N** ≤ 5.

#### Large dataset

1 ≤ **N** ≤ 1000.

### Sample

| input   | output     |
| ------- | ---------- |
| 2       | Problem    |
| 100 1   | Case #1: 0 |
| 100     | Case #2: 1 |
| 10 3    |            |
| 20 3 15 |            |

### 分析

注意题中的delay战略，也就是说对手的顺序是无所谓的。这个时候只需要将对手按照skills排序，按照贪心算法的策略，先尽量Dance，拿到Honor。如果现在的能量不够对战的话，那么从skill最大的队伍开始Recruit，就算拿到的skill只能打败一个队伍，那么Honor少一个又增加一个其实是不亏的。当Honor不足够Recruit的时候，剩下的队伍都Truce。整个过程中只要保证Energy大于0，Honor不小于0即可。

```C++
#include<iostream>
#include<algorithm>
using namespace std;

typedef long long ll;
ll s[1010];
ll n;
ll e,h;

ll solve(){
	h=0;
	cin>>e>>n;
	int i=0,j=n-1;
	for(int a=0;a<n;a++){
		cin>>s[a];
	}
	sort(s,s+n);
	while(i<=j){
		if(e>s[i]){
			e-=s[i];
			i++;
			h++;
		}
		else{
			if(h>0 && (j-i)>0){
				e+=s[j];
				h--;
				j--;
			}
			else{
				break;
			}
		}
	}
	return h;
}

int main(){
	freopen("B-large-practice.in","r",stdin);
	freopen("answer.out","w",stdout);
	int T;
	cin>>T;
	int c=1;
	while(T--){
		ll ans=solve();
		cout<<"Case #"<<c<<": "<<h<<endl;
		c++;
	}
}
```



## Problem C Catch Them All

## Problem

After the release of ***Codejamon Go***, you, like many of your friends, took to the streets of your city to catch as many of the furry little creatures as you could. The objective of the game is to catch *Codejamon* that appear around your city by going to their locations. You are wondering how long it would take for you to catch them all!

Your city consists of **N** locations numbered from 1 to **N**. You start at location 1. There are **M** bidirectional roads (numbered from 1 to **M**). The i-th road connects a pair of distinct locations (Ui, Vi), and it takes **Di** minutes to travel on it in either direction. It is guaranteed that it is possible to reach any other location from location 1 by travelling on one or more roads.

At time 0, a *Codejamon* will appear at a uniformly random location other than your current location (which is location 1 at time 0). Uniformly random means that the probability that it will appear at each of the **N** - 1 locations other than your current location is exactly 1 / (**N** - 1). The instant that a *Codejamon* appears, you can immediately start moving towards it. When you arrive at a location containing a *Codejamon*, you instantly catch it, and then a new *Codejamon* will instantly appear at a uniformly random location other than your current location, and so on. Notice that only one Codejamon is present at any given time, and you must catch the existing one before the next will appear.

Given the layout of your city, calculate the expected time to catch **P** *Codejamon*, assuming that you always take the fastest possible route between any two locations.

### Input

The input starts with one line containing one integer **T**: the number of test cases. **T** test cases follow.

Each test case begins with one line containing 3 integers **N**, **M** and **P**, indicating the number of locations, roads, and *Codejamon* to catch, respectively.

Then, each test case continues with **M** lines; the i-th of these lines contains three integers **Ui**, **Vi** and **Di**, indicating that the i-th road is between locations **Ui** and **Vi**, and it takes **Di**minutes to travel on it in either direction.

### Output

For each test case, output one line containing `Case #x: y`, where `x` is the test case number (starting from 1) and `y` is the expected time in minutes to catch **P** *Codejamon*. Your answer will be considered correct if it is within an absolute or relative error of 10-4 of the correct answer. See the [FAQ](https://code.google.com/codejam/resources/faq#real-number-behavior) for an explanation of what that means, and what formats of real numbers we accept.

### Limits

1 ≤ T ≤ 100.

N - 1 ≤ M ≤ (N \* (N \- 1)) / 2.

1 ≤ Di ≤ 10, for all i.

1 ≤ Ui < Vi ≤ N, for all i.

For all i and j with i ≠ j, Ui ≠ Uj and/or Vi ≠ Vj. (There is at most one road between any two locations.)

It is guaranteed that it is possible to reach any other location from location 1 by travelling on one or more roads.

#### Small dataset

2 ≤ N ≤ 50.

1 ≤ P ≤ 200.

#### Large dataset

2 ≤ N ≤ 100.

1 ≤ P ≤ 10^9.

### Sample

| input   | output               |
| ------- | -------------------- |
| 4       | Case #1: 2.250000    |
| 5 4 1   | Case #2: 1000.000000 |
| 1 2 1   | Case #3: 5.437500    |
| 2 3 2   | Case #4: 1.500000    |
| 1 4 2   |                      |
| 4 5 1   |                      |
| 2 1 200 |                      |
| 1 2 5   |                      |
| 5 4 2   |                      |
| 1 2 1   |                      |
| 2 3 2   |                      |
| 1 4 2   |                      |
| 4 5 1   |                      |
| 3 3 1   |                      |
| 1 2 3   |                      |
| 1 3 1   |                      |
| 2 3 1   |                      |

### 分析

这道题应该是最难的了。按照题目给出的条件，不难想到是和图论有关系，按照Floyd算法求全局最短路即可。但是这里涉及到一个抓P只小精灵的问题，和马尔可夫转移矩阵有关系。

在这里我们定义了一个类：matrix，定义了matrix的乘法操作，方便之后求多步马尔可夫矩阵。

我们首先用Floyd算法求全局最短路，保存在数组d中。然后初始化第一步马尔可夫矩阵A。矩阵的结构如下，以3个结点为例：

| \    | 0     | 1     | 2     | 3    |
| ---- | ----- | ----- | ----- | ---- |
| 0    | 0     | 1/n-1 | 1/n-1 | E01  |
| 1    | 1/n-1 | 0     | 1/n-1 | E11  |
| 2    | 1/n-1 | 1/n-1 | 0     | E21  |
| 3    | 0     | 0     | 0     | 1    |

定义三个节点编号分别为0，1，2。转移矩阵是一个4*4的矩阵，其中行和列下标为i和j代表从i转移到j的概率，i和j都属于[0,2]。`A[i][3]`中的数字为Eij。Eij代表从第i个节点开始，抓j个小精灵的期望时间。

由于题目中已经给出了初始位置为1(在转移矩阵的坐标定义下为0)，所以当我们将转移矩阵A^P计算出来之后，该矩阵(记为B)的`B[0][n]`即为答案。

其中求幂用快速幂运算即可。

```C++
#include<iostream>
#include<memory.h>
#include<cstdio>
using namespace std;

int d[110][110];  //记录顶点之间的最短距离

class matrix{
	public:
		int r;
		double A[110][110];  //定义转移矩阵
	
	matrix() {}
	
	matrix(int k){
		r=k;
		for(int i=0;i<=r;i++){
			for(int j=0;j<=r;j++){
				A[i][j]=0;
			}
		}
	} 
	void init0(){  //初始化转移矩阵
		for(int i=0;i<=r;i++){
			for(int j=0;j<=r;j++){
				A[i][j] = 0;
			}
		}
	}
	void idmetrix(){    //定义单位矩阵
		for(int i=0;i<=r;i++){
			for(int j=0;j<=r;j++){
				if(i==j) A[i][j]=1;
				else A[i][j]=0;
			}
		}
	}
	
	matrix operator*(const matrix B) const{
		matrix temp(r);
		for(int i=0;i<=r;i++){
			for(int j=0;j<=r;j++){
				for(int k=0;k<=r;k++){
					temp.A[i][j]+=A[i][k]*B.A[k][j];
				}
			}
		}
		return temp;
	}	 
};

matrix Pow(matrix A,int t){
	matrix ans(A.r);
	ans.idmetrix();
	while(t){
		if(t&1) ans=A*ans;
		A = A*A;
		t = t/2;
	}
	return ans;
}

matrix Ans;
int n,m,p;
double ans;

void Init(){
	memset(d,0x3f,sizeof(d));
	cin>>n>>m>>p;
	int u,v,w;
	for(int i=1;i<=m;i++){
		cin>>u>>v>>w;
		d[u][v] = d[v][u] = w;
	}
	Ans.r = n;
	Ans.init0();
	for(int i=1;i<=n;i++) d[i][i]=0;
	
	//计算最短路
	for(int k=1;k<=n;k++){
		for(int i=1;i<=n;i++){
			for(int j=1;j<=n;j++){
				if(d[i][j] > d[i][k]+d[k][j]) 
					d[i][j] = d[i][k]+d[k][j];
			}
		}
	}
	
	for(int i=1;i<=n;i++){
		d[i][0]=0;
		for(int j=1;j<=n;j++){
			d[i][0]+=d[i][j];
		}
	}
	
	//初始化转移矩阵
	for(int i=0;i<n;i++){
		for(int j=0;j<=n;j++){
			if(j==n) Ans.A[i][j] = (double)d[i+1][0]/(n-1);
			else if(i==j) Ans.A[i][j]=0;
			else Ans.A[i][j]=(double)1/(n-1);
		}
	}
	
	Ans.A[n][n] = 1.0; 
	 
}

void work(){
	matrix temp(n);
	temp = Pow(Ans,p);
	ans =temp.A[0][n];
}

int main(){
	freopen("C-large-practice.in","r",stdin);
	freopen("answer.out","w",stdout);
	int T,c=1;
	cin>>T;
	while(T--){
		Init();
		work();
		printf("Case #%d: %.6f\n",c,ans);
		c++;
	}
	return 0;
}


```



## Problem D Eat Cake

## Problem

Wheatley is at the best party in the world: it has infinitely many cakes! Each cake is a square with an integer side length (in cm). The party has infinitely many cakes of every possible integer side length. The cakes all have the same depth, so we will only consider their areas.

Wheatley is determined to eat one or more cakes that have a total combined area of *exactly* **N** cm2. But, since he is health-conscious, he wants to eat as few cakes as possible. Can you help him calculate the minimum number of cakes he can eat?

### Input

The input starts with one line containing one integer **T**, which is the number of test cases. **T** test cases follow. Each case consists of one line with one integer **N**, which is the exact total cake area that Wheatley wants to eat.

### Output

For each test case, output one line containing `Case #x: y`, where `x` is the test case number (starting from 1) and `y` is the minimum number of cakes that Wheatley can eat while eating the exact total area **N**.

### Limits

#### Small dataset

1 ≤ **T** ≤ 50.
1 ≤ **N** ≤ 50.

#### Large dataset

1 ≤ **T** ≤ 100.
1 ≤ **N** ≤ 10000.

### Sample

| input | output     |
| ----- | ---------- |
| 3     | Case #1: 3 |
| 3     | Case #2: 1 |
| 4     | Case #3: 2 |
| 5     |            |

### 分析

把问题抽象出来即求数字n最少可由数字的平方组成。

例如：`3 = 1*1 + 1*1 + 1*1`     `5 = 2*2 + 1*1`

使用dp计算所有的数字，然后直接输出答案dp[n]即可。

```C++
#include<iostream>
using namespace std;

int dp[10005];
int mmax=10005;

void init(){
	for(int i=0;i<mmax;i++){
		dp[i] = i;
	}
	for(int i=0;i<mmax;i++){
		int j=1;
		while(j*j+i<mmax){
			dp[j*j+i] = min(dp[j*j+i],dp[i]+1);
			j++;
		}
	}
}

int main(){
	freopen("D-large-practice.in","r",stdin);
	freopen("answer.out","w",stdout);
	int T;
	int n;
	cin>>T;
	int c=1;
	init();
	while(T--){
		cin>>n;
		cout<<"Case #"<<c<<": "<<dp[n]<<endl;
		c++;
	}
}
```




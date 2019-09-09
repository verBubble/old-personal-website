# Kickstart 2017 Round G

## Problem A  Huge Numbers

### Problem

Professor Shekhu has another problem for Akki today. He has given him three positive integers **A**, **N** and **P** and wants him to calculate the remainder when **AN!** is divided by **P**. As usual, **N!** denotes the product of the first **N** positive integers.

### Input

The first line of the input gives the number of test cases, **T**. **T** lines follow. Each line contains three integers **A**, **N** and **P**, as described above.

### Output

For each test case, output one line containing `Case #x: y`, where `x` is the test case number (starting from 1) and `y` is the answer.

### Limits

1 ≤ **T** ≤ 100.

#### Small dataset

1 ≤ **A** ≤ 10.
1 ≤ **N** ≤ 10.
1 ≤ **P** ≤ 10.

#### Large dataset

1 ≤ **A** ≤ 105.
1 ≤ **N** ≤ 105.
1 ≤ **P** ≤ 105.

###  Sample

| Input | output     |
| ----- | ---------- |
| 2     |            |
| 2 1 2 | Case #1: 0 |
| 3 3 2 | Case #2: 1 |

## 分析

使用快速幂算法即可，注意为了避免溢出，每一次幂操作之后都要求P的余数。

```C++
#include<iostream>
using namespace std;

typedef long long ll;

ll pow(ll a,int n,int p){
	ll res = 1;
	while(n){
		if(n&1) res=(res*a)%p;
		a=(a*a)%p;
		n>>=1;
	}
	return res;
}

int main(){
	freopen("A-large-practice.in","r",stdin); 
	freopen("answer.out","w",stdout);
	int T;
	ll a;
	int n,p;
	cin>>T;
	int j=0;
	while(T--){
		j++;
		cin>>a>>n>>p;
		for(int i=1;i<=n;i++){
			a=pow(a,i,p);
		}
		cout<<"Case #"<<j<<": "<<a<<endl;
	}
	return 0;
}
```

## Problem B Cards Game

### Problem

Professor Shekhu was a famous scientist working in the field of game theory in the early days of computer science. Right now, he's working on a game which involves a box containing **N** distinct cards. The i-th of these cards has a red number written on one side, and a blue number written on the other side. Both of these numbers are positive integers. The game proceeds as follows:

- The player starts with a total of 0 points. The objective of the game is to finish with the *lowest* possible total.
- As long as there are at least two cards remaining in the box, the player must repeat the following move:
  - Remove two cards of their choice from the box. Choose a red number **R**from one card and a blue number **B** from the other card.
  - Add the value **R ^ B** to the total, where **^** denotes bitwise XOR operation.
  - Return one of the two cards to the box, and remove the other from the game.
- The game ends when there is only one card remaining in the box (and so it is impossible to make another move).



Professor Shekhu has summoned his best student, Akki, to play this game. Can you help Akki find the minimum possible total, considering all possible ways in which he can play the game?

### Input

The first line of the input contains an integer **T**, the number of test cases. **T** test cases follow; each test case consists of three lines:
First line of the each test case will contain an integer **N**.

1. The first line contains a positive integer **N**: the number of cards in the box.
2. The second line contains a list of **N** positive integers **Ri**; the i-th of these represents the red number on the i-th card.
3. The third line contains a list of **N** positive integers **Bi**; the i-th of these represents the blue number on the i-th card.



### Output

For each test case, output one line containing `Case #x: y`, where `x` is the test case number (starting from 1) and `y` is the minimum possible total that Akki can attain, if he plays optimally.

### Limits

1 ≤ **T** ≤ 100.
1 ≤ **Ri** ≤ 109.
1 ≤ **Bi** ≤ 109.

#### Small dataset

2 ≤ **N** ≤ 5.

#### Large dataset

2 ≤ **N** ≤ 100.

### Sample

| input     | output     |
| --------- | ---------- |
| 2         | Case #1: 1 |
| 2         | Case #2: 5 |
| 1 2       |            |
| 3 3       |            |
| 1 101 501 |            |
| 3 2 3     |            |
|           |            |

## 分析：

注意解题的突破点是N张牌一共要操作n-1次，并且要使最后的得分最小，这里应该就联想到了最小生成树，用Kruskal算法就可以解决了。

```C++
#include<iostream>
#include<stdlib.h>
#include<algorithm>
using namespace std;

typedef long long ll;
ll sum;
int n,c;
int parents[110];
int red[110];
int blue[110];


struct edge{
	int u;
	int v;
	int w;
}edges[5100]; 

bool cmp(edge a,edge b){
	return a.w<b.w;
}

int Ufind(int x){
	if(parents[x]==x) return x;
	else{
		return parents[x] = Ufind( parents[x] );
	}
}

void setUnion(int v, int u){
	int x = Ufind(v);
	int y = Ufind(u);
	parents[x]=y;
	return;
}

void kruskal(){
	sum=0;
	int u,v;
	for(int i=0;i<c;i++){
		u=edges[i].u;
		v=edges[i].v;
		if(Ufind(u)!=Ufind(v)){
			sum+=edges[i].w;
			setUnion(v,u);
			
		}
	}
}

int main(){
	int T;
	freopen("B-large-practice.in","r",stdin);
	freopen("answer.out","w",stdout);
	cin>>T;
	int k=1;
	while(T--){
		sum=0;
		cin>>n;
		for(int i=0;i<n;i++){
			cin>>red[i];
		}
		for(int i=0;i<n;i++){
			cin>>blue[i];
		}
		c=0;
		for(int i=0;i<n;i++){
			for(int j=i+1;j<n;j++){
				edges[c].u=i;
				edges[c].v=j;
				edges[c++].w=min(red[i]^blue[j],red[j]^blue[i]);
			}
		}
		for(int i=0;i<=n;i++){
			parents[i]=i;
		}
		sort(edges,edges+c,cmp);
	//	cout<<c<<endl;
		//for(int i=0;i<n;i++){
		//	cout<<edges[i].w<<endl;
	//	}
		kruskal();
		cout<<"Case #"<<k<<": "<<sum<<endl;
		k++;
	}
	return 0;
}
```

## Problem C Matrix Cutting

### Problem

Prof Shekhu has a matrix of **N** rows and **M** columns where rows are numbered from 0 to **N-1** from top to bottom, and columns are numbered from 0 to **M-1** from left to right. Each cell in the matrix contains a positive integer.

He wants to cut this matrix into **N \* M** submatrices (each of size 1 * 1) by making horizontal and vertical cuts. A cut can be made only on the boundary between two rows or two columns.

Prof Shekhu invites his best student Akki for this job and makes an interesting proposition. Every time Akki makes a cut in a submatrix, before he makes the cut, he is awarded a number of coins equal to the minimum value in that submatrix. Note that with every cut, the total number of submatrices increases. Also, cuts in any two different submatrices are independent and likewise, Akki is awarded independently for the cuts in different submatrices.

Now, Akki has various ways in which he can make the cuts. Can you help him by maximizing the total number of coins he can gain?

### Input

The first line of the input contains an integer **T**, the number of test cases. **T** test cases follow. The first line of each test case contains two integers **N** and **M**, as described above.
Next, there are **N** lines of **M** positive integers each; these describe the matrix.

### Output

For each test case, output one line containing `Case #x: y`, where `x` is the test case number (starting from 1) and `y` is the maximum possible number of coins that Akki can be awarded, if he makes the cuts in optimal order.

### Limits

1 ≤ **T** ≤ 100.
1 ≤ each value in the matrix ≤ 105.

#### Small dataset

**N** = 1.
1 ≤ **M** ≤ 10.

#### Large dataset

1 ≤ **N** ≤ 40.
1 ≤ **M** ≤ 40.

### Sample

| input | output     |
| ----- | ---------- |
| 3     | Case #1: 5 |
| 2 2   | Case #2: 7 |
| 1 2   | Case #3: 1 |
| 3 4   |            |
| 2 3   |            |
| 1 2 1 |            |
| 2 3 2 |            |
| 1 2   |            |
| 1 2   |            |

## 分析：

记忆式搜索，也用了一点深度优先搜索的思想。调用的是f(1,1,n,m)，然后依次向下，调用更小的f。在遍历的时候，其实是遍历了每一种分割的可能性。

```C++
#include<iostream>
#include<memory.h>
using namespace std;

int n,m;
int num[45][45];
int dp[45][45][45][45];

//填充数组 以及初始化
void init(){
	cin>>n>>m;
	for(int i=1;i<=n;i++){
		for(int j=1;j<=m;j++){
			cin>>num[i][j];
		}
	}
	memset(dp,0,sizeof(dp));
}

int f(int a,int b,int c,int d){
	if(a==c && b==d) return 0;
	if(dp[a][b][c][d]>0) return dp[a][b][c][d];
	int ans=0;
	int mi=1e6;
	for(int i=a;i<=c;i++){
		for(int j=b;j<=d;j++){
			mi=min(mi,num[i][j]);
		}
	}     //寻找最小的值，代表当前要加的硬币数
	for(int i=a;i<c;i++){
		ans=max(ans,f(a,b,i,d)+f(i+1,b,c,d));
	}   //遍历横着切的每一种可能性
	for(int i=b;i<d;i++){
		ans=max(ans,f(a,b,c,i)+f(a,i+1,c,d));
	}  //遍历竖着切的每一种可能性
	dp[a][b][c][d] = ans+mi;  
	return ans+mi;	
}

int main(){
	freopen("C-large-practice.in","r",stdin);
	freopen("answer.out","w",stdout);
	int T;
	cin>>T;
	int c=1;
	while(T--){
		init();
		cout<<"Case #"<<c<<": "<<f(1,1,n,m)<<endl;
		c++;
	}
	return 0;
}
 
```


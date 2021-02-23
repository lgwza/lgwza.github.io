---
layout:     post
title:      CodeforcesRound704(Div. 2)(1492)题解
subtitle:   Codeforces 题解
date:       2021-02-24
author:     wza
header-img: img/post-bg-unix-linux.jpg
catalog: true
tags:
    - CF
---

# Codeforces Round #704 (Div. 2)

[比赛链接](https://codeforces.com/contest/1492)

## C. Maximum width

### 标签

子序列，贪心

### 题意

给定两个字符串 $s,t$，长度分别为 $n,m(m\leq n)$，保证 $t$ 是 $s$ 的一个子序列，求子序列 $t$ 在 $s$ 中两个字符的间隔的最大值是多少。

### 题解

用 $left_i$ 记录 $t$ 在 $s$ 中尽量靠左的位置，即 $t_i=s_{left_i}$ 且 $left_i$ 尽量小，用 $right_i$ 记录 $t$ 在 $s$ 中尽量靠右的位置，即 $t_i=s_{right_i}$ 且 $right_i$ 尽量大。最后取 $\max_{i=1}^{m-1}\{right_{i+1}-left_i\}$ 即可。

### 代码

```c++
#include<bits/stdc++.h>
using namespace std;
const int N=2e5+5;
int main(){
	ios_base::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	int n,m;
	string s,t;
	cin>>n>>m>>s>>t;
	vector<int>left(m+1),right(m+1);
	int cnt=0;
	for(int i=0;i<n;i++){
		if(s[i]==t[cnt]){
			left[cnt]=i;
			cnt++;
		}
		if(cnt==m) break;
	}
	cnt=m-1;
	for(int i=n-1;i>=0;i--){
		if(s[i]==t[cnt]){
			right[cnt]=i;
			cnt--;
		}
		if(cnt==-1) break;
	}
	int maxx=0;
	for(int i=0;i<m-1;i++){
		maxx=max(maxx,right[i+1]-left[i]);
	}
	cout<<maxx;
	return 0;
}
```

## D. Genius's Gambit

### 标签

构造，二进制

### 题意

给定 $3$ 个整数 $a,b,k(0\leq a;1\leq b;0\leq k\leq a+b\leq 2\cdot10^5)$，找到两个二进制数 $x,y(x\geq y)$ 满足：$x,y$ 都含有 $a$ 个 $0$ 和 $b$ 个 $1$，$x-y$ 恰有 $k$ 个 $1$。

### 题解

构造。先固定 $x$ 为 $11\cdots 1100\cdots 00$ 的形式，然后将 $y$ 从 $x$ 开始变化。首先可以发现，将前缀连续 $1$ 的最后一个每往右移一位，$x-y$ 中 $1$ 的数量就会 $+1$，即很容易构造出 $k\leq a$ 的情况。$k>a$ 之后，每次将连续前缀 $1$ 的最后一个往后移一位 $k$ 即 $+1$，由此当 $k\in [0,a+b-2]$ 时，可以构造出答案来。注意边界情况，例如 $a=0,b=1,k=0$ 等情况。

### 代码

```c++
#include<bits/stdc++.h>
using namespace std;
int main(){
	ios_base::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);
	int a,b,k;
	cin>>a>>b>>k;
	if(b==1&&k!=0||k>a+b-2&&k!=0||a==0&&k!=0){
		cout<<"No";
		return 0;
	}
	cout<<"Yes\n";
	if(a==0){
		for(int i=1;i<=2;i++){
			for(int j=1;j<=b;j++) cout<<'1';
			cout<<'\n';
		}
		return 0;
	}
	if(b==1){
		for(int i=1;i<=2;i++){
			cout<<'1';
			for(int j=1;j<=a;j++) cout<<'0';
			cout<<'\n';
		}
		return 0;
	}
	if(k>a){
		for(int i=1;i<=b;i++) cout<<'1';
		for(int i=1;i<=a;i++) cout<<'0';
		cout<<'\n';
	}
	else{
		for(int i=1;i<=b-1;i++) cout<<'1';
		for(int i=1;i<=a-k;i++) cout<<'0';
		cout<<'1';
		for(int i=1;i<=k;i++) cout<<'0';
		cout<<'\n';
	}
	if(k<=a){
		for(int i=1;i<=b-1;i++) cout<<'1';
		for(int i=1;i<=a;i++) cout<<'0';
		cout<<'1';
	}
	else{
		cout<<'1';
		for(int i=1;i<=a+b-2-k;i++) cout<<'1';
		cout<<'0';
		for(int i=1;i<=k-a;i++) cout<<'1';
		for(int i=1;i<=a-1;i++) cout<<'0';
		cout<<'1';
	}
	return 0;
}
```


---
title: "다이나믹 프로그래밍"
excerpt: "Dynamic Programming"

categories:
- Algorithm

tags:
- Introduction To Algorithms
---

Dynamic Programming
---

> 개념

- 동적 프로그래밍(Dynamic Programming)

  - 수학과 컴퓨터 과학, 그리고 경제학에서 동적 계획법이란 복잡한 문제를 간단한 여러 개의 문제로 나누어 푸는 방법을 말한다. 부분 문제 반복과 최적 부분 구조를 가지고 있는 알고리즘을 일반적인 방법에 비해 더욱 적은 시간 내에 풀 때 사용한다.
  - 동적 계획법의 원리는 매우 간단하다. 일반적으로 주어진 문제를 풀기 위해서, 문제를 여러 개의 하위 문제(subproblem)로 나누어 푼 다음, 그것을 결합해 최종적인 목적에 도달하는 것이다. 각 하위 문제의 해결을 계산한 뒤, 그 해결책을 저장해 후에 같은 하위 문제가 나왔을 경우 그것을 간단하게 해결할 수 있다. 이러한 방법으로 동적 계획법은 계산 횟수를 줄일 수 있다. 특히 이 방법은 하위 문제의 수가 기하급수적으로 증가할 때 유용하다.

  

> 응용

***[정리가 잘 된 블로그 링크](https://www.zerocho.com/category/Algorithm/post/584b979a580277001862f182)***

1. 막대 자르기(Rod Cutting)
2. 최장 공통 부분 수열(LCS)
3. 0/1 배낭 문제

​    

> 실전 문제

[BOJ 11052 - 카드 구매하기](https://www.acmicpc.net/problem/11052)

​    

> 해결

```c++
#include <iostream>
#include <vector>

using namespace std;

#define MAX(a,b) (a > b ? a : b)

int main()
{
	// sync off
	ios::sync_with_stdio(false);
	cin.tie(0);
	cout.tie(0);
	// ----------

	vector<int> cardPackPrice;
	vector<int> dp;
	int N, P;

	cin >> N;

	cardPackPrice.resize(N + 1);
	dp.resize(N + 1);

	for (int i = 1; i <= N; i++)
	{
		cin >> cardPackPrice[i];
	}

	// 1. 1, 2, ... n개를 가질 때 최대의 값을 구한다.
	dp[0] = 0;
	dp[1] = cardPackPrice[1];
	for (int i = 2; i <= N; i++)
	{
		int max = 0;
		for (int j = 1; j <= i; j++)
		{
			max = MAX(max, cardPackPrice[j] + dp[i - j]);
		}
		dp[i] = max;
	}

	cout << dp[N];

	return 0;
}
```


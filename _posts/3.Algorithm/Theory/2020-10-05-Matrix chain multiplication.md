---
title: "연쇄 행렬 곱셈(Matrix chain multiplication)"
excerpt: "Dynamic Programming"

categories:
- Algorithm

tags:
- Introduction To Algorithms
---

Dynamic Programming
---

Matrix chain multiplication
---

> 개념

- 연쇄 행렬 곱셈(Matrix chain multiplication)

  - 연쇄 행렬 곱셈의 최적화 문제를 동적 계획법(Dynamic Programming)을 이용해 해결한다. 주어진 행렬들의 곱을 최소의 연산으로 수행하는 최소 횟수를 구하는 알고리즘이다.
  - 행렬의 순서가 주어질 때 행렬의 곱셈에서 가장 효율적인 방법을 찾는 것이 목표다. 실제로는 곱셈을 수행하는 것이 아닌 행렬의 곱셈 **순서**를 결정하는 것이다.
  - **Brute Force**방식으로 모든 경우를 확인해 해결할 수도 있지만 행렬이 커질수록 느리고 비효율적이다.
  

​    

> 구현

- 연쇄 행렬 곱셈의 구현 핵심은 **부분 수열(subsequence)**을 이용하는 것이다.

  1. 전체 행렬을 2개의 부분 수열로 분리한다.
  2. 각 부분 수열의 최소 비용을 구한 후 합한다.
  3. 분리할 수 있는만큼 부분수열의 길이를 늘리면서 이 과정을 반복한다.

- 점화식은 다음과 같다.

  ```
  matrix[N][2] : 행렬
  DP[i][j]     : 행렬 i번에서 j번까지의 최소 비용
  => DP[i][j] = min(DP[i][j], DP[i][k] + DP[k + 1][j] + matrix[i][0] * matrix[k][1] * matrix[j][1])
  ```

​    

> 실전 문제

[BOJ 11049 - 행렬 곱셈 순서](https://www.acmicpc.net/problem/11049)

​    

> 해결

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <limits.h>

using namespace std;

void MakeMatrixChainMultiplication(const vector<pair<int, int>> &v, vector<vector<int>> &dp)
{
    int vectorSize = v.size();

    for (int i = 0; i < vectorSize; i++)
    {
        for (int j = 0; j < vectorSize - i; j++)
        {
            if (i == 0)
            {
                dp[j][j] = 0;
            }
            else
            {
                dp[j][j + i] = INT_MAX;

                for (int t = j; t < j + i; t++)
                {
                    dp[j][j + i] = min(dp[j][j + i], dp[j][t] + dp[t + 1][j + i] + (v[j].first * v[t].second * v[j + i].second));
                }
            }
        }
    }
}

int GetMinimumMultiplicationCount(const vector<vector<int>> &dp)
{
    return dp[0][dp.size() - 1];
}

int main()
{
    // sync off
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    // sync off

    // pair<r, c>
    vector<pair<int, int>> v;
    vector<vector<int>> dp;
    int N;

    cin >> N;

    for (int i = 0; i < N; i++)
    {
        int r, c;

        cin >> r >> c;

        v.push_back(make_pair(r, c));
    }

    dp.resize(N);
    for (int i = 0; i < N; i++)
    {
        dp[i].resize(N);
    }

    MakeMatrixChainMultiplication(v, dp);

    cout << GetMinimumMultiplicationCount(dp);

    return 0;
}
```


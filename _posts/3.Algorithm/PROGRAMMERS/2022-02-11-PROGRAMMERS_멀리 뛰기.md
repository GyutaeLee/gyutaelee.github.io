---
title: "PROGRAMMERS 프로그래머스 : 멀리 뛰기"
excerpt: "다이나믹 프로그래밍(Dynamic Programming)"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

   이번 문제는 **다이나믹 프로그래밍(Dynamic Programming)** 문제이다.

[PROGRAMMERS : 멀리 뛰기](https://programmers.co.kr/learn/courses/30/lessons/12914)    

​        

> 문제 아이디어

​	이 문제는 다이나믹 프로그래밍 문제이다. 규칙은 다음과 같다.    
1. 1칸을 뛰는 경우 : 1
2. 2칸을 뛰는 경우 : 2
3. 3칸을 뛰는 경우 : [1칸을 뛰고, 2칸을 뛰는 경우] + [2칸을 뛰고, 1칸을 뛰는 경우]
4. 위 규칙을 점화식으로 하면 dp[n] = dp[n-2] + dp[n-1]

​    

>해결

```c#
using System;
using System.Collections.Generic;

public class Solution
{
    public long solution(int n)
    {
        List<long> dp = new List<long>(new long[2000]);

        dp[0] = 1;
        dp[1] = 2;

        for (int i = 2; i < n; i++)
        {
            dp[i] = dp[i - 1] % 1234567 + dp[i - 2] % 1234567;
        }

        long answer = dp[n - 1] % 1234567;
        return answer;
    }
}
```

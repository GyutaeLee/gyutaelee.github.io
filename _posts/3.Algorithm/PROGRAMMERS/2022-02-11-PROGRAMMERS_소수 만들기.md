---
title: "PROGRAMMERS 프로그래머스 : 소수 만들기"
excerpt: "구현"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

   이번 문제는 **구현** 문제이다.

[PROGRAMMERS : 소수 만들기](https://programmers.co.kr/learn/courses/30/lessons/12977)    

​        

> 문제 아이디어

​	이 문제는 적힌 내용을 그대로 구현하면 되는 문제이다. 방법은 다음과 같다.    
1.  소수를 판단할 때마다 알고리즘을 돌리면 시간 낭비가 심하므로, 미리 리스트에 소수들을 담아둔다.
2.  Combination을 이용해서 nums 중에 3개씩 뽑아서 더한 후, 소수 판단을 한다. (이때 3중 for 문을 이용해도 무방)

​    

>해결

```c#
using System;
using System.Collections.Generic;

public class Solution
{
    List<int> CreatePrimeNumberList(int startNumber, int endNubmer)
    {
        List<int> primeNumberList = new List<int>();

        for (int i = startNumber; i <= endNubmer; i++)
        {
            primeNumberList.Add(i);
        }

        for (int factor = startNumber; factor <= endNubmer; factor++)
        {
            primeNumberList.RemoveAll(delegate (int x) { return x > factor && x % factor == 0; });
        }

        return primeNumberList;
    }

    public void Combination(ref int answer, List<int> primeNumberList, int[] nums, int[] arr, int index, int n, int r, int current)
    {
        if (r == 0)
        {
            int sumOfNumbers = nums[arr[0]] + nums[arr[1]] + nums[arr[2]];
            int findIndex = primeNumberList.FindIndex(x => x == sumOfNumbers);
            if (findIndex != -1)
            {
                answer++;
            }
        }
        else if (n == current)
        {
            return;
        }
        else
        {
            arr[index] = current;
            Combination(ref answer, primeNumberList, nums, arr, index + 1, n, r - 1, current + 1);
            Combination(ref answer, primeNumberList, nums, arr, index, n, r, current + 1);
        }
    }

    public int solution(int[] nums)
    {
        List<int> primeNumbeList = CreatePrimeNumberList(2, 3000);                
        int answer = 0;
        int []arr = new int[3];
        Combination(ref answer, primeNumbeList, nums, arr, 0, nums.Length, 3, 0);

        return answer;
    }
}
```

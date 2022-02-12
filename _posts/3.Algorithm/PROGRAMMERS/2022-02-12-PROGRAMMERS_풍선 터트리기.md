---
title: "PROGRAMMERS 프로그래머스 : 풍선 터트리기"
excerpt: "구현"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

   이번 문제는 **구현** 문제이다.

[PROGRAMMERS : 풍선 터트리기](https://programmers.co.kr/learn/courses/30/lessons/68646)    

​        

> 문제 아이디어

​	이 문제는 적힌 내용을 그대로 구현하면 되는 문제이다. 하지만 바로 아이디어가 생각나지는 않고, 골똘히 생각해야 아이디어가 떠오른다... 방법은 다음과 같다.

​	**조건**에 주목한다. 인접한 두 풍선 중 **번호가 더 작은 풍선**만을 터뜨릴 수 있다. 이 말은 바꿔 말하면, **기준이 되는 풍선보다 좌/우측에, 기준이 되는 풍선보다 큰 값을 가진 풍선이 하나라도 있으면** 이 풍선이 남을 수 있다는 것이다. 이걸 한 번 더 바꿔 말하면, **기준이 되는 풍선보다  좌/우측에, 기준이 되는 풍선보다 작은 값을 가지는 풍선이 모두 있으면** 이 풍선은 무조건 터져야 한다(남길 수 없다).

​	문제를 위와 같이 해석하고, 기준이 되는 풍선의 좌/우측 최소값을 갱신해주면서 문제를 해결하면 된다.

​    

>해결

```c#
using System;
using System.Linq;
using System.Collections.Generic;

public class Solution {
    public int solution(int[] a)
        {
            int leftMinimumValue = int.MaxValue;
            int rightMinimumValue = int.MaxValue;
            SortedSet<int> rightValueSet = new SortedSet<int>();

            for (int i = 0; i < a.Length; i++)
            {
                rightMinimumValue = (rightMinimumValue > a[i]) ? a[i] : rightMinimumValue;
                rightValueSet.Add(a[i]);
            }

            int answer = 0;
            for (int i = 0; i < a.Length; i++)
            {
                int currentValue = a[i];
                rightValueSet.Remove(currentValue);

                if (i == a.Length - 1)
                {
                    rightMinimumValue = int.MaxValue;
                }
                else if (currentValue == rightMinimumValue)
                {
                    rightMinimumValue = rightValueSet.ElementAt(0);
                }

                if (leftMinimumValue >= a[i] || rightMinimumValue >= a[i])
                {
                    answer++;
                }

                leftMinimumValue = (leftMinimumValue > a[i]) ? a[i] : leftMinimumValue;
            }

            return answer;
        }
}
```

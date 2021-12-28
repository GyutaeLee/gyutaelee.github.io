---
title: "PROGRAMMERS 프로그래머스 : 외벽 점검"
excerpt: "완전 탐색"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **완전 탐색** 문제이다.

[PROGRAMMERS : 완전 탐색](https://programmers.co.kr/learn/courses/30/lessons/60062)    

​        

> 문제 아이디어

​	이 문제는 최소한의 인원으로 외벽의 **weak **지점을 모두 탐색해야 한다. 출발점이 따로 제한되어 있지 않으므로, **weak** 의 위치에서 탐색을 시작하는게 가장 이득이다. 여기서 문제의 조건을 보자.

- **n** : 1~200 이하인 자연수
- **weak** : 1이상 15이하의 길이
- **dist** : 1이상 8이하의 자연수

어차피 모든 점을 도는게 아니라, **weak**의 각 점에서 출발할 것이므로  조건들 중에 **weak**와 **dist**를 주목해야 한다. 만약 모든 경우의 수를 확인한다고 했을때, 

- **최악의 경우 :** (**dist** 배열의 순열)  * (**weak** 배열의 size) = 8! * 15 = **604,800**

으로 모든 경우를 탐색해도 경우의 수가 매우 적으므로 **완전 탐색**을 사용해서 해결해도 무방하다.

​    

>해결

```c++
#include <string>
#include <vector>
#include <algorithm>
#include <limits.h>

using namespace std;

int solution(int n, vector<int> weak, vector<int> dist)
{
    int answer = INT_MAX;

    do
    {
        for (int weakStartIndex = 0; weakStartIndex < weak.size(); weakStartIndex++)
        {
            /*
            * ClockWise
            */
            bool isClockWiseEnd = false;
            int clockWiseCurrentWall = weak[weakStartIndex];
            int clockWiseCurrentWeakIndex = weakStartIndex;
            int clockWiseCheckCount = 0, clockWiseFriendCount = 0;

            /*
            * CounterClockWise
            */
            bool isCounterClockWiseEnd = false;
            int counterClockWiseCurrentWall = weak[weakStartIndex];
            int counterClockWiseCurrentWeakIndex = weakStartIndex;
            int counterClockWiseCheckCount = 0, counterClockWiseFriendCount = 0;
            for (vector<int>::iterator iter = dist.begin(); iter != dist.end(); ++iter)
            {
                if (isClockWiseEnd == false)
                {
                    clockWiseFriendCount++;
                }

                if (isCounterClockWiseEnd == false)
                {
                    counterClockWiseFriendCount++;
                }

                for (int i = 0; i <= *iter; i++)
                {
                    /*
                    * ClockWise
                    */
                    if (isClockWiseEnd == false)
                    {
                        if (weak[clockWiseCurrentWeakIndex] == clockWiseCurrentWall)
                        {
                            clockWiseCheckCount++;
                            if (clockWiseCheckCount == weak.size())
                            {
                                isClockWiseEnd = true;
                            }

                            clockWiseCurrentWeakIndex++;
                            if (clockWiseCurrentWeakIndex == weak.size())
                            {
                                clockWiseCurrentWeakIndex = 0;
                            }
                        }
                        clockWiseCurrentWall++;
                        if (clockWiseCurrentWall == n)
                        {
                            clockWiseCurrentWall = 0;
                        }
                    }
                    
                    /*
                    * CounterClockWise
                    */
                    if (isCounterClockWiseEnd == false)
                    {
                        if (weak[counterClockWiseCurrentWeakIndex] == counterClockWiseCurrentWall)
                        {
                            counterClockWiseCheckCount++;
                            if (counterClockWiseCheckCount == weak.size())
                            {
                                isCounterClockWiseEnd = true;
                            }

                            counterClockWiseCurrentWeakIndex--;
                            if (counterClockWiseCurrentWeakIndex == -1)
                            {
                                counterClockWiseCurrentWeakIndex = weak.size() - 1;
                            }
                        }

                        counterClockWiseCurrentWall--;
                        if (counterClockWiseCurrentWall == -1)
                        {
                            counterClockWiseCurrentWall = n - 1;
                        }
                    }
                }

                if (isClockWiseEnd == true)
                {
                    answer = min(answer, clockWiseFriendCount);
                }

                if (isCounterClockWiseEnd == true)
                {
                    answer = min(answer, counterClockWiseFriendCount);
                }

                if (isClockWiseEnd == true && isCounterClockWiseEnd == true)
                    break;

                clockWiseCurrentWall = weak[clockWiseCurrentWeakIndex];        
                counterClockWiseCurrentWall = weak[counterClockWiseCurrentWeakIndex];
            }
        }
    } while (next_permutation(dist.begin(), dist.end()));

    if (answer == INT_MAX)
    {
        answer = -1;
    }

    return answer;
}
```

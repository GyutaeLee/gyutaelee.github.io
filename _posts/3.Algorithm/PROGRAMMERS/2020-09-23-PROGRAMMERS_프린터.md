---
title: "PROGRAMMERS 프로그래머스 : 프린터"
excerpt: "큐(queue)"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **큐(QUEUE)** 문제이다.

[PROGRAMMERS : 프린터](https://programmers.co.kr/learn/courses/30/lessons/42587)    

​    

> 문제 이해

   문제를 읽어보자. 먼저 이 문제에서 나오는 변수는 총 2가지이다.

- vector<int> priorities : 우선 순위 
- location : 찾아야하는 인쇄물의 인덱스

​    

> 문제 아이디어

​	이 문제의 핵심은 대기목록의 최대 문서 개수가 100개이고, 우선순위가 1~9까지밖에 없다는 점이다. 1~9까지를 다 쓰고, 100번째에 인쇄가 된다는 가정을 하고 큐에 push & pop 작업을 반복해도 100 + 90 + ... + 10 = 550번밖에 연산하지 않는다. 우직하게 조건만 맞추어서 큐를 돌려주면 되는 문제다.

​    

>해결

```c++
#include <string>
#include <vector>
#include <queue>

using namespace std;

// currentMaxPriority 보다 작은 우선순위 중에 유효한 가장 큰 우선순위를 찾는다
int FindNextMaxPriority(const vector<int>& priorityCount, int currentMaxPriority)
{
    int priority = 0;

    if (currentMaxPriority == 0)
        return priority;

    for (int i = currentMaxPriority - 1; i > 0; i--)
    {
        if (priorityCount[i] != 0)
        {
            priority = i;
            break;
        }
    }

    return priority;
}

int solution(vector<int> priorities, int location)
{
    int answer = 1;
    int currentMaxPriority = 0;
    vector<int> priorityCount(10);

    // <location, priority>
    queue<pair<int, int>> q;

    for (int i = 0; i < priorities.size(); i++)
    {
        // 1. queue에 위치 값 인덱스와 우선순위를 넣는다.
        q.push(make_pair(i, priorities[i]));

        // 2. 각 우선순위가 몇개씩 들어있는지 센다.
        priorityCount[priorities[i]]++;

        // 3. 가장 높은 우선순위를 갱신한다.
        currentMaxPriority = (priorities[i] > currentMaxPriority) ? priorities[i] : currentMaxPriority;
    }

    while (q.empty() == false)
    {
        pair<int, int> value = q.front(); q.pop();

        // 4. 현재 숫자가 가장 높은 우선 순위이면 작업을 진행하고,
        if (value.second == currentMaxPriority)
        {
            // 4-1. 찾고 있는 인덱스 값이면 그만한다.
            if (value.first == location)
                break;

            // 4-2. 우선순위 수를 줄여주고, 인쇄 카운트를 늘린다.
            priorityCount[value.second]--;
            answer++;

            // 4-3. 현재 가장 높은 우선순위를 모두 인쇄했으면 다음 가장 높은 우선순위를 찾는다.
            if (priorityCount[value.second] == 0)
            {
                currentMaxPriority = FindNextMaxPriority(priorityCount, currentMaxPriority);
            }
        }
        // 5. 아니라면 다시 큐에 넣는다.
        else
        {
            q.push(value);
        }
    }

    return answer;
}
```

---
title: "PROGRAMMERS 프로그래머스 : 더 맵게"
excerpt: "힙(Heap)"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **힙(Heap)** 문제이다.

[PROGRAMMERS : 더 맵게](https://programmers.co.kr/learn/courses/30/lessons/42626)    

​    

> 문제 이해

   문제를 읽어보자. 먼저 이 문제에서 나오는 변수는 총 2가지이다.

- vector<int> scoville : 음식의 스코빌 지수
- K : 기준이 되는 스코빌 지수

​    

> 문제 아이디어

​	이 문제의 핵심은 **반복해서** 가장 맵지 않은 음식 1,2번째를 사용한다는 점이다. 항상 정렬을 하거나, 가장 맵지 않은 음식을 찾는 것은 비효율적이기 때문에 삽입할 때 정렬을 하는 우선순위 큐를 사용한다.

1. 우선순위 큐에 음식의 스코빌 지수를 모두 넣는다.
2. 음식을 조정해가면서 스코빌 지수를 확인한다.
3. 가장 낮은 수치의 스코빌 지수를 가진 음식이 스코빌 지수를 넘으면 조건 완료.

​	주의해야 할 점은 1번째로 맵지 않은 음식을 빼고, 2번째로 맵지 않은 음식을 뺄때도 큐가 비었는지에 대한 검사를 해야한다.

​    

>해결

```c++
#include <string>
#include <vector>
#include <queue>

using namespace std;

int solution(vector<int> scoville, int K)
{
    int answer = 0;
    priority_queue<int, vector<int>, greater<int>> pq;

    // 1. 우선순위 큐에 음식의 스코빌 지수를 모두 넣는다.
    for (int i = 0; i < scoville.size(); i++)
    {
        pq.push(scoville[i]);
    }

    // 2. 음식의 스코빌 지수를 조정해가면서 스코빌 지수를 확인한다.
    while (pq.empty() == false)
    {
        // 가장 맵지 않은 음식의 스코빌 지수
        int foodScoville01 = pq.top(); pq.pop();

        // 3. 가장 낮은 수치의 스코빌 지수를 가진 음식이 스코빌 지수를 넘으면 조건 완료.
        if (foodScoville01 >= K)
            break;

        // 4. 모든 음식의 스코빌 지수를 K 이상으로 만들 수 없다.
        if (pq.empty() == true)
        {
            answer = -1;
            break;
        }

        // 두 번째로 맵지 않은 음식의 스코빌 지수
        int foodScoville02 = pq.top(); pq.pop();

        // 섞음 음식의 스코빌 지수
        int foodScoville = foodScoville01 + (foodScoville02 * 2);

        // 섞은 횟수를 올린다.
        answer++;

        // 우선순위 큐에 넣어준다.
        pq.push(foodScoville);
    }

    return answer;
}
```

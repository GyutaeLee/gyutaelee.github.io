---
title: "PROGRAMMERS 프로그래머스 : 무지의 먹방 라이브"
excerpt: "우선순위 큐(Priority queue)"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **우선순위 큐(Priority queue)** 문제이다.

[PROGRAMMERS : 무지의 먹방 라이브](https://programmers.co.kr/learn/courses/30/lessons/42891)    

​    

> 문제 이해

   이 문제는 지문이 많이 길다. 그래서 그림과 함께 잘 읽어야 한다. 따로 설명하는거보다는 지문을 잘 읽고 들어가자.

​    

> 문제 아이디어

​	그냥 간단히 문제를 읽으면 브루트 포스로 무식하게 풀면 될거 같은 느낌이 든다. 하지만 그렇게 풀면 **효율성 테스트 제한 사항**에서 원소의 개수가 100,000,000개 & k : 2x10^13 에서 시간 초과가 나기 때문에 안 된다.

​	아이디어는 다음과 같다.

1. 모든 음식들을 먹는 시간을 기준, 오름차순으로 정렬(우선순위 큐)해준다.
2. 우선순위 큐에서 가장 앞에 있는 **음식의 먹는 시간 x 남은 음식의 개수**만큼 소요 시간을 소모시킨다.
   - 다 먹은 음식은 우선순위 큐에서 빼준다.
   - **음식을 다 먹고도 시간이 남으면 -1을 바로 출력한다.**
3. **2번** 과정을 남은 시간으로 우선순위 큐에서 가장 앞에 있는 음식을 다 먹지 못할 때까지 반복한다.
4. 남은 음식들을 우선순위 큐에서 빼면서, 원래 순서대로 돌려놓는다.
5. 가장 앞(0번 인덱스)에서 남은 시간만큼 움직인 후에 답을 구한다.

​    

>해결

```c++
#include <iostream>
#include <vector>
#include <queue>
#include <algorithm>

using namespace std;

int solution(vector<int> food_times, long long k)
{
    priority_queue<pair<int, int>, vector<pair<int, int> >, greater<pair<int, int> > > pq;
    long long sum = 0;
    unsigned int itemSize = food_times.size();
    for (int i = 0; i < itemSize; i++)
    {
        pq.push(make_pair(food_times[i], i + 1));
        sum += food_times[i];
    }

    if (sum <= k)
    {
        return -1;
    }

    int before = 0;
    long long remainTime = k;
    while ((pq.top().first - before) * pq.size() <= remainTime)
    {
        remainTime -= (pq.top().first - before) * pq.size();
        before = pq.top().first;
        pq.pop();
    }

    vector<pair<int, int>> remainFoods;
    while (pq.empty() == false)
    {
        remainFoods.push_back(make_pair(pq.top().second, pq.top().first));
        pq.pop();
    }
    sort(remainFoods.begin(), remainFoods.end());

    return remainFoods[remainTime % remainFoods.size()].first;
}
```

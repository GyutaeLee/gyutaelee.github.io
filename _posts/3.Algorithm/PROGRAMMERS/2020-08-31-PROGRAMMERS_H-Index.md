---
title: "PROGRAMMERS 프로그래머스 : H-Index"
excerpt: "정렬(Sort)"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **정렬(Sort)** 문제이다.

[PROGRAMMERS : H-Index](https://programmers.co.kr/learn/courses/30/lessons/42747)    

​    

> 문제 이해

   문제를 읽어보자. 먼저 이 문제에서 나오는 변수는 총 1가지이다.

- citations : 과학자가 발표한 논문들의 인용 횟수

​    

> 문제 아이디어

　H-Index의 핵심은 **인용 횟수**만 보면 된다는 것이다.

1. 인용 횟수를 내림차순으로 정렬한 후
2. **h번 이상 인용된 논문이 h편 이상인지**를 확인한다. (**이상**이므로 이때 h번 인용된 논문들도 포함한다)
3. 주의할 점 : H-Index는 citations 내부에 없는 값일 수 있다.
   - 예) [5, 5, 5, 5] 의 답은 4이다. 4번 이상 인용된 논문이 4편 이상이고, 나머지 논문은 없기 때문.

​	이 문제는 3. 번 사항만 주의하면 쉽게 풀 수 있다. 내 생각에는 문제 설명이 너무 부실했다. 생소한 H-Index의 개념이기 때문에 문제에 **답은 citations 내부에 없는 값일 수도 있다**를 직접적으로든, 돌려서든 명시해주었으면 어떨까 한다.

​    

>해결

```c++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

int GetOverNumberCount(vector<int>& v, int value)
{
    for (int i = 0; i < v.size(); i++)
    {
        if (v[i] < value)
        {
            return i;
        }
    }

    return v.size();
}

int solution(vector<int> citations)
{
    sort(citations.begin(), citations.end(), greater<int>());

    int hIndex = 1000;

    while (hIndex >= 1)
    {
        int overNumberCount = GetOverNumberCount(citations, hIndex);

        if (overNumberCount >= hIndex)
        {
            return hIndex;
        }

        hIndex--;
    }

    return hIndex;
}
```

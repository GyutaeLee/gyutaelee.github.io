---
title: "PROGRAMMERS 프로그래머스 : 타겟 넘버"
excerpt: "깊이/너비 우선 탐색(DFS/BFS)"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **깊이/너비 우선 탐색(DFS/BFS)** 문제이다.

[PROGRAMMERS : 타겟 넘버](https://programmers.co.kr/learn/courses/30/lessons/43165)    

​    

> 문제 이해

   문제를 읽어보자. 먼저 이 문제에서 나오는 변수는 총 2가지이다.

- vector<int>  numbers : 사용할 수 있는 숫자
- target : 결과적으로 만들어야 하는 숫자

​    

> 문제 아이디어

​	이 문제의 핵심은 모든 경우의 수를 다 해봐야한다는 점이다. 주어진 숫자의 개수가 최대 20개이고, 더하거나 빼는 연산만 하기 때문에 최대 연산 수는 2^20 = 1048576이다. 그러므로 BFS를 사용해서 더하고 빼는 경우를 모두 해보면 된다.

​	주의해야 할 점은 연산 도중에 타겟 넘버가 나오는 것은 무시해야한다. 문제에 써져있지는 않지만 테스트 케이스를 보면 예상할 수 있다. **(문제에 써져있으면 좋을텐데)**

​    

>해결

```c++
#include <string>
#include <vector>
#include <queue>

using namespace std;

int solution(vector<int> numbers, int target)
{
    int answer = 0;
    queue<pair<int, int>> q; // number, depth

    // 1. 초기값 대입
    q.push(make_pair(numbers[0],0));
    q.push(make_pair(-numbers[0], 0));

    while (q.empty() == false)
    {
        int number = q.front().first;
        int depth = q.front().second;
        
        q.pop();
                        
        // 모든 경우의 수를 다 했으면 그만한다.
        if (depth == numbers.size() - 1)
        {
            // 타겟 넘버가 만들어지면 카운트를 올려준다.
            if (number == target)
            {
                answer++;
            }

            continue;
        }

        // 2. 다음 경우의 수를 모두 큐에 넣어준다.
        depth++;

        // 2-1. 더하기
        q.push(make_pair(number + numbers[depth], depth));
        
        // 2-2. 빼기
        q.push(make_pair(number - numbers[depth], depth));
    }

    return answer;
}
```

---
title: "PROGRAMMERS 프로그래머스 : 네트워크"
excerpt: "깊이/너비 우선 탐색(DFS/BFS)"

categories:
- Algorithm

tags:
- BOJ
---

> 시작하면서

　이번 문제는 **깊이/너비 우선 탐색(DFS/BFS)** 문제이다.

[PROGRAMMERS : 네트워크](https://programmers.co.kr/learn/courses/30/lessons/43162)    

​    

> 문제 이해

   문제를 읽어보자. 먼저 이 문제에서 나오는 변수는 총 1가지이다.

- vector<vector<int>> computers : 각 컴퓨터들의 연결 상태

​    

> 문제 아이디어

　모든 노드 들을 하나씩 확인하면서 다른 컴퓨터들과의 연결 상태를 확인하는 것은 비효율적이다. 그렇기에 특정 컴퓨터에서 다른 연결된 컴퓨터들을 확인하면서 이미 확인한 컴퓨터는 재확인하지 않도록 해주어야 한다.

​    

>해결

```c++
#include <string.h>
#include <vector>
#include <queue>

using namespace std;

int solution(int n, vector<vector<int>> computers)
{
    queue<int> queue;
    int answer = 0;
    bool* bVisited;

    bVisited = new bool[n];
    memset(bVisited, false, sizeof(bool) * n);

    // 초기값 삽입
    queue.push(0);
    bVisited[0] = true;

    while (queue.empty() == false)
    {
        int computerIndex = queue.front(); queue.pop();

        // 현재 컴퓨터 인덱스와 연결되어 있고, 방문한적 없는 컴퓨터들을 큐에 넣어줌
        for (int i = 0; i < n; i++)
        {
            if (computers[computerIndex][i] == 1 && bVisited[i] == false)
            {
                bVisited[i] = true;
                queue.push(i);
            }
        }

        // 큐가 비어있는지 검사함
        if (queue.empty() == true)
        {
            // 큐가 비어있다면 네트워크 하나를 다 돌았다는 것을 기록
            answer++;

            // 나머지 컴퓨터 중에 확인하지 않은 컴퓨터가 있는지 체크
            for (int i = 0; i < n; i++)
            {
                // 확인하지 않은 컴퓨터는 큐에 넣어서 작업을 진행함
                if (bVisited[i] == false)
                {
                    queue.push(i);
                    bVisited[i] = true;
                    break;
                }
            }
        }
    }

    delete[] bVisited;

    return answer;
}
```

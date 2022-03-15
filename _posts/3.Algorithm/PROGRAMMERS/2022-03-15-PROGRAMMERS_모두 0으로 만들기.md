---
title: "PROGRAMMERS 프로그래머스 : 모두 0으로 만들기"
excerpt: "깊이 우선 탐색(DFS)"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

   이번 문제는 **깊이 우선 탐색(DFS)** 문제이다.

[PROGRAMMERS : 모두 0으로 만들기](https://programmers.co.kr/learn/courses/30/lessons/76503)    

​        

> 문제 아이디어

​	이 문제를 처음 봤을 때는 BFS를 사용해야할 것 같았다. 그 이유는 정점의 개수인 a의 크기가 최대 30만이라고 나와있기 때문에 DFS를 사용하면 스택 오버 플로우가 발생할 것 같은 느낌이 들었기 때문이다.

 그런데 문제를 읽으면서 생각을 했는데 리프 노드에서 탐색이 시작되어야 해서 BFS로 풀기에는 
 다소 난해한 감이 있어서 DFS로 문제를 풀어봤더니 오버 플로우가 발생하지 않았다... 우선 풀이는 다음과 같다.    

 노드의 값을 옮기는 행위를 리프 노드부터 지속해서 움직인다고 생각하면,

- 각 노드의 값을 옮기면서 계속 더해주면, 합쳐진 후에 옮겨야 할 값이 각 노드에 남는다.
- 각 노드의 절대값이 옮기는 비용이다.

 문제 풀이는 다음과 같다.

1. 아무 노드나 루트 노드로 잡는다.
   - 트리 구조 특성 상 어느 노드를 잡던간에 루트로 볼 수 있다. 편하게 하기 위해 '0'번째 노드를 루트로 잡자.
2. 루트 노드에서 리프 노드까지 재귀 함수를 호출해서 파고 든다.
3. 리프 노드에 도달하면, 부모 노드에 리프 노드의 값을 더해주고 리프 노드의 값을 리턴한다.
4. 부모 노드에서는 리프 노드들 값의 절대값을 합해서 모은다.
5. 리프 노드들을 다 돌았으면, 본인 노드의 절대값도 합해서 본인의 부모 노드로 올려준다.
6. 이를 모든 서브 트리에서 반복한다.

​    

>해결

```c++
#include <string>
#include <vector>
#include <math.h>

using namespace std;

long long dfs(vector<long long>& values, const vector<vector<int>>& graph, int beforeNodeIndex, int currentNodeIndex)
{
    if (graph[currentNodeIndex].size() == 1 && graph[currentNodeIndex][0] == beforeNodeIndex)
    {
        values[beforeNodeIndex] += values[currentNodeIndex];
        return values[currentNodeIndex];
    }

    long long sum = 0;
    for (int i = 0; i < graph[currentNodeIndex].size(); i++)
    {
        if (graph[currentNodeIndex][i] == beforeNodeIndex)
            continue;

        sum += abs(dfs(values, graph, currentNodeIndex, graph[currentNodeIndex][i]));
    }

    if (beforeNodeIndex != -1)
    {
        values[beforeNodeIndex] += values[currentNodeIndex];
        sum += abs(values[currentNodeIndex]);
    }

    return sum;
}

long long solution(vector<int> a, vector<vector<int>> edges)
{
    vector<vector<int>> graph(a.size());
    for (int i = 0; i < edges.size(); i++)
    {
        graph[edges[i][0]].push_back(edges[i][1]);
        graph[edges[i][1]].push_back(edges[i][0]);
    }

    vector<long long> values(a.begin(), a.end());
    long long answer = dfs(values, graph, -1, 0);

    if (values[0] != 0)
    {
        answer = -1;
    }

    return answer;
}
```

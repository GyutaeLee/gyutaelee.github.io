---
title: "PROGRAMMERS 프로그래머스 : 합승 택시 요금"
excerpt: "플로이드-와샬 알고리즘(Floyd-Warshall Algorithm)"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **플로이드-와샬 알고리즘(Floyd-Warshall Algorithm)** 문제이다.

[PROGRAMMERS : 합승 택시 요금](https://programmers.co.kr/learn/courses/30/lessons/72413)    

​    

> 문제 이해

   이 문제는 지문이 많이 길다. 그래서 그림과 함께 잘 읽어야 한다. 따로 설명하는거보다는 지문을 잘 읽고 들어가자.

​    

> 문제 아이디어

​	이 문제의 주요점은 다음과 같다.

1. 지점 최대 수는 **200**이다.
2. fares의 최대 크기는 **200 * 199 / 2 = 19900**이다.

​	경우의 수가 매우 적으므로 모든 경로를 탐색함에 시간이 부족하지 않을 것임을 예상할 수 있다. 문제의 아이디어는 다음과 같다.

1. **시작점**에서 **하차점** 까지의 최소 경로를 구한다.
2. **하차점**에서 **A** 까지의 최소 경로를 구한다.
3. **하차점**에서 **B** 까지의 최소 경로를 구한다.

​	이를 모든 점에서 해준 다음, 가장 최소의 값을 구하면 문제가 해결된다. 이때 모든 경로를 구하기 위해 **플로이드-와샬 알고리즘**을 사용한다.

​    

>해결

```c++
#include <string>
#include <vector>
#include <limits.h>

using namespace std;

void InitializeGraph(int n, vector<vector<int>>& graph, vector<vector<int>> &fares)
{
    graph.resize(n + 1);
    for (int i = 1; i <= n; i++)
    {
        graph[i].resize(n + 1);
        for (int j = 1; j <= n; j++)
        {
            graph[i][j] = (i == j) ? 0 : INT_MAX;
        }
    }

    int faresSize = fares.size();
    for (int i = 0; i < faresSize; i++)
    {
        int start = fares[i][0];
        int end = fares[i][1];
        int weight = fares[i][2];

        graph[start][end] = weight;
        graph[end][start] = weight;
    }
}

void DoFloydWarshall(vector<vector<int>>& graph)
{
    int n = graph.size() - 1;

    for (int t = 1; t <= n; t++)
    {
        for (int i = 1; i <= n; i++)
        {
            for (int j = 1; j <= n; j++)
            {
                if (graph[i][t] == INT_MAX)
                {
                    continue;
                }

                if (graph[t][j] == INT_MAX)
                {
                    continue;
                }

                int newRoute = graph[i][t] + graph[t][j];
                if (graph[i][j] > newRoute)
                {
                    graph[i][j] = newRoute;
                }
            }
        }
    }
}

int FindMinimumRoute(vector<vector<int>> &graph, int s, int a, int b)
{
    int n = graph.size() - 1;
    int minimumRoute = INT_MAX;

    for (int dropOffPoint = 1; dropOffPoint <= n; dropOffPoint++)
    {
        int routeWeight = 0;

        if (graph[s][dropOffPoint] == INT_MAX)
        {
            continue;
        }

        if (graph[dropOffPoint][a] == INT_MAX)
        {
            continue;
        }

        if (graph[dropOffPoint][b] == INT_MAX)
        {
            continue;
        }

        routeWeight += graph[s][dropOffPoint];
        routeWeight += graph[dropOffPoint][a];
        routeWeight += graph[dropOffPoint][b];

        minimumRoute = min(minimumRoute, routeWeight);
    }

    return minimumRoute;
}

int solution(int n, int s, int a, int b, vector<vector<int>> fares)
{
    vector<vector<int>> graph;

    InitializeGraph(n, graph, fares);
    DoFloydWarshall(graph);
    int answer = FindMinimumRoute(graph, s, a, b);
    
    return answer;
}
```

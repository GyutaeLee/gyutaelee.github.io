---
title: "PROGRAMMERS 프로그래머스 : 가장 먼 노드"
excerpt: "BFS(Breadth First Search)"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **BFS(Breadth First Search)** 문제이다.

[PROGRAMMERS : 가장 먼 노드](https://programmers.co.kr/learn/courses/30/lessons/49189)    

​        

> 문제 아이디어

​	노드의 개수는 최대 **20,000개**, 간선의 개수는 **50,000개**이다. 1번 노드와 다른 노드들의 최단 거리들을 찾아야하므로 DFS를 사용하면 스택 오버플로우가 발생할만한 크기이므로, BFS로 문제를 해결했다.

​    

>해결

```c++
#include <string>
#include <vector>
#include <queue>
#include <limits.h>

using namespace std;

int solution(int n, vector<vector<int>> edge)
{
	vector<vector<int>> edges(n + 1);
	for (int i = 0; i < edge.size(); i++)
	{		
		edges[edge[i][0]].push_back(edge[i][1]);
		edges[edge[i][1]].push_back(edge[i][0]);
	}

	vector<int> minimumEdgeCount(n + 1, INT_MAX);
	queue<pair<int, int>> q;
	q.push({ 1, 0 });

	while (!q.empty())
	{
		int currentNode = q.front().first;
		int currentEdgeCount = q.front().second;
		q.pop();

		if (minimumEdgeCount[currentNode] <= currentEdgeCount)
			continue;

		minimumEdgeCount[currentNode] = currentEdgeCount;

		for (int i = 0; i < edges[currentNode].size(); i++)
		{
			if (minimumEdgeCount[edges[currentNode][i]] <= currentEdgeCount + 1)
				continue;

			q.push({ edges[currentNode][i], currentEdgeCount + 1 });
		}
	}

	int max = 0, answer = 0;
	for (int i = 1; i <= n; i++)
	{
		if (minimumEdgeCount[i] > max)
		{
			max = minimumEdgeCount[i];
			answer = 1;
		}
		else if (minimumEdgeCount[i] == max)
		{
			answer++;
		}
	}

	return answer;
}
```

---
title: "PROGRAMMERS 프로그래머스 : 양과 늑대"
excerpt: "깊이/너비 우선 탐색(DFS/BFS)"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

   이번 문제는 **깊이 우선 탐색(DFS/BFS)** 문제이다.

[PROGRAMMERS : 양과 늑대](https://programmers.co.kr/learn/courses/30/lessons/92343)    

​        

> 문제 아이디어

​	이 문제는 노드의 최대 수가 17밖에 안되므로 깊이/너비 우선 탐색 둘 중 아무거나 사용해서 풀 수 있다. 방법은 다음과 같다.
1. 노드에서 어느 방향으로 가든지 먼 노드로 이동하는데 비용이 따로 들지 않으므로, 방문한 노드들을 하나로 합쳐서 생각한다.
   - 이를 나타내기 위해 nextNodes에 현재 방문 가능한 노드들을 계속해서 업데이트 한다.
2. 방문할 수 있는 노드들을 방문하면서 양/늑대의 개수를 세어준다.
   - 이때, 양의 수와 늑대의 수가 같아지면 더 이상 방문할 수 없으므로 방문하지 않는다.
3. 가능한 모든 경우를 해보고 최대 양의 개수를 출력한다.

​    

>해결

```c++
#include <string>
#include <vector>
#include <queue>
#include <math.h>

using namespace std;

#define SHEEP (0)
#define WOLF (1)

struct Information
{
	vector<int> nextNodes;
	int sheepCount;
	int wolfCount;
};

int solution(vector<int> info, vector<vector<int>> edges)
{
	vector<vector<int>> graph(info.size());
	for (int i = 0; i < edges.size(); i++)
	{
		graph[edges[i][0]].push_back(edges[i][1]);
	}

	queue<Information> q;
	q.push({ graph[0], 1 ,0 });

	int answer = 0;
	while (q.empty() == false)
	{
		Information currentInformation = q.front();
		q.pop();
		answer = max(answer, currentInformation.sheepCount);

		int differenceBetweenSheepAndWolves = currentInformation.sheepCount - currentInformation.wolfCount;
		for (int i = 0; i < currentInformation.nextNodes.size(); i++)
		{
			int nextNode = currentInformation.nextNodes[i];
			if (info[nextNode] == WOLF && differenceBetweenSheepAndWolves == 1)
				continue;
			
			vector<int> nextNodes;
			for (int j = 0; j < currentInformation.nextNodes.size(); j++)
			{
				if (i == j)
					continue;

				nextNodes.push_back(currentInformation.nextNodes[j]);
			}

			for (int j = 0; j < graph[nextNode].size(); j++)
			{
				nextNodes.push_back(graph[nextNode][j]);
			}

			int nextSheepCount = currentInformation.sheepCount, nextWolfCount = currentInformation.wolfCount;
			if (info[nextNode] == WOLF)
			{
				nextWolfCount++;
			}
			else
			{
				nextSheepCount++;
			}

			q.push({ nextNodes, nextSheepCount, nextWolfCount});
		}
	}

	return answer;
}
```

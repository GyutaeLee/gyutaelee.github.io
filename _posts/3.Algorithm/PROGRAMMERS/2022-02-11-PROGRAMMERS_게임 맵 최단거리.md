---
title: "PROGRAMMERS 프로그래머스 : 게임 맵 최단거리"
excerpt: "BFS(너비 우선 탐색)"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

   이번 문제는 **BFS(너비 우선 탐색)** 문제이다.

[PROGRAMMERS : 게임 맵 최단거리](https://programmers.co.kr/learn/courses/30/lessons/1844)    

​        

> 문제 아이디어

​	이 문제는 기본적인 길 찾기 문제이다. maps의 최대 크기가 100x100이므로, dfs, bfs 중 무엇을 선택해도 사실 상관없는 문제이다.    
    문제를 해결하는 방법은 다음과 같다.
1.  현재 위치에서 위/아래/오른/왼 4방향으로 갈 수 있는 방향이 있으면, 해당 좌표와 움직인 거리를 큐에 넣어준다.
2.  이때 방문한 곳은 다시 방문할 필요 없으므로 isVisited 배열에 기록한다.
3.  목표 지점에 도달할 때까지 반복한다.

​    

>해결

```c#
using System;
using System.Collections.Generic;

public class Solution
{
    class PointInformation
    {
        public int y, x;
        public int moveCount;
        public PointInformation(int y, int x, int moveCount)
        {
            this.y = y;
            this.x = x;
            this.moveCount = moveCount;
        }
    };

    readonly int[,] direction = { { -1, 0 }, { 0, 1 }, { 1, 0 }, { 0, -1 } };
    public int solution(int[,] maps)
    {
        int n = maps.GetLength(0);
        int m = maps.GetLength(1);
        int answer = int.MaxValue;
        Queue<PointInformation> queue = new Queue<PointInformation>();
        bool[,] isVisited = new bool[n, m];

        isVisited[0, 0] = true;
        queue.Enqueue(new PointInformation(0, 0, 1));
        while (queue.Count != 0)
        {
            PointInformation current = queue.Dequeue();

            for (int i = 0; i < 4; i++)
            {
                int nextY = current.y + direction[i,0];
                int nextX = current.x + direction[i,1];

                if (nextY == n - 1 && nextX == m - 1)
                {
                    answer = Math.Min(answer, current.moveCount);
                    break;
                }

                if (nextY < 0 || nextX < 0 || nextY >= n || nextX >= m)
                {
                    continue;
                }

                if (maps[nextY, nextX] == 0)
                {
                    continue;
                }

                if (isVisited[nextY, nextX] == true)
                {
                    continue;
                }

                isVisited[nextY, nextX] = true;
                queue.Enqueue(new PointInformation(nextY, nextX, current.moveCount + 1));
            }
        }

        if (answer == int.MaxValue)
        {
            answer = -1;
        }
        else
        {
            answer++;
        }

        return answer;
    }
}
```

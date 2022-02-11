---
title: "PROGRAMMERS 프로그래머스 : 거리두기 확인하기"
excerpt: "BFS(너비 우선 탐색)"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

   이번 문제는 **BFS(너비 우선 탐색)** 문제이다.

[PROGRAMMERS : 거리두기 확인하기](https://programmers.co.kr/learn/courses/30/lessons/81302)    

​        

> 문제 아이디어

​	이 문제는 현실의 상황을 설명해주기 때문에 문제를 잘 읽어야한다. 요점은 다음과 같다.  
1.  각 강의실에서 거리두기를 해야하는데, 그 거리가 좌표 거리로 총 2 이하면 실패다.
2.  이때, 파티션이 있는 곳은 거리가 가까워도 무시한다. 즉, 파티션은 방문하지 않아도 된다.
3.  위 규칙으로 사람들만 탐색하면서 거리가 2 이하인 사람이 있으면 실패, 없으면 성공으로 답을 출력하면 된다.

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
    public int[] solution(string[,] places)
    {
        int[] answer = new int[5];

        for (int i = 0; i < 5; i++)
        {
            List<KeyValuePair<int, int>> points = new List<KeyValuePair<int, int>>();

            for (int j = 0; j < 5; j++)
            {
                string currentRow = places[i, j];
                for (int k = 0; k < 5; k++)
                {
                    if (currentRow[k] == 'P')
                    {
                        points.Add(new KeyValuePair<int, int>(j, k));
                    }
                }
            }

            bool isFound = false;
            foreach (KeyValuePair<int, int> point in points)
            {
                Queue<PointInformation> queue = new Queue<PointInformation>();
                queue.Enqueue(new PointInformation(point.Key, point.Value, 0));
                bool[,] isVisited = new bool[5, 5];

                isVisited[point.Key, point.Value] = true;
                while (queue.Count != 0)
                {
                    PointInformation currentPointInformation = queue.Dequeue();
                    if (currentPointInformation.moveCount + 1 > 2)
                        continue;

                    for (int j = 0; j < 4; j++)
                    {
                        int currentY = currentPointInformation.y + direction[j, 0];
                        int currentX = currentPointInformation.x + direction[j, 1];

                        if (currentY < 0 || currentX < 0 || currentY >= 5 || currentX >= 5)
                            continue;

                        if (isVisited[currentY, currentX] == true)
                            continue;

                        if (places[i, currentY][currentX] == 'X')
                            continue;

                        if (places[i, currentY][currentX] == 'P')
                        {
                            isFound = true;
                            goto Found;
                        }

                        isVisited[currentY, currentX] = true;
                        queue.Enqueue(new PointInformation(currentY, currentX, currentPointInformation.moveCount + 1));
                    }
                }
            }

        Found:
            if (isFound == true)
            {
                answer[i] = 0;
            }
            else
            {
                answer[i] = 1;
            }
        }

        return answer;
    }
}
```

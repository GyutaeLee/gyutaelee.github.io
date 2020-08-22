---
title: "PROGRAMMERS 프로그래머스 : 정수 삼각형"
excerpt: "동적계획법(Dynamic Programming)"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **동적계획법(Dynamic Programming)** 문제이다.

[PROGRAMMERS : 정수 삼각형](https://programmers.co.kr/learn/courses/30/lessons/43105)    

​    

> 문제 이해

   문제를 읽어보자. 먼저 이 문제에서 나오는 변수는 총 1가지이다.

- vector<vector<int>> triangle : 삼각형을 이루고 있는 2차원 배열

​    

> 문제 아이디어

　꼭대기에서 바닥까지 이동할 때 한 칸씩 아래로만 이동이 가능하다는 제약 조건이 있다. 모든 경로를 한 번씩 다 해보기에는 비효율적이므로, 한 번 거쳐가면서 해당 위치까지 오는 값을 갱신해준다.

​	이 때 갱신 조건은 이전에 값이 있었다면, 해당 값보다 높을 때, 즉 최대 값을 만들 수 있는 값만을 갱신해준다.

​    

>해결

```c++
#include <string>
#include <vector>

using namespace std;

int solution(vector<vector<int>> triangle) 
{
    int answer = 0;
    vector<vector<int>> pathValue;

    // set vector size
    pathValue.resize(triangle.size());
    
    // init 0 index
    pathValue[0].resize(1);
    pathValue[0] = triangle[0];

    for (int i = 0; i < triangle.size() - 1; i++)
    {
        // set vector size
        pathValue[i + 1].resize(triangle[i + 1].size());
        for (int j = 0; j < triangle[i].size(); j++)
        {
            // dp
            if (pathValue[i][j] + triangle[i + 1][j] > pathValue[i + 1][j])
            {
                pathValue[i + 1][j] = pathValue[i][j] + triangle[i + 1][j];
            }

            // dp
            if (pathValue[i][j] + triangle[i + 1][j + 1] > pathValue[i + 1][j + 1])
            {
                pathValue[i + 1][j + 1] = pathValue[i][j] + triangle[i + 1][j + 1];
            }
        }
    }

    // find max value
    for (int i = 0; i < pathValue[pathValue.size() - 1].size(); i++)
    {
        answer = pathValue[pathValue.size() - 1][i] > answer ? pathValue[pathValue.size() - 1][i] : answer;
    }

    return answer;
}
```

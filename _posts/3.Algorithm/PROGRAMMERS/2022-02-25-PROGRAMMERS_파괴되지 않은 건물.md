---
title: "PROGRAMMERS 프로그래머스 : 파괴되지 않은 건물"
excerpt: "구간합"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

   이번 문제는 **구간합** 문제이다.

[PROGRAMMERS : 파괴되지 않은 건물](https://programmers.co.kr/learn/courses/30/lessons/92344)    

​        

> 문제 아이디어

​	이 문제는 그냥 읽었을 때는 적힌 그대로 구현하면 되지 않을까 라는 생각이 드는 문제이다. 하지만 효율성 테스트가 있기 때문에 단순히 구현하면 O(N * M * skill) -> O(1000 * 1000 * 250,000)의 시간 복잡도가 나오므로 시간 초과로 문제를 맞출 수 없다.    
​	이때 사용 가능한게 **구간 합** 개념이다. 일반적으로는 선형으로 1차원 배열에서 많이 사용하는데, 이걸 조금만 더 생각해보면 2차원 배열에도 이용할 수 있다. 설명은 [링크](https://tech.kakao.com/2022/01/14/2022-kakao-recruitment-round-1/)로 첨부한다.

​    

>해결

```c++
#include <string>
#include <vector>

using namespace std;

int solution(vector<vector<int>> board, vector<vector<int>> skill)
{
    int row = board.size();
    int col = board[0].size();

    vector<vector<int>> sumOfIntervals(row + 1);
    for (int i = 0; i < sumOfIntervals.size(); i++)
    {
        sumOfIntervals[i].resize(col + 1);
    }

    for (auto currentSkill : skill)
    {
        int degree = (currentSkill[0] == 1) ? -currentSkill[5] : currentSkill[5];

        sumOfIntervals[currentSkill[1]][currentSkill[2]] += degree;
        sumOfIntervals[currentSkill[1]][currentSkill[4] + 1] -= degree;
        sumOfIntervals[currentSkill[3] + 1][currentSkill[2]] -= degree;
        sumOfIntervals[currentSkill[3] + 1][currentSkill[4] + 1] += degree;
    }

    for (int i = 0; i < row + 1; i++)
    {
        for (int j = 0; j < col; j++)
        {
            sumOfIntervals[i][j + 1] += sumOfIntervals[i][j];
        }
    }

    for (int j = 0; j < col + 1; j++)
    {
        for (int i = 0; i < row; i++)
        {
            sumOfIntervals[i + 1][j] += sumOfIntervals[i][j];
        }
    }

    int answer = 0;
    for (int i = 0; i < row; i++)
    {
        for (int j = 0; j < col; j++)
        {
            if (board[i][j] + sumOfIntervals[i][j] > 0)
            {
                answer++;
            }
        }
    }

    return answer;
}
```

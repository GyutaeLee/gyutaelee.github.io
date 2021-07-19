---
title: "PROGRAMMERS 프로그래머스 : 매출 하락 최소화"
excerpt: "깊이우선탐색/동적 계획법(DFS/Dynamic Programming)"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **깊이우선탐색/동적 계획법(DFS/Dynamic Programming)** 문제이다.

[PROGRAMMERS : 매출 하락 최소화](https://programmers.co.kr/learn/courses/30/lessons/72416)    

​    

> 문제 이해

   이 문제는 지문이 많이 길다. 그래서 그림과 함께 잘 읽어야 한다. 따로 설명하는거보다는 지문을 잘 읽고 들어가자.

​    

> 문제 아이디어

​	이 문제는 참 복잡하고, 아이디어를 짜기 쉽지 않다. 키 포인트를 하나씩 집어보자.

1. **1번**은 항상 CEO이다.
2. **팀**에서 **팀장**은 한 명이다.
3. **팀**에서 몇 명이 참여해도 상관없다.
4. **팀**에서 적어도 **1명 이상**은 워크숍에 참여해야한다.
5. 직원들의 하루 평균 **매출액의 합**이 **최소**가 되어야 한다.

​	사실 이렇게 적어봐도 감이 확 오지는 않는다. 그렇다면 단순하게 문제를 생각해보자. 직원들의 수와 동일한 **sales**의 크기가 최대 300,000이므로 N번만 돌아야하는 것을 추측할 수 있다. 그리고 각 직원들은 2가지 경우의 수를 가지고 있다. **워크숍 참석**, **워크숍 불참**. 이 경우의 수를 모두 확인하면서 문제를 풀 것이다.

​	이 경우의 수를 확인하면서 각 값들은 저장되어야 한다. 그래서 **DP\[직원의 수][2]** 크기의 배열이 필요하다. 여기서 중요한 점은 이 방법으로 문제를 해결할 때에는 가장 아랫단, 즉 **말단 직원**부터 확인해야 한다. 그 이유는 내 밑에 있는 직원들이 **참석, 불참**일 때의 값을 알아야 나의 값도 계산할 수 있기 때문이다.

​	이 이상은 설명하기가 너무 힘들다... (경우의 수도 많고, 글 쓰기에는 시간이 너무 많이 든다)
[좋은 블로그 추천](https://yabmoons.tistory.com/625)

​    

>해결

```c++
#include <string>
#include <vector>
#include <utility>
#include <limits.h>

using namespace std;

#define NOT_ATTENDANCE (0)
#define ATTENDANCE     (1)

void DFS(const vector<vector<int>>& team, vector<vector<int>>& DP, const vector<int>& sales, int currentIndex)
{
    if (team[currentIndex].size() == 0)
    {
        DP[currentIndex][NOT_ATTENDANCE] = 0;
        DP[currentIndex][ATTENDANCE] = sales[currentIndex - 1];
        return;
    }

    for (int i = 0; i < team[currentIndex].size(); i++)
    {
        int nextIndex = team[currentIndex][i];
        DFS(team, DP, sales, nextIndex);
    }

    bool mustAttend = true;
    int sum = 0, min_CostDifferenceOfAttendance = INT_MAX;
    for (int i = 0; i < team[currentIndex].size(); i++)
    {
        int member = team[currentIndex][i];
        sum += min(DP[member][NOT_ATTENDANCE], DP[member][ATTENDANCE]);
        min_CostDifferenceOfAttendance = min(min_CostDifferenceOfAttendance, DP[member][ATTENDANCE] - DP[member][0]);

        if (DP[member][NOT_ATTENDANCE] >= DP[member][ATTENDANCE])
        {
            mustAttend = false;
        }
    }

    DP[currentIndex][ATTENDANCE] = sales[currentIndex - 1] + sum;

    if (mustAttend == false)
    {
        DP[currentIndex][NOT_ATTENDANCE] = sum;
    }
    else
    {
        DP[currentIndex][NOT_ATTENDANCE] = sum + min_CostDifferenceOfAttendance;
    }
}

int solution(vector<int> sales, vector<vector<int>> links)
{
    int answer = 0;
    vector<vector<int>> team;

    team.resize(sales.size() + 1);
    for (int i = 0; i < links.size(); i++)
    {
        team[links[i][0]].push_back(links[i][1]);
    }

    vector<vector<int>> DP;
    DP.resize(sales.size() + 1);
    for (int i = 0; i < DP.size(); i++)
    {
        DP[i].resize(2);
    }

    DFS(team, DP, sales, 1);
    answer = min(DP[1][NOT_ATTENDANCE], DP[1][ATTENDANCE]);

    return answer;
}
```

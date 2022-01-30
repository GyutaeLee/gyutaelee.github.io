---
title: "PROGRAMMERS 프로그래머스 : 여행 경로"
excerpt: "DFS(깊이 우선 탐색)"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **DFS(깊이 우선 탐색)** 문제이다.

[PROGRAMMERS : 여행 경로](https://programmers.co.kr/learn/courses/30/lessons/43164)    

​        

> 문제 아이디어

​	이 문제는 모든 항공권을 사용해야하므로, 완전 탐색을 해야하는 문제이다. 또한 같은 출발지에서 여러 목적지를 갈 수 있으므로 같은 도시에서 갈 수 있는 항공권을 모두 사용했는지 체크도 필요하다.

​    

>해결

```c++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

bool Dfs(const vector<vector<string> > tickets, vector<bool> &usedTickets, vector<string> &answer, string currentTicket, int useTicketCount)
{
   answer.push_back(currentTicket);
   if (useTicketCount == tickets.size())
      return true;

   for (int i = 0; i < tickets.size(); i++)
   {
      if (tickets[i][0] == currentTicket && usedTickets[i] == false)
      {
         usedTickets[i] = true;
         bool isTrue = Dfs(tickets, usedTickets, answer, tickets[i][1], useTicketCount + 1);
         if (isTrue == true)
            return true;
         usedTickets[i] = false;
      }
   }
   answer.pop_back();
   return false;
}

vector<string> solution(vector<vector<string> > tickets)
{
   vector<bool> isUsedTickets(tickets.size(), false);
   sort(tickets.begin(), tickets.end());

   vector<string> answer;
   Dfs(tickets, isUsedTickets, answer, "ICN", 0);

   return answer;
}
```

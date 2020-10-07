---
title: "PROGRAMMERS 프로그래머스 : 카펫"
excerpt: "완전 탐색(Brute Force Search)"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **완전 탐색(Brute Force Search)** 문제이다.

[PROGRAMMERS : 카펫](https://programmers.co.kr/learn/courses/30/lessons/42842)    

​    

> 문제 이해

   문제를 읽어보자. 먼저 이 문제에서 나오는 변수는 총 2가지이다.

- brown 
- yellow

​    

> 문제 아이디어

​	이 문제의 핵심은 대기목록의 최대 brown 개수가 5,000이고, yellow 개수가 2,000,000이다. 모든 경우의 수를 다 구해보아도 그 수가 충분히 작으므로 완전 탐색이 가능하다.

1. 가로를 가장 길게, 세로를 가장 짧게 시작한다. (문제 조건이 가로가 세로보다 같거나 더 길다고 했다)
2. 가로를 줄이고, 세로를 늘리면서 조건에 맞는 답을 찾는다.

​    

>해결

```c++
#include <string>
#include <vector>

using namespace std;

vector<int> solution(int brown, int yellow)
{
    vector<int> answer;
    int horizontal, vertical, yellowCount;
    
    // 1. 가로가 가장 긴 경우부터 시작한다.
    horizontal = (brown - 2) / 2;
    vertical = 3;
    
    yellowCount = (horizontal - 2) * (vertical - 2);
    
    // 2. 가로를 줄이고, 세로를 늘리면서 조건에 맞는 답을 찾는다.
    while (yellowCount != yellow)
    {
        horizontal--;
        vertical++;
        
        yellowCount = (horizontal - 2) * (vertical - 2);
    }
    
    answer.push_back(horizontal);
    answer.push_back(vertical);
        
    return answer;
}
```

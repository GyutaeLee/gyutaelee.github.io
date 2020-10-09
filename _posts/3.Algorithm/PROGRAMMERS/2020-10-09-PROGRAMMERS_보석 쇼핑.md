---
title: "PROGRAMMERS 프로그래머스 : 보석 쇼핑"
excerpt: "투 포인터(Two Pointer)"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **투 포인터(Two Pointer)** 문제이다.

[PROGRAMMERS : 보석 쇼핑](https://programmers.co.kr/learn/courses/30/lessons/67258)    

​    

> 문제 이해

   문제를 읽어보자. 먼저 이 문제에서 나오는 변수는 총 1가지이다.

- vector<string> gems : 진열된 보석의 종류

​    

> 문제 아이디어

​	이 문제의 핵심은 진열된 보석의 최대 개수가 100,000 개라는 점이다. 경우의 수가 매우 크므로 n번의 탐색으로 문제를 해결해야한다. 연속된 보석들을 검사해야하므로 투 포인터를 사용해서 head와 tail을 옮겨가며 문제를 해결했다.    

>해결

```c++
#include <string>
#include <vector>
#include <unordered_set>
#include <unordered_map>

using namespace std;

vector<int> solution(vector<string> gems)
{
	vector<int> answer;
	unordered_set<string> gemSet;
	unordered_map<string, int> gemMap;

	// 1. answer 초기값 '-1'
	answer.push_back(-1);
	answer.push_back(-1);

	// 2. 보석 종류가 몇개인지 센다.
	for (int i = 0; i < gems.size(); i++)
	{
		gemSet.insert(gems[i]);
	}

	// 3. 투 포인터를 사용해서 문제를 해결한다.
	int headIndex = 0, tailIndex = 0;
	while (true)
	{
		if (gemMap.size() >= gemSet.size())
		{
			// 3-1. 조건에 맞으면 답을 갱신한다.		
			int gemCount = (tailIndex - 1) - headIndex;

			if ((answer[1] == -1 && answer[0] == -1) ||
				(answer[1] - answer[0] > gemCount) ||
				(answer[1] - answer[0] == gemCount && answer[0] > headIndex))
			{
				answer[0] = headIndex;
				answer[1] = tailIndex - 1;
			}

			// 3-2. headIndex의 값을 하나 줄여본다.
			gemMap[gems[headIndex]]--;

			if (gemMap[gems[headIndex]] == 0)
			{
				gemMap.erase(gems[headIndex]);
			}

			headIndex++;
		}
		else if (tailIndex >= gems.size())
		{
			break;
		}
		// 3-3. 조건에 맞지 않으면 tailIndex의 값을 하나 늘려본다.
		else
		{
			gemMap[gems[tailIndex]]++;
			tailIndex++;
		}
	}

	// 4. 답은 index가 1씩 높으므로 값을 더해준다.
	answer[0]++;
	answer[1]++;

	return answer;
}
```

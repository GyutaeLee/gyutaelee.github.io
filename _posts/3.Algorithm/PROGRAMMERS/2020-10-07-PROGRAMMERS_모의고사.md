---
title: "PROGRAMMERS 프로그래머스 : 모의고사"
excerpt: "완전 탐색(Brute Force Search)"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **완전 탐색(Brute Force Search)** 문제이다.

[PROGRAMMERS : 모의고사](https://programmers.co.kr/learn/courses/30/lessons/42840)    

​    

> 문제 이해

   문제를 읽어보자. 먼저 이 문제에서 나오는 변수는 총 2가지이다.

- vector<int> answers : 문제의 답
- 수포자들의 문제 찍는 방식

​    

> 문제 아이디어

​	이 문제의 핵심은 대기목록의 최대 수포자들의 문제 찍는 방식의 길이가 짧고, 시험의 최대 문제가 10,000개라는 점이다. 충분히 작으므로 완전 탐색을 통해 해결할 수 있다.

1. 각 수포자들의 문제 찍는 방식을 vector에 저장한다.
2. 첫 문제부터 마지막 문제까지 각 수포자들의 문제 찍는 방식과 비교해가며 점수를 센다.
3. 가장 높은 점수를 가진 수포자들을 배열에 넣는다.

​    

>해결

```c++
#include <string>
#include <vector>

using namespace std;

int GetMathAbandonerScore(const vector<int>& answers, int type)
{
	vector<int> number;
	int score = 0;

    // 각 수포자들의 패턴에 따라 vector를 만든다.
	switch (type)
	{
		case 1:	
			number = { 1, 2, 3, 4, 5 };	
			break;
		case 2:
			number = {2,1,2,3,2,4,2,5};
			break;
		case 3:
			number = { 3, 3, 1, 1, 2, 2, 4, 4, 5, 5 };
			break;
		default:
			break;
	}

    // 첫 문제부터 마지막 문제까지 문제 찍는 방식을 루프하면서 점수를 센다.
	int numberIndex = 0;
	for (int i = 0; i < answers.size(); i++, numberIndex++)
	{
		if (numberIndex >= number.size())
		{
			numberIndex = 0;
		}

		if (number[numberIndex] == answers[i])
		{
			score++;
		}
	}

	return score;
}

vector<int> solution(vector<int> answers)
{
	vector<int> answer;
	vector<int> score;
	int max = 0;

    // 1. 각 수포자들의 점수를 구한다.
	score.push_back(GetMathAbandonerScore(answers, 1));
	score.push_back(GetMathAbandonerScore(answers, 2));
	score.push_back(GetMathAbandonerScore(answers, 3));

    // 2. 최대 점수를 구한다.
	for (int i = 0; i < score.size(); i++)
	{
		max = (score[i] > max) ? score[i] : max;
	}

    // 3. 최대 점수를 가진 수포자들을 답에 넣어준다.
	for (int i = 0; i < score.size(); i++)
	{
		if (score[i] == max)
		{
			answer.push_back(i + 1);
		}
	}

	return answer;
}
```

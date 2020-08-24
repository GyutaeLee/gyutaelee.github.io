---
title: "PROGRAMMERS 프로그래머스 : 가장 큰 수"
excerpt: "정렬(Sort)"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **정렬(Sort)** 문제이다.

[PROGRAMMERS : 가장 큰 수](https://programmers.co.kr/learn/courses/30/lessons/42746)    

​    

> 문제 이해

   문제를 읽어보자. 먼저 이 문제에서 나오는 변수는 총 1가지이다.

- vector<int> numbers : 주어진 정수 벡터

​    

> 문제 아이디어

　주어진 정수를 정수가 아닌 **문자열**처럼 붙이는게 핵심이다. 단순하게 이어 붙였을 때 더 큰 경우를 만들면 되므로, quick sort의 비교 함수를 문자열 비교 함수로 만들어서 문제를 해결하면 된다.

​    

>해결

```c++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

bool Compare(int A, int B)
{
    string caseA, caseB;

    caseA = to_string(A) + to_string(B);
    caseB = to_string(B) + to_string(A);

    if (caseA > caseB)
    {
        return true;
    }
    else if (caseA < caseB)
    {
        return false;
    }

    return false;
}

string solution(vector<int> numbers)
{
    string answer = "";

    // 전부 0 이면 0으로 출력
    for (int i = 0; i < numbers.size(); i++)
    {
        if (numbers[i] != 0)
            break;

        if (i == numbers.size() - 1)
        {
            return "0";
        }
    }

    // Compare 함수로 정렬
	sort(numbers.begin(), numbers.end(), Compare);

	for (int i = 0; i < numbers.size(); i++)
	{
		answer.append(to_string(numbers[i]));
	}

    return answer;
}
```

---
title: "PROGRAMMERS 프로그래머스 : 소수 찾기"
excerpt: "완전탐색(Brute-force)"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **완전탐색(Brute-force)** 문제이다.

[PROGRAMMERS : 소수 찾기](https://programmers.co.kr/learn/courses/30/lessons/42839)    

​    

> 문제 이해

   문제를 읽어보자. 먼저 이 문제에서 나오는 변수는 총 1가지이다.

- numbers : 숫자가 들어있는 string

​    

> 문제 아이디어

　해당 문제의 핵심은 numbers의 길이가 7이하라는 것이다. 이렇게 되면 문자열의 길이가 7일 때가 가장 큰 경우인데 충분히 작으므로 완전탐색으로 해결이 가능하다.

​    

>해결

```c++
#include <string>
#include <vector>
#include <algorithm>
#include <set>

using namespace std;

bool IsPrimeNumber(int number)
{
    if (number <= 1)
    {    
        return false;
    }

    for (int i = 2; i * i <= number; i++)
    {
        if (number % i == 0)
        {
            return false; 
        }
    }

    return true;
}

int solution(string numbers)
{
    set<int> numberSet;
    vector<int> numberVector;
    int answer = 0;

    for (int i = 0; i < numbers.length(); i++)
    {
        numberVector.push_back(numbers[i] - '0');
    }

    sort(numberVector.begin(), numberVector.end());

	do
	{
		int number = 0;

		for (int i = 0; i < numberVector.size(); i++)
		{
			number += numberVector[i];

			if (IsPrimeNumber(number) == true)
			{
				numberSet.insert(number);
			}

            number *= 10;
		}
	} while (next_permutation(numberVector.begin(), numberVector.end()));

    answer = numberSet.size();

    return answer;
}
```

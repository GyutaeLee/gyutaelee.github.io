---
title: "PROGRAMMERS 프로그래머스 : 문자열 압축"
excerpt: "2020 KAKAO BLIND RECRUITMENT"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **2020 KAKAO BLIND RECRUITMENT** 문제이다. 문자열 조작 문제이다.

[PROGRAMMERS : 문자열 압축](https://programmers.co.kr/learn/courses/30/lessons/60057)    

​    

> 문제 이해

   문제를 읽어보자. 먼저 이 문제에서 나오는 변수는 총 1가지이다.

- s : 문자열

​    

> 문제 아이디어

　문자열의 길이가 1000이므로 단순하게 1, 2, ..., n/2 까지의 문자열들을 범위만 잘 나누어서 비교해주면 쉽게 풀리는 문제이다.

​    

>해결

```c++
#include <string>
#include <vector>

using namespace std;

void CheckAnswer(int* answer, int* currentLength, int i, int sameCount)
{
    if (sameCount > 1)
    {
        int sameCountLength = 0;

        if (sameCount < 10)
        {
            sameCountLength = 1;
        }
        else if (sameCount < 100)
        {
            sameCountLength = 2;
        }
        else if (sameCount < 1000)
        {
            sameCountLength = 3;
        }

        *currentLength -= (sameCount * i) - (sameCountLength + i);

        *answer = (*answer > *currentLength) ? *currentLength : *answer;
    }
}

int solution(string s) {
    int answer = 0;

    // 초기 답 : 문자열 초기 길이
    answer = s.length();

    // 문자열 절반까지만 검사
    float iRange = s.length() * 0.5f;
    for (int i = 1; i <= iRange; i++)
    {
        int currentLength = s.length();
        int sameCount = 0;
        string compareString;
        
        // 문자열 길이에서 비교 길이를 빼준만큼만 검사
        int jRange = s.length() - i + 1;
        for (int j = 0; j < jRange; )
        {
            // 초기 세팅
            if (sameCount == 0)
            {
                compareString = s.substr(j, i);
                sameCount = 1;

                j += i;
            }
            else
            {
                // 동일한 문자열인 경우 다음 범위 검사
                if (compareString == s.substr(j, i))
                {
                    sameCount++;

                    j += i;
                }
                // 답과 비교해서 값을 넣어준다
                else
                {
                    CheckAnswer(&answer, &currentLength, i, sameCount);
                    sameCount = 0;
                }
            }
        }

        // 검사 도중 문자열 마지막까지 도달해서 빠져나왔을 때를 대비
        CheckAnswer(&answer, &currentLength, i, sameCount);
    }

    return answer;
}
```

---
title: "PROGRAMMERS 프로그래머스 : 괄호 변환"
excerpt: "2020 KAKAO BLIND RECRUITMENT"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **2020 KAKAO BLIND RECRUITMENT** 문제이다. 문자열 조작 문제이다.

[PROGRAMMERS : 괄호 변환](https://programmers.co.kr/learn/courses/30/lessons/60058)    

​    

> 문제 이해

   문제를 읽어보자. 먼저 이 문제에서 나오는 변수는 총 1가지이다.

- p : 균형잡힌 괄호 문자열

​    

> 문제 아이디어

　이 문제는 해결해야하는 방법이 나와있다.  다른 방법으로 하면 더 쉬울거 같지만, 재귀 함수를 잘 사용하는지에 대해 테스트하고자 하는 문제 같다.

​	문제에 나온 방법을 로직에 맞게 잘 짜면 문제는 해결된다.

​    

>해결

```c++
#include <string>
#include <vector>

using namespace std;

bool IsCorrectBracketString(string str);
string MakeCorrectBracketString(string u, string v);
string Recrusive(string p);

bool IsCorrectBracketString(string str)
{
    int leftBracketCount = 0, rightBracketCount = 0;
    bool bResult = true;

    for (int i = 0; i < str.length(); i++)
    {
        if (str[i] == '(')
        {
            leftBracketCount++;
        }
        else if (str[i] == ')')
        {
            rightBracketCount++;
        }

        if (leftBracketCount < rightBracketCount)
        {
            bResult = false;
            break;
        }
    }

    return bResult;
}

string MakeCorrectBracketString(string u, string v)
{    
    //4. 문자열 u가 "올바른 괄호 문자열"이 아니라면 아래 과정을 수행합니다.    
    string str;
    
    // 4 - 1. 빈 문자열에 첫 번째 문자로 '('를 붙입니다.
    str.append("(");

    // 4 - 2. 문자열 v에 대해 1단계부터 재귀적으로 수행한 결과 문자열을 이어 붙입니다.
    v = Recrusive(v);
    str.append(v);
    
    // 4 - 3. ')'를 다시 붙입니다.
    str.append(")");

    // 4 - 4. u의 첫 번째와 마지막 문자를 제거하고, 나머지 문자열의 괄호 방향을 뒤집어서 뒤에 붙입니다.
    u.erase(0, 1);
    u.erase(u.length() - 1, 1);

    for (int i = 0; i < u.length(); i++)
    {
        if (u[i] == '(')
        {
            u[i] = ')';
        }
        else if (u[i] == ')')
        {
            u[i] = '(';
        }
    }

    str.append(u);

    // 4 - 5. 생성된 문자열을 반환합니다.
    return str;
}

string Recrusive(string p)
{
    // 1. 입력이 빈 문자열인 경우, 빈 문자열을 반환한다.
    if (p == "")
    {
        return p;
    }

    // 2. 문자열 w를 두 "균형잡힌 괄호 문자열" u, v로 분리합니다.
    // 단, u는 "균형잡힌 괄호 문자열"로 더 이상 분리할 수 없어야 하며, v는 빈 문자열이 될 수 있습니다.
    string u, v;
    int leftBracketCount = 0, rightBracketCount = 0;
    int strIndex = 0;

    // 균형잡힌 문자열 u 분리
    while (strIndex < p.length())
    {
        u += p[strIndex];

        if (p[strIndex] == '(')
        {
            leftBracketCount++;
        }
        else if (p[strIndex] == ')')
        {
            rightBracketCount++;
        }

        strIndex++;

        if (leftBracketCount == rightBracketCount)
        {
            break;
        }
    }

    // 남은 문자열 v
    v = p.substr(strIndex, p.length() - strIndex + 1);

    // 3. 문자열 u가 "올바른 괄호 문자열" 이라면 문자열 v에 대해 1단계부터 다시 수행합니다.
    // 3-1. 수행한 결과 문자열을 u에 이어 붙인 후 반환합니다.
    if (IsCorrectBracketString(u) == true)
    {
        v = Recrusive(v);

        u.append(v);
    }
    else
    {
        u = MakeCorrectBracketString(u, v);
    }

    return u;
}

string solution(string p)
{
    string answer = "";

    answer = p;

    answer = Recrusive(answer);

    return answer;
}
```

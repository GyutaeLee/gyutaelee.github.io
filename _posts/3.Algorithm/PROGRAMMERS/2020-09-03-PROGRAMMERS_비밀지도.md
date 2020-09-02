---
title: "PROGRAMMERS 프로그래머스 : 비밀 지도"
excerpt: "비트연산(Bitwise operator)"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **비트연산(Bitwise operator)** 문제이다.

[PROGRAMMERS : 비밀 지도](https://programmers.co.kr/learn/courses/30/lessons/17681)    

​    

> 문제 이해

   문제를 읽어보자. 먼저 이 문제에서 나오는 변수는 총 3가지이다.

- n : 지도 한 변의 길이
- arr1, arr2 : 암호화된 비밀 지도

​    

> 문제 아이디어

　이 문제의 핵심은 두 지도를 겹쳤을 때 답이 나온다. 이 겹치는 과정이 비트 연산의 \'|' OR 연산과 동일하다.

1. arr1과 arr2을 한 줄씩 OR 연산을 통해 해독된 숫자로 바꾼다.
2. 바뀐 숫자를 2진수로 변환해서 1은 '#', 0은 ' '을 넣어서 문자열로 반환한다.
3. answer 벡터에 하나씩 넣어준다.

​	위의 과정을 거치면 답이 나온다. 물론 이 문제는 비트 연산을 사용하지 않아도 풀 수 있지만, 대놓고 비트 연산을 떠올려서 풀라고 문제에 써있는듯 해서 문제 종류를 **비트 연산**으로 넣었다.

​    

>해결

```c++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

string DemicalToBinaryNumberString(int n, int demical)
{
    string binaryNumber = "";

    while (demical > 0)
    {
        binaryNumber.append((demical % 2 == 1) ? "#" : " ");
        demical /= 2;
    }

    for (int i = 0; i < binaryNumber.length() - n; i++)
    {
        binaryNumber.append(" ");
    }
    
    reverse(binaryNumber.begin(), binaryNumber.end());

    return binaryNumber;
}

vector<string> solution(int n, vector<int> arr1, vector<int> arr2) {
    vector<string> answer;

    for (int i = 0; i < n; i++)
    {
        int demical = arr1[i] | arr2[i];
        answer.push_back(DemicalToBinaryNumberString(n, demical));
    }
    
    return answer;
}
```

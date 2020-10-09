---
title: "PROGRAMMERS 프로그래머스 : 단어 변환"
excerpt: "깊이/너비 우선 탐색(DFS/BFS)"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **너비 우선 탐색(BFS)** 문제이다.

[PROGRAMMERS : 단어 변환](https://programmers.co.kr/learn/courses/30/lessons/43163)    

​    

> 문제 이해

   문제를 읽어보자. 먼저 이 문제에서 나오는 변수는 총 3가지이다.

- begin, traget : 검사해야할 단어
- vector<string> words : 사용할 수 있는 단어들의 집합

​    

> 문제 아이디어

​	이 문제의 핵심은 words 배열의 최대 길이가 50개라는 점이다. 그래서 현재 글자에서 변할 수 있는 글자가 있는지 배열을 계속 돌면서 검사하고, 변할 수 있는 글자는 또 다시 검사하면서 답을 찾으면 된다.

​	여기서 한 번 검사한 단어는 다시 검사하지 않기 위해 visited 배열을 사용한다.

​    

>해결

```c++
#include <string>
#include <vector>
#include <queue>
#include <algorithm>

using namespace std;

bool CanTranslate(string a, string b)
{
    if (a.length() != b.length())
        return false;

    int differentCount = 0;

    for (int i = 0; i < a.length(); i++)
    {
        if (a[i] != b[i])
        {
            differentCount++;

            if (differentCount > 1)
                return false;
        }
    }

    return true;
}

int solution(string begin, string target, vector<string> words)
{
    int answer = 0;

    // 찾을 수 없는 경우 예외처리
    if (std::find(words.begin(), words.end(), target) == words.end())
        return answer;

    queue<pair<string,int>> q;
    vector<bool> visited;

    // 방문했는지 여부
    visited.assign(words.size(), false);

    // 초기 queue값
    q.push(make_pair(begin, 0));

    // BFS
    while (q.empty() == false && answer == 0)
    {
        string word = q.front().first;
        int depth = q.front().second;

        q.pop();

        for (int i = 0; i < words.size(); i++)
        {
            if (visited[i] == false && CanTranslate(word, words[i]) == true)
            {
                if (words[i] == target)
                {
                    answer = depth + 1;
                    break;
                }

                q.push(make_pair(words[i], depth + 1));

                visited[i] = true;
            }
        }
    }

    return answer;
}
```

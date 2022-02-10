---
title: "PROGRAMMERS 프로그래머스 : 신고 결과 받기"
excerpt: "구현"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

   이번 문제는 **구현** 문제이다.

[PROGRAMMERS : 신고 결과 받기](https://programmers.co.kr/learn/courses/30/lessons/92334)    

​        

> 문제 아이디어

​	이 문제는 적힌 내용을 그대로 구현하면 되는 문제이다. 주의할 점은 무턱대고 모든 경우를 선형적으로 돌면 시간 초과가 발생할 수 있다.

​    

>해결

```c#
using System;
using System.Collections.Generic;

public class Solution
{
    public int[] solution(string[] id_list, string[] report, int k)
    {
        Dictionary<string, List<string>> reportListDictionary = new Dictionary<string, List<string>>();
        foreach (string element in report)
        {
            string[] splited = element.Split(' ');
            string userID = splited[0];
            string reportedID = splited[1];

            if (reportListDictionary.ContainsKey(userID) == false)
            {
                reportListDictionary.Add(userID, new List<string>());
            }

            if (reportListDictionary[userID].Contains(reportedID) == false)
            {
                reportListDictionary[userID].Add(reportedID);
            }
        }

        Dictionary<string, int> reportedCountDictionary = new Dictionary<string, int>();
        foreach (KeyValuePair<string, List<string>> dic in reportListDictionary)
        {
            foreach (string element in dic.Value)
            {
                if (reportedCountDictionary.ContainsKey(element) == true)
                {
                    reportedCountDictionary[element] += 1;
                }
                else
                {
                    reportedCountDictionary[element] = 1;
                }
            }
        }

        int[] answer = new int[id_list.Length];
        for (int i = 0; i < id_list.Length; i++)
        {
            string userID = id_list[i];
            if (reportListDictionary.ContainsKey(userID) == true)
            {
                foreach (string element in reportListDictionary[userID])
                {
                    if (reportedCountDictionary[element] >= k)
                    {
                        answer[i]++;
                    }
                }
            }
        }

        return answer;
    }
}
```

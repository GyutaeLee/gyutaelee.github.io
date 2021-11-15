---
title: "PROGRAMMERS 프로그래머스 : 셔틀버스"
excerpt: "구현"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **구현** 문제이다.

[PROGRAMMERS : 셔틀버스](https://programmers.co.kr/learn/courses/30/lessons/17678)    

​        

> 문제 아이디어

​	처음 문제를 읽으면 난해함을 느낄 수 있다. 규칙은 다음과 같다.

1. 최초의 셔틀 버스는 **09:00** 시에 온다.
2. 셔틀 버스는 **t분** 간격으로 **n번** 만큼 오고, 한 번에 **m명**을 태울 수 있다.

​	해당 규칙들로 문제를 해결해야 한다. 방법은 다음과 같다.

1. **string**으로 되어있는 **timetable**을 모두 **seconds(int)**로 변경해서 **오름차순**으로 정렬한다.
2. 가장 빨리 줄을 선 크루부터 한 명씩 세어가면서 버스 줄의 마지막이 되면 해당 시간을 **string**으로 변경해서 리턴한다.
   - 마지막 버스의 마지막 번째 크루보다 1초 빠르게 오는게 정답
   - 마지막 크루까지 태웠는데도 버스의 자리가 남으면, 마지막 버스 도착 시간이 정답

​    

>해결

```c++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

int TimeToSeconds(string time)
{
    int seconds = 0;

    seconds += stoi(time.substr(0, 2)) * 60;
    seconds += stoi(time.substr(3, 2));

    return seconds;
}

string SecondsToTime(int seconds)
{
    string time;

    int hoursPart = seconds / 60;
    int secondsPart = seconds - 60 * (seconds / 60);

    time = to_string(hoursPart) + ":" + to_string(secondsPart);
    
    if (hoursPart < 10)
    {
        time.insert(0, "0");
    }

    if (secondsPart < 10)
    {
        time.insert(3, "0");
    }

    return time;
}

string solution(int n, int t, int m, vector<string> timetable)
{
    vector<int> timetableSeconds;

    for (int i = 0; i < timetable.size(); i++)
    {
        timetableSeconds.push_back(TimeToSeconds(timetable[i]));
    }
    sort(timetableSeconds.begin(), timetableSeconds.end(), less<int>());

    int currentIndex = 0, currentPassangerCount = 0;
    int remainingShuttleCount = n;
    int currentTimeSeconds = 9 * 60;
    string answer = "";
    while (true)
    {        
        while (currentTimeSeconds >= timetableSeconds[currentIndex])
        {
            currentIndex++;
            currentPassangerCount++;

            if (currentPassangerCount == m)
                break;

            if (currentIndex >= timetableSeconds.size())
                return SecondsToTime(currentTimeSeconds);            
        }

        remainingShuttleCount--;
        if (remainingShuttleCount == 0)
        {
            if (currentPassangerCount < m)
            {
                answer = SecondsToTime(currentTimeSeconds);
            }
            else
            {
                answer = SecondsToTime(timetableSeconds[currentIndex - 1] - 1);
            }
            break;
        }

        currentTimeSeconds += t;
        currentPassangerCount = 0;
    }

    return answer;
}
```

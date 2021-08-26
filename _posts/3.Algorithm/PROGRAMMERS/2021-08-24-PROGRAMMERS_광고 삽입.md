---
title: "PROGRAMMERS 프로그래머스 : 광고 삽입"
excerpt: "투 포인터(Two Pointer)"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **투 포인터(Two Pointer)** 문제이다.

[PROGRAMMERS : 광고 삽입](https://programmers.co.kr/learn/courses/30/lessons/72414)    

​        

> 문제 아이디어

​	이 문제는 입력 크기가 매우 크다. **logs** 배열의 크기가 최대 300,000 이므로 최대 **O(N)**의 시간 복잡도로 문제를 해결해야한다는 것을 눈치채고 시작해야 한다. 흐름은 다음과 같다.

1. 입력으로 주어진 시간들을 **초 단위**로 변경한다.
2. 동영상 재생시간의 최대가 **100시간**이므로, 이를 초단위로 나누어 1초마다 시청자가 몇명인지 계산해준다.
3. 광고 시간만큼 처음부터 끝까지 한 번 돌면서 최대 시청자 수가 존재하는 시간을 구해준다.

​    

>해결

```c++
#include <string>
#include <vector>
#include <utility>

using namespace std;

int ConvertTimeStringToInt(string time)
{
    int timeInt = 0;

    int convertTime = 0;
    convertTime = stoi(time.substr(0, 2));
    convertTime *= 3600;
    timeInt += convertTime;

    convertTime = stoi(time.substr(3, 2));
    convertTime *= 60;
    timeInt += convertTime;

    convertTime = stoi(time.substr(6, 2));
    timeInt += convertTime;

    return timeInt;
}

string ConvertTimeIntToString(int time)
{
    string timeString = "";

    string convertTime = to_string(time / 3600);
    if (convertTime.length() == 1) convertTime.insert(0, "0");
    timeString.append(convertTime + ":");
    time %= 3600;

    convertTime = to_string(time / 60);
    if (convertTime.length() == 1) convertTime.insert(0, "0");
    timeString.append(convertTime + ":");
    time %= 60;

    convertTime = to_string(time);
    if (convertTime.length() == 1) convertTime.insert(0, "0");
    timeString.append(convertTime);

    return timeString;
}

string solution(string play_time, string adv_time, vector<string> logs)
{    
    vector<int> adViewers(ConvertTimeStringToInt(play_time), 0);

    for (string log : logs)
    {
        int start = ConvertTimeStringToInt(log.substr(0, 8));
        int end = ConvertTimeStringToInt(log.substr(9, 8));
        for (int i = start; i < end; i++)
        {
            adViewers[i]++;
        }
    }

    int adTime = ConvertTimeStringToInt(adv_time);
    long sum = 0;
    for (int i = 0; i < adTime; i++)
    {
        sum += adViewers[i];
    }
    long maxSum = sum;
    int maxIndex = 0;

    int headIndex = 0;
    int endTime = adViewers.size();
    for (int i = adTime; i < endTime; i++, headIndex++)
    {
        sum -= adViewers[headIndex];
        sum += adViewers[i];

        if (maxSum < sum)
        {
            maxSum = sum;
            maxIndex = headIndex + 1;
        }
    }

    string answer = ConvertTimeIntToString(maxIndex);
    return answer;
}
```

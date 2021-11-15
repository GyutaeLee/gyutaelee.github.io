---
title: "PROGRAMMERS 프로그래머스 : 추석 트래픽"
excerpt: "구현"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **구현** 문제이다.

[PROGRAMMERS : 추석 트래픽](https://programmers.co.kr/learn/courses/30/lessons/17676)    

​        

> 문제 아이디어

​	처음에 문제에 접근할 때에는 단순히 **1초** 단위로 쪼개서 계산을 하면 되는 걸로 문제를 착각했다. 하지만 milli second 까지 접근해야 문제가 풀린다.

​	예를 들면 단순히 **1~2초** 처리량만 존재하는게 아니라, **1.012~2.012초** 처리량도 존재할 수 있다는 것이다. 문제를 해결하는 방법은 다음과 같다.

1. 로그 데이터를 **응답 완료 시간/처리 시간**으로 변환해준다. 이때 기준은 **milli second**이다.
2. 변환한 **응답 완료 시간**과 **처리 시간**은  vector를 하나씩 만들어서 저장한다.
3. 변환 및 저장이 모두 끝났으면 2중 for문을 통해 문제를 해결한다.
4. 먼저 **끝나는 시간(endTime)**을 만든다.
   - 문제에서 원하는건 **초당 처리량** 이기 때문에 **1000**의 가중치를 더해서 **endTime** 변수를 만든다.
5. 2중 for문에서는 다음 인덱스부터 마지막 인덱스까지 비교를 진행한다.
6. 각 vector 인덱스에 저장되어 있는 **응답 완료 시간/처리 시간**을 기준으로 **시작 시간(startTime)**을 만든다.
   - 이때 **'1'**을 더해주는 이유는 단순히 세보면 안다. (빼면 1초의 오차가 생김)
7. 각 vector 인덱스의 **시작 시간(startTime)**이 **끝나는 시간(endTime)**보다 작으면 초당 처리량을 늘려준다.
   - 이렇게 문제를 해결할 수 있는 이유는 본문에 **lines 배열**이 **응답완료 시간**을 기준으로 **오름차순** 정렬되어 있기 때문이다. (뒤쪽 **vector index의 응답 완료 시간이 무조건 더 먼 시간**이므로, 시작 시간만 더 빠르면 무조건 커버된다.)
8. 2중 for문이 끝나면 초당 처리량과, 이전 최대 초당 처리량을 비교해가면서 최대값을 answer에 저장한다.
9. 마지막에는 answer에 1을 더해준다. (바깥쪽 for문의 비교하는 처리량이 들어가지 않았기 때문)

​    

>해결

```c++
#include <string>
#include <vector>
#include <utility>

using namespace std;

int ConvertLineToResponseCompletionTimeMilliSeconds(string line)
{
    int hours = stoi(line.substr(11, 2)) * 3600000; 
    int minutes = stoi(line.substr(14, 2)) * 60000;
    int seconds = stoi(line.substr(17, 2)) * 1000;
    int milliseconds = stoi(line.substr(20, 3));

    return hours + minutes + seconds + milliseconds;
}

int ConvertLineToProcessingTimeMilliSeconds(string line)
{
    float processingTime = stoi(line.substr(24, 1)) * 1000;

    if (line[25] == '.')
    {
        int secondTextIndex = line.find('s');
        int processingTimeDemicalPlaces = stoi(line.substr(26, line.length() - (secondTextIndex - 25)));
        processingTime += processingTimeDemicalPlaces;
    }

    return processingTime;
}

int solution(vector<string> lines)
{
    vector<int> throughputPerSecond(86401, 0);
    int answer = 0;

    vector<int> responseCompleteSeconds, processingTimes;
    for (int i = 0; i < lines.size(); i++)
    {
        int responseCompleteSecond = ConvertLineToResponseCompletionTimeMilliSeconds(lines[i]);
        int processingTime = ConvertLineToProcessingTimeMilliSeconds(lines[i]);

        responseCompleteSeconds.push_back(responseCompleteSecond);
        processingTimes.push_back(processingTime);
    }

    for (int i = 0; i < lines.size(); i++)
    {
        int throughputPerSecond = 0;
        int endTime = responseCompleteSeconds[i] + 1000;
        for (int j = i + 1; j < lines.size(); j++)
        {
            int startTime = responseCompleteSeconds[j] - processingTimes[j] + 1;
            if (startTime < endTime)
            {
                throughputPerSecond++;
            }
        }
        answer = max(throughputPerSecond, answer);
    }

    return answer + 1;
}
```

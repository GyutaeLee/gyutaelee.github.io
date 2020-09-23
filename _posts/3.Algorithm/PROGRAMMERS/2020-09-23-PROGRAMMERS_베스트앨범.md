---
title: "PROGRAMMERS 프로그래머스 : 베스트앨범"
excerpt: "해시(HASH)"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **해시(HASH)** 문제이다.

[PROGRAMMERS : 베스트앨범](https://programmers.co.kr/learn/courses/30/lessons/42579)    

​    

> 문제 이해

   문제를 읽어보자. 먼저 이 문제에서 나오는 변수는 총 2가지이다.

- vector<string> genres : 음악 장르 종류
- vector<int> plays : 음악 재생 수

​    

> 문제 아이디어

​	이 문제의 핵심은 장르끼리도 비교하고, 동일한 장르의 노래끼리도 비교하고, 재생 횟수가 같은 노래 끼리는 고유 번호로도 비교해야한다는 점이다. 

​	우선 장르가 string 값으로 주어지기 때문에 hash table 개념을 사용하기에 좋으므로 **unordered_map**을 사용했다. 그리고 장르마다 총 재생 수를 저장할 때에는 하나의 키에 하나의 밸류만 사용하면 되므로 **map**을 사용했고, 조건 1번인 많이 재생된 장르부터 차례대로 확인하기 위해 vector를 하나 더 만들어서 정렬한 후에 사용했다.

​    

>해결

```c++
#include <string>
#include <vector>
#include <map>
#include <set>
#include <unordered_map>
#include <algorithm>

using namespace std;

bool compare_playCountMap(const pair<string, int>& a, const pair<string, int>& b)
{
	return a.second > b.second;
}

bool compare_indexVector(const pair<int, int>& a, const pair<int, int>& b)
{
	return a.second > b.second;
}

vector<int> solution(vector<string> genres, vector<int> plays)
{
	vector<int> answer;

	// <genres, <index, plays>>
	unordered_multimap<string, pair<int, int>> hashMap;

	// <genres, play count>
	map<string, int> playCountMap;

	// <genres, play count>
	vector<pair<string, int>> playCountVector;

	for (int i = 0; i < genres.size(); i++)
	{
		// 1. genres를 키로 맵 저장
		hashMap.insert(make_pair(genres[i], make_pair(i, plays[i])));

		// 2. genre별 총 재생 수 저장
		if (playCountMap.find(genres[i]) == playCountMap.end())
		{
			playCountMap.insert(make_pair(genres[i], plays[i]));
		}
		else
		{
			playCountMap[genres[i]] += plays[i];
		}

	}

	// 3. genre 총 재생 수로 정렬
	for (map<string, int>::iterator iter = playCountMap.begin(); iter != playCountMap.end(); iter++)
	{
		playCountVector.push_back(make_pair(iter->first, iter->second));
	}

	sort(playCountVector.begin(), playCountVector.end(), compare_playCountMap);

	// 4. 정렬된 genres 순회
	for (int i = 0; i < playCountVector.size(); i++)
	{
		vector<pair<int, int>> indexVector;

		// 5. genres에 담겨있는 곡들과 플레이 수를 벡터에 넣고
		unordered_multimap<string, pair<int, int>>::iterator iter = hashMap.find(playCountVector[i].first);
		for (int j = 0; j < hashMap.count(playCountVector[i].first); j++, iter++)
		{
			indexVector.push_back(make_pair(iter->second.first, iter->second.second));
		}

		// 6. 고유 번호가 낮은 순으로 정렬하고
		sort(indexVector.begin(), indexVector.end(), less<pair<int, int>>());

		// 7. 플레이 수가 높은 순에 따라 정렬한 후에
		sort(indexVector.begin(), indexVector.end(), compare_indexVector);

		// 7. 최대 2개까지 넣는다.
		for (int j = 0; j < 2 && j < indexVector.size(); j++)
		{
			answer.push_back(indexVector[j].first);
		}
	}

	return answer;
}
```

---
title: "PROGRAMMERS 프로그래머스 : 다단계 칫솔 판매"
excerpt: "DFS/BFS & Map"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **DFS/BFS & Map** 문제이다.

[PROGRAMMERS : 다단계 칫솔 판매](https://programmers.co.kr/learn/courses/30/lessons/77486)    

​    

> 문제 이해

   이 문제는 지문이 많이 길다. 그래서 그림과 함께 잘 읽어야 한다. 따로 설명하는거보다는 지문을 잘 읽고 들어가자.

​    

> 문제 아이디어

​	이 문제의 핵심은 **이득**을 합해서 계산하면 안 되고, 하나 하나 **따로** 계산을 해주어야한다는 점이다. 문제 조건에 **seller**에 같은 이름이 **중복**해서 들어있을 수 있다고 하는데, 이 중복을 하나로 합한 후 계산하려고 하면 답이 틀렸다고 나온다.

​	두 번째로는 이익 계산은 **상위 조직원**을 기준으로 이익을 계산한 후, 남은 이익을 **seller**가 가져가야 한다. 예를 들면, seller가 **21원** 이득을 봤을 때에는 상위 조직원에게 10%를 줘야하므로, **21 * 0.1**로 **2.1**이 나오는데, 정수로 만들기 위해 **0.1**을 떼면 **2**가 상위 조직원의 이익이고, 남은 **19**가 **seller**의 이득이다.

​	만약 **seller**에도 따로 이익 계산이 들어가서 **21 * 0.9**를 해버리면 **18.9**가 나오고, **0.9**를 떼버리면 **18**이 나오므로 **0.9**가 공중 분해되서 사라진다. 이 점을 주의하자.

​	마지막으로, 나는 편하게 값들에 접근하기 위해서 **Map**을 사용했는데, 다른 방법을 사용해서 진행해도 된다. 알고리즘은 단순하다.

1. **enroll**의 순서가 맨 뒤가 다단계의 가장 아래이므로, 맨 뒤부터 인덱싱한다.
2. **seller** 본인의 이익을 계산하고, **senior**의 이익을 계산해서 올려보내준다.
   - 이 때 **senior**의 이익은 vector에 하나씩 따로 담는다. 위에서 말했듯이 **이득**은 합해서 계산하면 안 되고, 각각 따로 계산해야한다.
3. 2번 과정을 모든 **seller**에서 반복해서 답을 구한다.

​    

>해결

```c++
#include <string>
#include <vector>
#include <unordered_map>

using namespace std;

#define SALE_PROFIT (100);

void CalculateSavedProfit(unordered_map<string, vector<int>>& profitMap, string sellerName, string seniorName)
{
	for (int i = 0; i < profitMap[sellerName].size(); i++)
	{
		int savedProfit = profitMap[sellerName][i];

		if (savedProfit * 0.1f < 1)
		{
			continue;
		}

		int seniorProfit = savedProfit * 0.1f;
		int sellerProfit = savedProfit - seniorProfit;

		profitMap[sellerName][i] = sellerProfit;
		profitMap[seniorName].push_back(seniorProfit);
	}
}

void CaculateSalesProfit(unordered_map<string, vector<int>>& profitMap, unordered_map<string, vector<int>>& sellerMap, string sellerName, string seniorName)
{
	for (int i = 0; i < sellerMap[sellerName].size(); i++)
	{
		int totalProfit = sellerMap[sellerName][i] * SALE_PROFIT;
		int sellerProfit = 0;
		int seniorProfit = 0;
		if (totalProfit * 0.1f < 1)
		{
			sellerProfit = totalProfit;
			seniorProfit = 0;
		}
		else
		{
			seniorProfit = totalProfit * 0.1f;
			sellerProfit = totalProfit - seniorProfit;
		}

		profitMap[sellerName].push_back(sellerProfit);
		profitMap[seniorName].push_back(seniorProfit);
	}
}

void CaculateSellerProfit(unordered_map<string, vector<int>>& profitMap, vector<int>& answer, string sellerName, int answerIndex)
{
	for (int i = 0; i < profitMap[sellerName].size(); i++)
	{
		answer[answerIndex] += profitMap[sellerName][i];
	}
}

vector<int> solution(vector<string> enroll, vector<string> referral, vector<string> seller, vector<int> amount)
{
	vector<int> answer(enroll.size(), 0);

	unordered_map<string, vector<int>> sellerMap;
	for (int i = 0; i < seller.size(); ++i)
	{
		sellerMap[seller[i]].push_back(amount[i]);
	}

	unordered_map<string, vector<int>> profitMap;
	for (int i = enroll.size() - 1; i >= 0; i--)
	{
		string sellerName = enroll[i];
		string seniorName = referral[i];

		CalculateSavedProfit(profitMap, sellerName, seniorName);
		CaculateSalesProfit(profitMap, sellerMap, sellerName, seniorName);
		CaculateSellerProfit(profitMap, answer, sellerName, i);
	}

	return answer;
}
```

---
title: "PROGRAMMERS 프로그래머스 : 블록 게임"
excerpt: "구현"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **구현** 문제이다.

[PROGRAMMERS : 블록 게임](https://programmers.co.kr/learn/courses/30/lessons/42894)    

​        

> 문제 아이디어

​	이 문제는 여러 조건에 부합하게 구현해야하는 문제이다. board의 최대 크기가 **50x50=250**이므로 계산의 크기가 작음을 알 수 있다. 문제를 읽어보면 문제를 해결할 수 있는 경우의 수가 제한되어 있으므로 모든 경우를 계산해서 답을 구하면 된다.

- case 1 : 가로 + 위쪽 왼쪽 1개
- case 2 : 가로 + 위쪽 가운데 1개
- case 3 : 가로 + 위쪽 오른쪽 1개
- **case 4 : 가로 + 그외**
- case 5 : 세로 + 왼쪽 맨 아래 1개
- case 6 : 세로 + 오른쪽 맨 아래 1개
- **case 7 : 세로 + 그외**

​	7개의 case 중에 **4, 7**을 제외한 5가지의 경우만이 답을 낼 수 있는 케이스이다. 해당 케이스들을 구해서 블록을 쌓았을 때 제거 될 수 있는지까지 계산하면 답을 구할 수 있다.

​    

>해결

```c++
#include <string>
#include <vector>

using namespace std;

// case 1 : 가로 + 위쪽 왼쪽 1개
// case 2 : 가로 + 위쪽 가운데 1개
// case 3 : 가로 + 위쪽 오른쪽 1개
// case 4 : 가로 + 그외
// case 5 : 세로 + 왼쪽 맨 아래 1개
// case 6 : 세로 + 오른쪽 맨 아래 1개
// case 7 : 세로 + 그외
enum BlockCase
{
	BlockCase_1 = 1,
	BlockCase_2 = 2,
	BlockCase_3 = 3,
	BlockCase_4 = 4,
	BlockCase_5 = 5,
	BlockCase_6 = 6,
	BlockCase_7 = 7,
};

class Block
{
private:
	void DetermineCaseNumber()
	{
		/*
		* 가로
		*/ 
		if (this->Points[0].first == this->Points[1].first && this->Points[0].first == this->Points[2].first)
		{
			if (this->Points[3].first == this->Points[0].first - 1)
			{
				if (this->Points[3].second == this->Points[0].second)
				{
					this->BlockCase = BlockCase_1;
				}
				else if (this->Points[3].second == this->Points[1].second)
				{
					this->BlockCase = BlockCase_2;
				}
				else if (this->Points[3].second == this->Points[2].second)
				{
					this->BlockCase = BlockCase_3;
				}
			}
			else if (this->Points[3].first == this->Points[0].first + 1)
			{
				this->BlockCase = BlockCase_4;
			}
		}
		else if (this->Points[1].first == this->Points[2].first && this->Points[1].first == this->Points[3].first)
		{
			if (this->Points[0].first == this->Points[1].first - 1)
			{
				if (this->Points[0].second == this->Points[1].second)
				{
					this->BlockCase = BlockCase_1;
				}
				else if (this->Points[0].second == this->Points[2].second)
				{
					this->BlockCase = BlockCase_2;
				}
				else if (this->Points[0].second == this->Points[3].second)
				{
					this->BlockCase = BlockCase_3;
				}
			}
			else if (this->Points[0].first == this->Points[1].first + 1)
			{
				this->BlockCase = BlockCase_4;
			}
		}
		/*
		* 세로
		*/ 
		else if (this->Points[1].second == this->Points[2].second && this->Points[1].second == this->Points[3].second)
		{
			if (this->Points[0].first == this->Points[3].first)
			{
				if (this->Points[0].second == this->Points[3].second - 1)
				{
					this->BlockCase = BlockCase_5;
				}
				else if (this->Points[0].second == this->Points[3].second + 1)
				{
					this->BlockCase = BlockCase_6;
				}
			}
			else
			{
				this->BlockCase = BlockCase_7;
			}
		}
		else if (this->Points[0].second == this->Points[2].second && this->Points[0].second == this->Points[3].second)
		{
			if (this->Points[1].first == this->Points[3].first)
			{
				if (this->Points[1].second == this->Points[3].second - 1)
				{
					this->BlockCase = BlockCase_5;
				}
				else if (this->Points[1].second == this->Points[3].second + 1)
				{
					this->BlockCase = BlockCase_6;
				}
			}
			else
			{
				this->BlockCase = BlockCase_7;
			}
		}
		else if (this->Points[0].second == this->Points[1].second && this->Points[0].second == this->Points[3].second)
		{
			if (this->Points[2].first == this->Points[3].first)
			{
				if (this->Points[2].second == this->Points[3].second - 1)
				{
					this->BlockCase = BlockCase_5;
				}
				else if (this->Points[2].second == this->Points[3].second + 1)
				{
					this->BlockCase = BlockCase_6;
				}
			}
			else
			{
				this->BlockCase = BlockCase_7;
			}
		}
		else if (this->Points[0].second == this->Points[1].second && this->Points[0].second == this->Points[2].second)
		{
			if (this->Points[3].first == this->Points[2].first)
			{
				if (this->Points[3].second == this->Points[2].second - 1)
				{
					this->BlockCase = BlockCase_5;
				}
				else if (this->Points[3].second == this->Points[2].second + 1)
				{
					this->BlockCase = BlockCase_6;
				}
			}
			else
			{
				this->BlockCase = BlockCase_7;
			}
		}
	}

public:
	vector<pair<int, int>> Points;
	BlockCase BlockCase;
	bool IsErased;

	Block(const vector<pair<int, int>> p)
	{
		this->Points.assign(p.begin(), p.end());
		this->IsErased = false;
		DetermineCaseNumber();
	}

	bool CanErased(const vector<vector<int>>& board)
	{
		bool canErased = true;
		pair<int, int> point01, point02;

		switch (this->BlockCase)
		{
		case BlockCase_1:
			point01.first = this->Points[0].first;
			point01.second = this->Points[0].second + 1;

			point02.first = this->Points[0].first;
			point02.second = this->Points[0].second + 2;

			for (int i = 0; i <= point01.first; i++)
			{
				if (board[i][point01.second] != 0 || board[i][point02.second] != 0)
				{
					canErased = false;
					break;
				}
			}
			break;
		case BlockCase_2:
			point01.first = this->Points[0].first;
			point01.second = this->Points[0].second - 1;

			point02.first = this->Points[0].first;
			point02.second = this->Points[0].second + 1;

			for (int i = 0; i <= point01.first; i++)
			{
				if (board[i][point01.second] != 0 || board[i][point02.second] != 0)
				{
					canErased = false;
					break;
				}
			}
			break;
		case BlockCase_3:
			point01.first = this->Points[0].first;
			point01.second = this->Points[0].second - 2;

			point02.first = this->Points[0].first;
			point02.second = this->Points[0].second - 1;

			for (int i = 0; i <= point01.first; i++)
			{
				if (board[i][point01.second] != 0 || board[i][point02.second] != 0)
				{
					canErased = false;
					break;
				}
			}
			break;
		case BlockCase_5:
			for (int i = 0; i <= this->Points[2].first - 1; i++)
			{
				if (board[i][this->Points[2].second] != 0)
				{
					canErased = false;
					break;
				}
			}
			break;
		case BlockCase_6:
			for (int i = 0; i <= this->Points[3].first - 1; i++)
			{
				if (board[i][this->Points[3].second] != 0)
				{
					canErased = false;
					break;
				}
			}
			break;
		case BlockCase_4:
		case BlockCase_7:
			canErased = false;
			break;
		default:
			canErased = false;
			break;
		}

		return canErased;
	}
};

int solution(vector<vector<int>> board)
{
	vector<vector<pair<int, int>>> blockPoints(201);

	for (int i = 0; i < board.size(); i++)
	{
		for (int j = 0; j < board[i].size(); j++)
		{
			if (board[i][j] == 0)
				continue;

			blockPoints[board[i][j]].push_back({ i ,j });
		}
	}

	vector<Block> blocks;
	for (int i = 1; i < blockPoints.size(); i++)
	{
		if (blockPoints[i].size() == 0)
			continue;

		blocks.push_back(Block(blockPoints[i]));
	}

	int answer = 0;
	bool isOneMoreLoop = true;
	while (isOneMoreLoop)
	{
		isOneMoreLoop = false;

		for (int i = 0; i < blocks.size(); i++)
		{
			if (blocks[i].IsErased == true)
				continue;

			if (blocks[i].CanErased(board) == true)
			{
				for (int j = 0; j < blocks[i].Points.size(); j++)
				{
					board[blocks[i].Points[j].first][blocks[i].Points[j].second] = 0;
				}

				blocks[i].IsErased = true;
				isOneMoreLoop = true;
				answer++;
			}
		}
	}

	return answer;
}
```

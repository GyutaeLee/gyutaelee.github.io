---
title: "PROGRAMMERS 프로그래머스 : 행렬 테두리 회전하기"
excerpt: "구현"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 단순한 **구현** 문제이다.

[PROGRAMMERS : 행렬 테두리 회전하기](https://programmers.co.kr/learn/courses/30/lessons/77485)    

​        

> 문제 아이디어

​	이 문제는 문제에 적힌대로 구현만 하면 된다. 주의할 점은 **행과 열**의 순서를 헷갈리면 문제가 길어질 수 있다.

​    

>해결

```c++
#include <string>
#include <vector>
#include <limits.h>
#include <utility>

using namespace std;

int RotateMatrixAndGetMinimumValue(vector<vector<int>>& matrix, const vector<int>& query)
{
	int startRow = query[0];
	int startColumn = query[1];
	int endRow = query[2];
	int endColumn = query[3];
		
	int minimumValue = INT_MAX;
	pair<int, int> currentPoint = make_pair(startRow, startColumn);	

	int beforeValue = matrix[currentPoint.first][currentPoint.second];
	for (int i = startColumn + 1; i <= endColumn; i++)
	{
		minimumValue = min(minimumValue, beforeValue);

		int currentValue = matrix[currentPoint.first][i];
		matrix[currentPoint.first][i] = beforeValue;
		beforeValue = currentValue;
	}
	currentPoint.second = endColumn;

	for (int i = startRow + 1; i <= endRow; i++)
	{
		minimumValue = min(minimumValue, beforeValue);

		int currentValue = matrix[i][currentPoint.second];
		matrix[i][currentPoint.second] = beforeValue;
		beforeValue = currentValue;
	}
	currentPoint.first = endRow;

	for (int i = endColumn - 1; i >= startColumn; i--)
	{
		minimumValue = min(minimumValue, beforeValue);

		int currentValue = matrix[currentPoint.first][i];
		matrix[currentPoint.first][i] = beforeValue;
		beforeValue = currentValue;
	}
	currentPoint.second = startColumn;

	for (int i = endRow - 1; i >= startRow; i--)
	{
		minimumValue = min(minimumValue, beforeValue);

		int currentValue = matrix[i][currentPoint.second];
		matrix[i][currentPoint.second] = beforeValue;
		beforeValue = currentValue;
	}

	return minimumValue;
}

vector<int> solution(int rows, int columns, vector<vector<int>> queries)
{
	vector<vector<int>> matrix;

	matrix.resize(rows + 1);
	for (int i = 1; i <= rows; i++)
	{
		matrix[i].resize(columns + 1);
		for (int j = 1; j <= columns; j++)
		{
			matrix[i][j] = (i - 1) * columns + j;
		}
	}

	vector<int> answer(queries.size());
	for (int i = 0; i < queries.size(); i++)
	{
		int minimumValue = RotateMatrixAndGetMinimumValue(matrix, queries[i]);
		answer[i] = minimumValue;
	}

	return answer;
}
```

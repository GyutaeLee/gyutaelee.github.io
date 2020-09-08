---
title: "PROGRAMMERS 프로그래머스 : 자물쇠와 열쇠"
excerpt: "2차원 배열(2D Array)"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **2차원 배열(2D Array)** 문제이다.

[PROGRAMMERS : 자물쇠와 열쇠](https://programmers.co.kr/learn/courses/30/lessons/60059)    

​    

> 문제 이해

   문제를 읽어보자. 먼저 이 문제에서 나오는 변수는 총 2가지이다.

- key : 2차원 열쇠 벡터
- lock : 2차원 자물쇠 벡터

​    

> 문제 아이디어

​	이 문제의 핵심은 key와 lock의 최대 크기가 크지 않다는 점이다. 그렇기에 모든 경우의 수를 다 시도해서 해결하면 된다. 순서는 다음과 같다.

1. 자물쇠의 크기의 3배가 되는 큰 자물쇠를 만들고, 중앙에 자물쇠의 값을 둔다.
2. 자물쇠의 배열에 키를 넣을 수 있는 모든 경우의 수를 다 해보고, 자물쇠를 열 수 있는지 확인한다. (+ 회전)

주의해야 할 점은 자물쇠와 열쇠의 크기는 동일하지 않을  수도 있다는 점이다.

​    

>해결

```c++
#include <string>
#include <vector>

using namespace std;

void GetRightRotatedVector(vector<vector<int>>& v)
{
    int size = v.size();
    vector<vector<int>> arr(size, vector<int>(size));

    for (int i = 0; i < v.size(); i++)
    {
        arr[i].resize(v[i].size());
        for (int j = 0; j < v[i].size(); j++)
        {
            arr[i][j] = v[size - 1 - j][i];
        }
    }

    v = arr;
}

bool CheckKeyAndBigRock(int low, int column, vector<vector<int>>& key, vector<vector<int>> bigLock)
{
    int lockSize = bigLock.size() / 3;
    int doubleLockSize = lockSize * 2;

    // lock 전체 맵에 자물쇠 값을 더해준다.
    for (int i = 0; i < key.size(); i++)
    {
        for (int j = 0; j < key.size(); j++)
        {
			bigLock[i + low][j + column] += key[i][j];
        }
    }

    // 조건 1 : 자물쇠 돌기 부분과 열쇠 돌기 부분이 겹치면 안 된다 -> 값이 2가 나옴
    // 조건 2 : 모든 부분의 홈이 없어야 한다 -> 값이 0이 나옴
    // --> 값이 1이 아니면 답이 아님
    for (int i = lockSize; i < doubleLockSize; i++)
    {
        for (int j = lockSize; j < doubleLockSize; j++)
        {
            if (bigLock[i][j] != 1)
            {
                return false;
            }
        }
    }

    return true;
}

bool solution(vector<vector<int>> key, vector<vector<int>> lock) 
{
    bool answer = false;

    vector<vector<int>> rotatedKey;
    vector<vector<int>> bigLock;
    
    int lockSize = lock.size();
    int doubleLockSize = lockSize * 2;

    // 1. 3배 사이즈의 lock을 만든다.
    bigLock.resize(lockSize * 3);
    for (int i = 0; i < lockSize * 3; i++)
    {
        bigLock[i].resize(lockSize * 3);
    }

    for (int i = lockSize; i < doubleLockSize; i++)
    {
        for (int j = lockSize; j < doubleLockSize; j++)
        {
            bigLock[i][j] = lock[i - lockSize][j - lockSize];
        }
    }

    rotatedKey.assign(key.begin(), key.end());
    for (int rotate = 0; rotate < 4; rotate++)
    {
        // 2. 90도, 180도, 270도, 360도 총 4종류의 키를 집어넣어본다.
        GetRightRotatedVector(rotatedKey);

        // 3. 0 ~ lock에 영향을 미치는 곳까지 전부 대입해본다.
        for (int i = 0; i < doubleLockSize; i++)
        {
            for (int j = 0; j < doubleLockSize; j++)
            {
                answer = CheckKeyAndBigRock(i, j, rotatedKey, bigLock);

                if (answer == true)
                    goto finish;
            }
        }
    }

finish:
    return answer;
}
```

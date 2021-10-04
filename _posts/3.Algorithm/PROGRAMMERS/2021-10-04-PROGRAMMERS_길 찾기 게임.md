---
title: "PROGRAMMERS 프로그래머스 : 길 찾기 게임"
excerpt: "이진 트리(Binary Tree)"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **이진 트리(Trie)** 문제이다.

[PROGRAMMERS : 길 찾기 게임](https://programmers.co.kr/learn/courses/30/lessons/42892)    

​        

> 문제 아이디어

​	이 문제는 어찌 보면 단순하다. **x, y 좌표**를 기준 값으로 트리를 만든 후에 전위/후위 순회를 해주면 문제가 해결된다. 방법은 다음과 같다.

1. 주어진 노드 정보들을**y 좌표 값**을 기준으로 정렬시킨다.
2. 첫번째 인덱스에 있는 값을 **root**로 이진 트리를 만든다.
3. 이진 트리를 전위/후위 순회해준다.

​    

>해결

```c++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

class Node
{
public:
    bool isRoot;
    int number;
    int x, y;
    Node *left, *right;

    Node(Node *node, Node *l, Node *r)
    {
        this->isRoot = node->isRoot;
        this->number = node->number;
        this->x = node->x;
        this->y = node->y;
        this->left = node->left;
        this->right = node->right;
    }

    Node(bool i, int n, int x, int y, Node *l, Node *r)
    {
        this->isRoot = i;
        this->number = n;
        this->x = x;
        this->y = y;
        this->left = l;
        this->right = r;
    }

    void Push(Node* node)
    {
        if (this->x > node->x)
        {
            if (this->left == NULL)
            {
                this->left = node;
            }
            else
            {
                this->left->Push(node);
            }
        }
        else
        {
            if (this->right == NULL)
            {
                this->right = node;
            }
            else
            {
                this->right->Push(node);
            }
        }
    }

    void MakePreorderCircuit(vector<int> &circuit)
    {
		circuit.push_back(this->number);

        if (this->left != NULL)
        {
            this->left->MakePreorderCircuit(circuit);
        }
        
        if (this->right != NULL)
        {
            this->right->MakePreorderCircuit(circuit);
        }
    }

    void MakePostorderCircuit(vector<int>& circuit)
    {
        if (this->left != NULL)
        {
            this->left->MakePostorderCircuit(circuit);
        }

        if (this->right != NULL)
        {
            this->right->MakePostorderCircuit(circuit);
        }

        circuit.push_back(this->number);
    }
};

bool _compare(const Node& a, const Node& b)
{
    if (a.y > b.y)
        return true;

    if (a.y == b.y && a.x < b.x)
        return true;

    return false;        
}

vector<vector<int>> solution(vector<vector<int>> nodeinfo)
{
    vector<Node> nodeCoordinate;
    for (int i = 0; i < nodeinfo.size(); i++)
    {
        int x = nodeinfo[i][0];
        int y = nodeinfo[i][1];
        nodeCoordinate.push_back(Node(false, i + 1, x, y, NULL, NULL));
    }

    sort(nodeCoordinate.begin(), nodeCoordinate.end(), _compare);
    nodeCoordinate[0].isRoot = true;

    Node *root = &nodeCoordinate[0];
    for (int i = 1; i < nodeCoordinate.size(); i++)
    {
        root->Push(&nodeCoordinate[i]);
    }

    vector<int> preorderCircuit, postorderCircuit;    

    root->MakePreorderCircuit(preorderCircuit);
    root->MakePostorderCircuit(postorderCircuit);

    vector<vector<int>> answer;
    answer.push_back(preorderCircuit);
    answer.push_back(postorderCircuit);
    return answer;
}
```

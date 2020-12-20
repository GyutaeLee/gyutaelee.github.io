---
title: "Segment Tree Implementation Code (C++)"
excerpt: "세그먼트 트리 구현 코드(Segment Tree Code - C++)"

categories:
- Algorithm

tags:
- Introduction To Algorithms

---

[theory link 1](https://gyutaelee.github.io/algorithm/Efficient-and-easy-segment-trees/)

[theory link 2](https://gyutaelee.github.io/algorithm/Lazy-Propagation/)

​    

> Read

​	long long 데이터를 기준으로 만들어진 클래스이다. 기본 토대를 바탕으로 본인이 필요한 데이터로 치환해서 사용하거나 template으로 변경해서 사용하면 된다.

​	알고리즘 공부용으로 **검증이 완벽히 되지 않은 코드**이니 본인이 사용할 때 완벽하게 검증을 해보는 것을 추천한다. ([BOJ 10999](https://www.acmicpc.net/problem/10999) 문제를 LPSegmentTree.h 로 해결했다.)

​    

> SegmentTree.h

```c++
class SegmentTree
{
    private:
        std::vector<long long> tree;
        int treeSize;
    public:
        void Input(int treeSize);
        void Build();
        void Push();
        void ModifyElement(int position, long long value);
        void ModifyInterval(int left, int right, long long value);
        long long QueryElement(int position);
        long long QueryInterval(int left, int right);      

        void Print(); 
};

void SegmentTree::Input(int treeSize)
{
    this->treeSize = treeSize;
    this->tree.resize(this->treeSize * 2);

    for (int i = 0; i < this->treeSize; i++)
    {
        std::cin >> this->tree[this->treeSize + i];
    }
}

void SegmentTree::Build()
{
    for (int i = this->treeSize - 1; i > 0; i--)
    {
        this->tree[i] = this->tree[i<<1] + this->tree[i<<1|1];
    }
}

void SegmentTree::Push()
{
    for (int i = 1; i < this->treeSize; i++)
    {
        this->tree[i<<1] += tree[i];
        this->tree[i<<1|1] += tree[i];
        this->tree[i] = 0;
    }
}

void SegmentTree::ModifyElement(int position, long long value)
{
    for (this->tree[position += this->treeSize] = value; position > 1; position >>= 1)
    {
        this->tree[position>>1] = this->tree[position] + this->tree[position^1];
    }
}

void SegmentTree::ModifyInterval(int left, int right, long long value)
{
    for (left += this->treeSize, right += this->treeSize; left < right; left >>= 1, right >>= 1)
    {
        if (left & 1)
        {
            this->tree[left++] += value;
        }
        if (right & 1)
        {
            tree[--right] += value;
        }
    }
}

long long SegmentTree::QueryElement(int position)
{
    long long result = 0;

    for (position += this->treeSize; position > 0; position >>= 1)
    {
        result += this->tree[position];
    }

    return result;
}

// sum on interval [left, right)
long long SegmentTree::QueryInterval(int left, int right)
{
    long long result = 0;

    for (left += this->treeSize, right += this->treeSize; left < right; left >>= 1, right >>= 1)
    {
        if (left & 1)
        {
            result += this->tree[left++];
        }
        if (right & 1)
        {
            result += this->tree[--right];
        }
    }

    return result;
}

void SegmentTree::Print()
{
    for (int i = 1; i < this->tree.size(); i++)
    {
        std::cout << this->tree[i] << " ";
    }
    std::cout << "\n";
}
```

​    

> LPSegmentTree.h (Lazy Propagation Segment Tree)

```c++
/*
    Lazy Propagation Segment Tree
*/
class LPSegmentTree
{
private:
    std::vector<long long> tree;
    std::vector<long long> d;
    int treeSize;
    int treeHeight;

public:
    void Input(int treeSize);
    void BuildInterval(int left, int right);
    void Calculate(int position, int k);
    void Apply(int position, long long value, int k);
    void PushInterval(int left, int right);
    void ModifyInterval(int left, int right, long long value);
    long long QueryInterval(int left, int right);

    void Print();
};

void LPSegmentTree::Input(int treeSize)
{
    this->treeSize = treeSize;
    this->treeHeight = sizeof(int) * 8 - __builtin_clz(treeSize);
    this->tree.resize(this->treeSize * 2);
    this->d.resize(this->treeSize);

    for (int i = 0; i < this->treeSize; i++)
    {
        std::cin >> this->tree[this->treeSize + i];
    }
}

void LPSegmentTree::BuildInterval(int left, int right)
{
    int k = 2;
    for (left += this->treeSize, right += this->treeSize - 1; left > 1; k <<= 1)
    {
        left >>= 1;
        right >>= 1;
        for (int i = right; i >= left; i--)
        {
            Calculate(i, k);
        }
    }
}

void LPSegmentTree::Calculate(int position, int k)
{
    if (this->d[position] == 0)
    {
        this->tree[position] = this->tree[position << 1] + this->tree[position << 1 | 1];
    }
    else
    {
        this->tree[position] += this->d[position] * k;
    }
}

void LPSegmentTree::Apply(int position, long long value, int k)
{
    this->tree[position] += value * k;
    if (position < this->treeSize)
    {
        this->d[position] += value;
    }
}

void LPSegmentTree::PushInterval(int left, int right)
{
    int s = this->treeHeight;
    int k = 1 << (this->treeHeight - 1);
    for (left += this->treeSize, right += this->treeSize - 1; s > 0; s--, k >>= 1)
    {
        for (int i = left >> s; i <= right >> s; i++)
        {
            if (this->d[i] == 0)
                continue;

            Apply(i << 1, this->d[i], k);
            Apply(i << 1 | 1, this->d[i], k);
            this->d[i] = 0;
        }
    }
}

void LPSegmentTree::ModifyInterval(int left, int right, long long value)
{
    if (value == 0)
        return;

    PushInterval(left, left + 1);
    PushInterval(right - 1, right);

    bool cLeft = false, cRight = false;
    int k = 1;
    for (left += this->treeSize, right += this->treeSize; left < right; left >>= 1, right >>= 1, k <<= 1)
    {
        if (cLeft == true)
        {
            Calculate(left - 1, k);
        }

        if (cRight == true)
        {
            Calculate(right, k);
        }

        if (left & 1)
        {
            Apply(left++, value, k);
            cLeft = true;
        }

        if (right & 1)
        {
            Apply(--right, value, k);
            cRight = true;
        }
    }

    for (--left; right > 0; left >>= 1, right >>= 1, k <<= 1)
    {
        if (cLeft == true)
        {
            Calculate(left, k);
        }

        if (cRight == true && (cLeft == false || left != right))
        {
            Calculate(right, k);
        }
    }
}

long long LPSegmentTree::QueryInterval(int left, int right)
{
    PushInterval(left, left + 1);
    PushInterval(right - 1, right);

    long long result = 0;
    for (left += this->treeSize, right += this->treeSize; left < right; left >>= 1, right >>= 1)
    {
        if (left & 1)
        {
            result += this->tree[left++];
        }
        if (right & 1)
        {
            result += this->tree[--right];
        }
    }

    return result;
}

void LPSegmentTree::Print()
{
    for (int i = 1; i < this->tree.size(); i++)
    {
        std::cout << this->tree[i] << " ";
    }
    std::cout << "\n";

    for (int i = 1; i < this->d.size(); i++)
    {
        std::cout << this->d[i] << " ";
    }
    std::cout << "\n";
}
```


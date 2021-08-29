---
title: "PROGRAMMERS 프로그래머스 : 가사 검색"
excerpt: "트라이(Trie)"

categories:
- Algorithm

tags:
- PROGRAMMERS
---

> 시작하면서

　이번 문제는 **트라이(Trie)** 문제이다.

[PROGRAMMERS : 가사 검색](https://programmers.co.kr/learn/courses/30/lessons/60060)    

​        

> 문제 아이디어

​	이 문제는 반복된 문자열을 검색해서 찾아야하는 문제이다. 인풋인 **queries**의 길이가 **최대 100,000**, **words**의 길이도 **최대 100,000**, 단어들의 길이도 **최대 10,000**이므로 효율적인 검색이 필요하다.

​	다른 설명 필요 없이 **Trie**를 사용하면 된다. 여기서 주의할 점은 **검색할 때 단어의 길이**도 신경을 써주어야 한다.

​    

>해결

```c++
#include <string>
#include <vector>
#include <algorithm>
#include <string.h>
#include <unordered_map>

using namespace std;

#define ALPHABET_COUNT (26)

class Trie
{
private:
    Trie* m_node[ALPHABET_COUNT];
    unordered_map<int, int> count;
public:
    Trie()
    {
        memset(this->m_node, NULL, sizeof(this->m_node));
        count.clear();
    }

    ~Trie()
    {
        Clear();
    }

    void Clear()
    {
        for (int i = 0; i < ALPHABET_COUNT; i++)
        {
            if (this->m_node[i])
            {
                delete this->m_node[i];
            }
        }

        memset(this->m_node, NULL, sizeof(this->m_node));
        count.clear();
    }

    void Insert(const char* key, int keyLength);
    Trie* Find(const char* key, int keyLength);
    int FindCount(const char* key, int keyLength);
};


void Trie::Insert(const char* key, int keyLength)
{
    if (*key == '\0')
    {
        return;
    }
    else
    {
        int index = *key - 'a';

        if (this->m_node[index] == NULL)
        {
            this->m_node[index] = new Trie();
        }

        if (this->m_node[index]->count.find(keyLength) == this->m_node[index]->count.end())
        {
            this->m_node[index]->count[keyLength] = 1;
        }
        else
        {
            this->m_node[index]->count[keyLength]++;
        }       
        
        this->m_node[index]->Insert(key + 1, keyLength);
    }
}

Trie* Trie::Find(const char* key, int keyLength)
{
    if (*key == '\0')
        return this;

    int index = *key - 'a';

    if (this->m_node[index] == NULL)
        return NULL;
    else
        return this->m_node[index]->Find(key + 1, keyLength);
}

int Trie::FindCount(const char* key, int keyLength)
{
    Trie *keyTrie = this->Find(key, keyLength);

    if (keyTrie == NULL)
        return 0;

    if (keyTrie->count.find(keyLength) == keyTrie->count.end())
    {
        return 0;
    }
    else
    {
        return keyTrie->count[keyLength];
    }
}

vector<int> solution(vector<string> words, vector<string> queries)
{
    Trie* forwardTrie = new Trie(), *backwardTrie = new Trie();

    for (string word : words)
    {
        forwardTrie->Insert(word.c_str(), word.length());

        string reverseWord = word;
        reverse(reverseWord.begin(), reverseWord.end());
        backwardTrie->Insert(reverseWord.c_str(), word.length());
    }

    vector<int> answer;
    unordered_map<string, int> allQuestionCase;
    for (string query : queries)
    {
        int count = 0;

        if (query[0] == '?')
        {
            int lastQuestionMarkIndex = query.rfind('?');
            string queryWithoutQuestionMark = query.substr(lastQuestionMarkIndex + 1, query.length() - lastQuestionMarkIndex);
            if (queryWithoutQuestionMark == "")
            {
                if (allQuestionCase.find(query) == allQuestionCase.end())
                {

                    for (string word : words)
                    {
                        if (word.length() == query.length())
                        {
                            count++;
                        }
                    }

                    allQuestionCase[query] = count;
                }
                else
                {
                    count = allQuestionCase[query];
                }
            }
            else
            {
                reverse(queryWithoutQuestionMark.begin(), queryWithoutQuestionMark.end());
                count = backwardTrie->FindCount(queryWithoutQuestionMark.c_str(), query.length());
            }
        }
        else
        {
            int firstQuestionMarkIndex = query.find('?');
            string queryWithoutQuestionMark = query.substr(0, firstQuestionMarkIndex);

            count = forwardTrie->FindCount(queryWithoutQuestionMark.c_str(), query.length());
        }

        answer.push_back(count);
    }
    
    return answer;
}
```

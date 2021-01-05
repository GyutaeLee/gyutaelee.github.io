---
title: "Longest Palindromic Subsequence"
excerpt: "가장 긴 팰린드롭 부분 문자열"

categories:
- Algorithm

tags:
- Introduction To Algorithms

---

   [LINK](https://www.techiedelight.com/longest-palindromic-subsequence-using-dynamic-programming/)

​	LPS(Longest Palindromic Subsequence) 문제는 문자열의 가장 긴 하위 회문 시퀀스를 찾는 문제이다. 이 문제는 가장 긴 공통 문자열(LCS) 문제와 다르다.  하위 문자열과 달리 하위 시퀀스는 시퀀스 내에서 연속적인 위치를 가지고 있을 필요가 없다.

​	이 문제를 해결하는 아이디어는 재귀를 사용하는 것이다. 문자열 X[i..j]의 마지막 문자를 첫 번째 문자와 비교하는 것이다. 두 가지 가능성이 있다.

1. 문자열의 마지막 문자가 첫 번째 문자와 같으면 회문에 첫 번째와 마지막 문자를 포함하고, 나머지 하위 문자열 X[i+1, j-1]에 대해 반복한다.
2. 문자열의 마지막 문자가 첫 번째 문자와 다른 경우, 우리가 얻은 두 값 중 최대 값을 반환한다.
   - 마지막 문자를 제거하고 나머지 부분 문자열 X[i, j-1]에 대해 반복
   - 첫 번째 문자를 제거하고 나머지 부분 문자열 X[i+1, j]에 대해 반복

​	이렇게 하면 시퀀스 X의 가장 긴 반복 하위 시퀀스의 길이를 찾기 위해 아래의 재귀 관계가 생성된다.

```
            | 1                                      (if i = j)
LPS[i..j] = | LPS[i+1..j-1] + 2                      (if X[i] = X[j])
            | max (LPS[i+1..j], LPS[i..j-1])         (if X[i] != X[j])
```

​	아래 솔루션은 위의 관계를 사용해 반복적으로 시퀀스 X의 LPS 길이를 찾는다.

```c++
// Function to find the length of Longest Palindromic Subsequence
// of substring X[i..j]
int longestPalindrome(string X, int i, int j)
{
    // base condition
    if (i > j)
        return 0;
 
    // if string X has only one character, it is palindrome
    if (i == j)
        return 1;
 
    // if last character of the string is same as the first character
    if (X[i] == X[j])
    {
        // include first and last characters in palindrome
        // and recur for the remaining substring X[i+1, j-1]
        return longestPalindrome(X, i + 1, j - 1) + 2;
    }
 
    // if last character of string is different to the first character
 
    // 1. Remove last character and recur for the remaining
    //    substring X[i, j-1]
    // 2. Remove first character and recur for the remaining
    //    substring X[i+1, j]
 
    // return maximum of the two values
    return max (longestPalindrome(X, i, j - 1),
                longestPalindrome(X, i + 1, j));
}
```

​	위 솔루션의 최악 시간 복잡도는 O(2^n)이고, 보조 공간은 O(1)이다. 최악의 경우는 X에 반복되는 문자가 없고 각 재귀 호출이 두 번의 재귀 호출로 끝날 때 발생한다.

​	LPS 문제는 최적화에 관한 문제도 있다. LPS 길이가 1인 길이 6의 시퀀스에 대한 재귀 트리를 고려해보겠다.

![Longest Palindromic Subsequence](..\..\..\assets\images\Algorithm\IntroductiionToAlgorithm\LPS_01)

​	보시다시피 동일한 하위 문제가 반복해서 계산되고 있다. 우리는 최적화를 위해 중복되는 하위 문제를 반복적으로 게산하는 대신, 하위 문제 솔루션을 메모하는 동적 프로그래밍으로 해결 할 수 있다는 것을 알고 있다.

```c++
// Function to find the length of Longest Palindromic Subsequence
// of substring X[i..j]
int longestPalindrome(string X, int i, int j, auto &lookup)
{
    // base condition
    if (i > j)
        return 0;
 
    // if string X has only one character, it is palindrome
    if (i == j)
        return 1;
 
    // construct an unique map key from dynamic elements of the input
    string key = to_string(i) + "|" + to_string(j);
 
    // if sub-problem is seen for the first time, solve it and
    // store its result in a map
    if (lookup.find(key) == lookup.end())
    {
        /* if last character of the string is same as the first character
        include first and last characters in palindrome and recur
        for the remaining substring X[i+1, j-1] */
        if (X[i] == X[j])
            lookup[key] = longestPalindrome(X, i + 1, j - 1, lookup) + 2;
        else
 
        /* if last character of string is different to the first character
 
        1. Remove last char & recur for the remaining substring X[i, j-1]
        2. Remove first char & recur for the remaining substring X[i+1, j]
        3. Return maximum of the two values */
 
        lookup[key] = max (longestPalindrome(X, i, j - 1, lookup),
                    longestPalindrome(X, i + 1, j, lookup));
    }
 
    // return the subproblem solution from the map
    return lookup[key];
}
```

​	위 솔루션의 시간 복잡도는 O(n^2)이고, 보조 공간은 O(n^2)이다. 위의 솔루션은 LPS의 길이는 찾지만 LPS를 인쇄하지는 않는다.

​    

**How can we print Longest Palindromic Subsequence**

​	LPS 문제는 LCS 문제의 전형적인 변형이다. 아이디어는 주어진 문자열의 LCS를 역방향으로 찾는 것이다. 문자열 X와 이를 뒤집은 Y의 LCS를 찾으면 답이 된다.

```c++
// lookup[i][j] stores the length of LCS of substring X[0..i-1], Y[0..j-1]
int lookup[N][N];
 
// Function to find LCS of string X[0..m-1] and Y[0..n-1]
string longestPalindrome(string X, string Y, int m, int n)
{
    // return empty string if we have reached the end of
    // either sequence
    if (m == 0 || n == 0)
        return string("");
 
    // if last character of X and Y matches
    if (X[m - 1] == Y[n - 1])
    {
        // append current character (X[m-1] or Y[n-1]) to LCS of
        // substring X[0..m-2] and Y[0..n-2]
        return longestPalindrome(X, Y, m - 1, n - 1) + X[m - 1];
    }
 
    // else when the last character of X and Y are different
 
    // if top cell of current cell has more value than the left
    // cell, then drop current character of string X and find LCS
    // of substring X[0..m-2], Y[0..n-1]
 
    if (lookup[m - 1][n] > lookup[m][n - 1])
        return longestPalindrome(X, Y, m - 1, n);
 
    // if left cell of current cell has more value than the top
    // cell, then drop current character of string Y and find LCS
    // of substring X[0..m-1], Y[0..n-2]
 
    return longestPalindrome(X, Y, m, n - 1);
}
 
// Function to find length of LCS of substring X[0..n-1] and Y[0..n-1]
int LCSLength(string X, string Y, int n)
{
    // first row and first column of the lookup table
    // are already 0 as lookup[][] is globally declared
 
    // fill the lookup table in bottom-up manner
    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= n; j++)
        {
            // if current character of X and Y matches
            if (X[i - 1] == Y[j - 1])
                lookup[i][j] = lookup[i - 1][j - 1] + 1;
 
            // else if current character of X and Y don't match
            else
                lookup[i][j] = max(lookup[i - 1][j], lookup[i][j - 1]);
        }
    }
    return lookup[n][n];
}
```

​	위 솔루션의 시간 복잡도는 O(n^2)이고, 보조 공간은 O(n^2)이다.
---
title: "Elements of dynamic programming"
excerpt: "Dynamic Programming"

categories:
- Algorithm

tags:
- Introduction To Algorithms
---

Elements of dynamic programming
---

>  Dynamic Programming이 되기 위한 2가지 특징

1. Optimal substructure
   - 주어진 문제에 대한 최적해를 구하고자 할 때, 좀  더 작은 sub-problem들에 대한 optimal solution들을 찾은 후 그것들을 결합하면 전체 문제의 optimal solution이 된다.

2. Overlapping subproblems
   - sub-sub problem 간에 중복되는 경우가 여러번 발생하는 경우. 이러한 경우에는 이것을 매번 계산하지 않고, 먼저 계산한 값을 배열이나 해시 테이블에 저장해두었다가 다음번 수행시 즉시 반환할 수 있다.

​	이 2가지 특징이 확인되면 다이나믹 프로그래밍으로 해결할 수 있는 문제라고 볼 수 있다.

다이나믹 프로그래밍은 optimal substructre를 bottom-up 방식에서 사용한다. sub-problem에 대한 optimal solution을 찾고, 그 sub-problems들을 해결해서 전체 문제에 대한 optimal solution을 찾는다.

​    

> Memoization

- 개념

  컴퓨터 프로그램이 동일한 계산을 반복해야 할 때, 이전에 계산한 값을 메모리에 저장함으로써 동일한 계산의 반복 수행을 제거해 프로그램 실행 속도를 빠르게 하는 기술이다. 일반적으로 비효율적인 recursive algorithm을 memoize 하는 식이다.

- 주의점
  1. 메모이즈된 함수는 결과들을 저장하기 때문에 메모리를 추가적으로 소비한다.
  2. 실행 속도가 빠르거나, 사용횟수가 적은 경우에는 실용적이지 않다.
  3. 항상 같은 값을 기대하는 pure function들에만 사용이 가능하다.
---
title: "Quaternion"
excerpt: "Quaternion"

categories:
- Computer Graphics

Tags:
- etc
---

[original document](..\..\..\assets\etc\quaternion.pdf) 

> Introduction

​	Quaternion의 개발은 1843년 W.R.Hamilton에 의해 이루어졌다. 이야기에 따르면 Hamilton은 아내 Helen과 함께 Royal Irish Academy에서 걷다가 3차원을 곱하기 위해 4차원을 추가하는 아이디어에 충격을 받았다. 이 돌파구에 흥분한 부부는 왕립 운하의 브룸 다리를 지나갈 때 새로 발견된 Quaternion 방정식을 다리의 돌에 새겼다.

![Quaternion_01](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_01.PNG)

이 사건은 액자에 새겨져있다. Hamilton은 남은 여생을 Quaternion을 연구하는데 보냈으며, 이는 최초의 non-commutative algebra(비가환 대수학)이 되었다.

​	지금까지 우리는 원점을 통한 축에 대한 R^3의 회전이 행렬식 1을 갖는 3x3 직교 행렬로 표현될 수 있다는 것을 배웠다. 그러나 행렬 표현은 9개 요소 중 4개만 독립적이기 때문에 중복되어 보인다. 또한, 이러한 행렬의 기하학적 해석은 회전 축과 각도를 추출하기 위해 여러 단계의 계산을 수행할 때까지 명확하지 않다. 더욱이, 두 개의 회전을 구성하려면 27개의 곱셈과 18개의 덧셈이 필요한 두 개의 해당 행렬의 곱을 계산해야 한다.

​	Quaternion은 R^3의 회전이 관련된 상황을 분석하는데 매우 효율적이다. Quaternion은 회전 행렬보다 더 간결한 표현인 4-tuple이다. 회전축과 각도를 간단하게 복구할 수 있어 기하학적 의미도 더욱 분명해졌다. 소개할 Quaternion 대수학도 회전을 쉽게 구성할 수 있게 해준다. 이것은 quaternion 합성이 단지 16개의 곱셈과 12개의 덧셈을 취하기 때문이다.

​    

> Quaternion Algebra

​	Quaternion set는 덧셈 및 곱셈의 두 가지 연산과 함께 비가환성 링을 고리를 형성한다. R^3에 대한 표준 직교 기준은 3개의 단위 벡터

- i = (1, 0, 0)

- j = (0, 1, 0)
- k = (0, 0, 1)

로 지정된다. Quaternion **q** 는 스칼라 **q0**와 벡터 q = (q1, q2, q3)의 합으로 정의된다.

![Quaternion_02](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_02.PNG)

#### - Addition and Multiplication

​	두 개의 Quaternion을 추가하면 구성 요소별로 작동한다. 보다 구체적으로 위의 Quaternion **q**와 다른 Quaternion을 고려해라

![Quaternion_03](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_03.PNG)

그리고

![Quaternion_04](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_04.PNG)

모든 Quaternion **q**는 성분 -qi, i = 0, 1, 2, 3을 갖는 음의 -q를 갖는다. 두 쿼터니언의 곱은 Hamilton이 도입한 다음과 같은 기본 규칙을 충족한다.

![Quaternion_05](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_05.PNG)

이제 두 개의 쿼터니언 p와  q의 곱을 알 수 있다:

![Quaternion_06](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_06.PNG)

무슨 일이 일어나고 있는지 기억하거나 이해하는 것조차 너무 길다. 다행히도 R^3에서 두 벡터의 내적과 외적을 활용해 위의 쿼터니언 곱을 보다 간결한 형식으로 작성할 수 있다.

![Quaternion_07](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_07.PNG)

위에서 p = (p1, p2, p3), q = (q1, q2, q3)는 각각 p와 q의 벡터 부분이다.

**Example 1. 두 벡터가 다음과 같이 주어졌다고 가정하자:**

![Quaternion_08](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_08.PNG)

벡터 부분 p = (1, -2, 1), q = (-1, 2, 3)의 내/외적을 계산한다.

![Quaternion_09](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_09.PNG)

(1)에 의해 쿼터니언 곱은 다음과 같다.

![Quaternion_10](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_10.PNG)

우리는 두 쿼터니언의 곱이 스칼라 부분 p0q0 - pq와 벡터 부분 p0q + q0p + p X q를 갖는 쿼터니언임을 알 수 있다. 쿼터니언 세트는 곱셈과 덧셈에서 닫혀있다.

쿼터니언의 곱셈이 덧셈보다 분배적임을 확인하는 것은 어렵지 않다. 항등 쿼터니언에는 실수 부분 1과 벡터 부분 0이 있다.

#### - Complex Conjugate, Norm, and Inverse

​	q = q0 + q = q0 + q1i + q2j + q3k를 쿼터니언이라고 하자. q^*로 표시된 q의 켤레 복소수는 다음과 같이 정의된다.

![Quaternion_11](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_11.PNG)

이전 정의로 부터 우리는 이미 알고 있다

![Quaternion_12](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_12.PNG)

두 개의 쿼터니언 p와 q가 주어지면 다음을 쉽게 확인할 수 있다

![Quaternion_13](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_13.PNG)

|q|로 표시되는 쿼터니언 q의 노말은, 스칼라 |q| = √(q∗q)이다. 쿼터니언은 노말이 1인 경우 단위 쿼터니언이라고 한다. 두 쿼터니언 p와 q의 노말은 개별 노말의 곱이다. 그래서 우리는 다음을 알 수 있다.

![Quaternion_14](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_14.PNG)

쿼터니언 q의 역함수는 다음과 같이 정의된다.

![Quaternion_15](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_15.PNG)

q^-1 * q = q * q^-1 = 1. q가 단위 쿼터니언인 경우 역함수는 켤레 q^*이다.
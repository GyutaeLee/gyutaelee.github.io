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

​	지금까지 우리는 원점을 통한 축에 대한 $\mathbb{R}^3$의 회전이 행렬식 1을 갖는 3x3 직교 행렬로 표현될 수 있다는 것을 배웠다. 그러나 행렬 표현은 9개 요소 중 4개만 독립적이기 때문에 중복되어 보인다. 또한, 이러한 행렬의 기하학적 해석은 회전 축과 각도를 추출하기 위해 여러 단계의 계산을 수행할 때까지 명확하지 않다. 더욱이, 두 개의 회전을 구성하려면 27개의 곱셈과 18개의 덧셈이 필요한 두 개의 해당 행렬의 곱을 계산해야 한다.

​	Quaternion은 $\mathbb{R}^3$​의 회전이 관련된 상황을 분석하는데 매우 효율적이다. Quaternion은 회전 행렬보다 더 간결한 표현인 4-tuple이다. 회전축과 각도를 간단하게 복구할 수 있어 기하학적 의미도 더욱 분명해졌다. 소개할 Quaternion 대수학도 회전을 쉽게 구성할 수 있게 해준다. 이것은 Quaternion 합성이 단지 16개의 곱셈과 12개의 덧셈을 취하기 때문이다.

​    

> Quaternion Algebra

​	Quaternion set는 덧셈 및 곱셈의 두 가지 연산과 함께 비가환성 링을 고리를 형성한다. $\mathbb{R}^3$에 대한 표준 직교 기준은 3개의 단위 벡터

- i = (1, 0, 0)

- j = (0, 1, 0)
- k = (0, 0, 1)

로 지정된다. Quaternion $q$ 는 스칼라 $q_0$와 벡터 $q = (q_1, q_2, q_3)$의 합으로 정의된다.

![Quaternion_02](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_02.PNG)

​    

#### - Addition and Multiplication

​	두 개의 Quaternion을 추가하면 구성 요소별로 작동한다. 보다 구체적으로 위의 Quaternion $q$와 다른 Quaternion을 고려해라

![Quaternion_03](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_03.PNG)

그리고

![Quaternion_04](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_04.PNG)

모든 Quaternion $q$는 성분 $-q_i$, $i = 0, 1, 2, 3$​을 갖는 음의 $-q$를 갖는다. 두 쿼터니언의 곱은 Hamilton이 도입한 다음과 같은 기본 규칙을 충족한다.

![Quaternion_05](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_05.PNG)

이제 두 개의 쿼터니언 $p$와 $q$의 곱을 알 수 있다:

![Quaternion_06](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_06.PNG)

무슨 일이 일어나고 있는지 기억하거나 이해하는 것조차 너무 길다. 다행히도 $\mathbb{R}^3$에서 두 벡터의 내적과 외적을 활용해 위의 쿼터니언 곱을 보다 간결한 형식으로 작성할 수 있다.

![Quaternion_07](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_07.PNG)

위에서 $p = (p1, p2, p3), q = (q1, q2, q3)$​는 각각 $p$와 $q$의 벡터 부분이다.

**Example 1. 두 벡터가 다음과 같이 주어졌다고 가정하자:**

![Quaternion_08](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_08.PNG)

벡터 부분 $p = (1, -2, 1), q = (-1, 2, 3)$의 내/외적을 계산한다.

![Quaternion_09](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_09.PNG)

(1)에 의해 쿼터니언 곱은 다음과 같다.

![Quaternion_10](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_10.PNG)

우리는 두 쿼터니언의 곱이 스칼라 부분 $p_0q_0 - pq$와 벡터 부분 $p_0q + q_0p + p \times q$를 갖는 쿼터니언임을 알 수 있다. 쿼터니언 세트는 곱셈과 덧셈에서 닫혀있다.

쿼터니언의 곱셈이 덧셈보다 분배적임을 확인하는 것은 어렵지 않다. 항등 쿼터니언에는 실수 부분 1과 벡터 부분 0이 있다.

​    

#### - Complex Conjugate, Norm, and Inverse

​	$q = q_0 + q = q_0 + q_1i + q_2j + q_3k$를 쿼터니언이라고 하자. $q^*$로 표시된 $q$의 켤레 복소수는 다음과 같이 정의된다.

![Quaternion_11](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_11.PNG)

이전 정의로 부터 우리는 이미 알고 있다

![Quaternion_12](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_12.PNG)

두 개의 쿼터니언 $p$와 $q$가 주어지면 다음을 쉽게 확인할 수 있다

![Quaternion_13](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_13.PNG)

$|q|$​로 표시되는 쿼터니언 $q$의 노말은, 스칼라 $\|q\| = \sqrt{q∗q}$이다. 쿼터니언은 노말이 1인 경우 단위 쿼터니언이라고 한다. 두 쿼터니언 $p$와 $q$의 노말은 개별 노말의 곱이다. 그래서 우리는 다음을 알 수 있다.

![Quaternion_14](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_14.PNG)

쿼터니언 q의 역함수는 다음과 같이 정의된다.

![Quaternion_15](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_15.PNG)

$q^-1q = qq^-1 = 1$. $q$가 단위 쿼터니언인 경우 역함수는 켤레 $q^*$이다.

​    

> Quaternion Rotation Operator

​	$\mathbb{R}^4$​에 있는 쿼터니언이 $\mathbb{R}^3$​에 있는 벡터에서 어떻게 작동할 수 있을까? 먼저, 벡터 **v ∈ *$\mathbb{R}^3$​*은 실수부가 0인 순수 쿼터니언이다. 단위 쿼터니언 **$q = q_0 + q$만 고려하자. ${q_0}^2 + ||q||^2 = 1$은 다음과 같은 angle $$\theta$$​ 가 있어야 함을 의미한다.

![Quaternion_16](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_16.PNG)



실제로 **$$\cos \theta$$​​​ = $$q_0$$​​​ , $$\sin \theta$$​​​​ = ||q||**인 고유한 **$$\theta \in [0, \pi]$$​**​​​ 구간이 존재한다. 단위 쿼터니언은 이제 **angle $$\theta$$​​​​**와 단위 벡터 $u = q/||q||$​로 작성할 수 있다:

![Quaternion_17](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_17.PNG)

단위 쿼터니언 $q$를 사용해 벡터 v $$\in$$ $\mathbb{R}^3$​​​ 에 대한 연산자를 정의한다.

![Quaternion_18](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_18.PNG)

여기서 우리는 두 가지를 관찰한다. 첫째, 쿼터니언 연산자 (3)은 벡터 $v$의 길이를 변경하지 않는다.

![Quaternion_19](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_19.PNG)

둘째, $v$​​의 방향은 $q$​를 따르는 경우 연산자 $$L_q$$​​​ 에 의해 변경되지 않은 상태로 유지된다. 이를 확인하기 위해 $v = kq$​로 두고 다음을 수행한다.

![Quaternion_20](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_20.PNG)

두 관찰은 연산자 $$L_q$$​가 $q$에 대한 회전과 같은 역할을 한다고 추측하게 한다. 이것은 다음 정리에 의해 정확해진다.

정리를 진행하기 전에 연산자 $$L_q$$​가 $\mathbb{R}^3$​에 대해 선형이라는 점에 주목한다. 임의의 두 벡터 $$v_1, v_2$$​ $$\in$$​  $\mathbb{R}^3$​ 및 임의의 $$a_1, a_2 \in \mathbb{R}^3$$​​​​ 에 대해 다음 식을 볼 수 있다.

![Quaternion_21](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_21.PNG)

![Quaternion_22](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_22.PNG)

그리고 임의 벡터 $v \in \mathbb{R}^3$ 에 대한 연산자 액션은​

![Quaternion_23](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_23.PNG)

$v$에 대한 것은 회전 축으로서 $u$에 대한 각도를 통한 벡터의 회전과 동일하다.

**Proof**	주어진 벡터 $v \in \mathbb{R}^3$, 우리는 그것을 $v = a + n$으로 분해한다. 여기서 $a$는 벡터 $q$를 따른 성분이고, $n$은 $q$에 수직인 성분이다. 그런 다음 연산자 $L_q$ 에서 $a$는 불변인 반면, $n$은 각도 $q$에 대해 회전한다는 것을 보여준다. 연산자는 선형이므로 이미지 $qvq^*$ 가 실제로 각도를 통해 $q$에 대한 $v$의 회전으로 해석된다는 것을 보여준다.

​	우리는 초기 추론에서 $L_q$에서 $a$가 불변임을 안다. 따라서 직교 성분 $n$에 대한 $L_q$의 영향에 초점을 맞추겠다. 우리는 다음 식을 가지고 있다.

![Quaternion_24](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_24.PNG)

위의 마지막 단계에서 $u = q/||q||$ 를 소개했다. $n\perp = u \times n$을 나타낸다. 그래서 마지막 방정식은 다음과 같다.

![Quaternion_25](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_25.PNG)

$n\perp$ 와 $n$​은 같은 길이를 가지고 있다:

![Quaternion_26](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_26.PNG)

마지막으로 (5)를 다음 형식으로 다시 작성한다.

![Quaternion_27](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_27.PNG)

즉, 결과 벡터는 $n$과 $n\perp$로 정의된 평면의 각도를 통한 $n$의 회전이다. 아래 그림을 참조하자. 이 벡터는 회전 축과 분명히 수직이다.

![Quaternion_28](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_28.PNG)

​	단위 쿼터니언 형식 (4)를 (3)에 대입하고, 다음을 통해 축 $u$를 $\theta$까지를 중심으로 벡터 $v$를 회전해 결과 벡터를 얻는다:

![Quaternion_29](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_29.PNG)

EXAMPLE 2. $\frac{2\pi}{3}$의 각도를 통해 (1, 1, 1)로 정의된 축에 대한 회전을 고려해라. 이 축에 대해 basis vector $i, j, k$는 $2\pi$를 통해 회전할 때 동일한 원뿔을 생성한다. 우리는 단위 벡터를 정의한다.

![Quaternion_30](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_30.PNG)

회전 각 $\theta = \frac{2\pi}{3}$. 쿼터니언 $q$는 다음과 같이 회전을 정의한다.

![Quaternion_31](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_31.PNG)

basis vector $i = (1,0,0)$에 대한 회전 효과를 계산해보자. (6)을 사용해 결과 벡터를 얻는다:

![Quaternion_32](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_32.PNG)

연산자 $L_q$에서 $v$의 회전은 벡터 $v$에 연결된 관찰자의 관점에서도 해석될 수 있다. 그가 보고 있는 것은 좌표 프레임이 쿼터니언에 의해 정의된 동일한 축에 대한 각도를 통해 회전한다는 것이다.

![Quaternion_33](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_33.PNG)

그리고 임의 벡터 $v \in \mathbb{R}^3$에 대한 연산자의 동작

![Quaternion_34](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_34.PNG)

$v$는 회전하지 않는 동안 각도를 통해 축 $u$에 대한 좌표 프레임의 회전이다.

동일하게, 연산자 $L_{q^*}$는 $q$에 대한 각 $-\theta$를 통해 좌표 프레임에 대해 벡터 $v$를 회전한다.​

​	쿼터니언 연산자 $L_q(v) = qvq^*$ 는 (고정된) 좌표 프레임에 대한 점 또는 벡터 회전으로 해석될 수 있다. 쿼터니언 연산자 $L_{q^*}(v) = q^*vq$ 는 (고정된) 점의 공간에 대한 좌표 프레임 회전으로 해석될 수 있다.

​    

> Quaternion Operator Sequences

​	$p$와 $q$를 두 개의 단위 쿼터니언이라고 하자. 먼저 연산자 $L_q$를 벡터 $u$에 적용하고, 벡터 $v$를 얻는다. $v$에 연산자 $L_q$를 적용하고 벡터 $w$를 얻는다. 동일하게 우리는 두 연산자의 합성 $L_q ◦ L_p$를 적용한다:

![Quaternion_35](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_35.PNG)

​	$p$와 $q$는 단위 커터니언이므로 곱도 $pq$​이다. 따라서 위의 방정식은 쿼터니언을 정의하는 두 쿼터니언 $p$와 $q$의 곱인 회전 연산자를 설명한다. 복합 회전의 축과 각도는 곱 $qp$로 제공된다.

​	유사하게, 각각 쿼터니언 $p$​​와 $q$​​에 의해 결정된 좌표계의 회전을 수행하는 쿼터니언 연산자 $L_{q^*}(u) = p^*vq$​​ 및 $L_{q^*}(v) = q^*vq$​​를 고려하자. 쿼터니언 곱 $pq$​​는 연산자 $L_{(pq)^*}$​​를 정의한다. 이는 연산자 $L_{q^*}$​​ 다음에 $L_{p^*}$​​가 오는 시퀀스를 나타낸다. $L_{(pq)^*}$​​의 축과 회전 각도는 쿼터니언 곱 $pq$​​로 표시되는 것이다.

EXAMPLE 3.  이제 쿼터니언 방법을 사용해 "Space Rotations"라는 제목의 메모에서 위성 추적 예제의 합성 회전 축과 각도를 찾는다. 추적 응용 프로그램은 베어링 각도를 통해 z축을 중심으로 회전한 다음 고도 각도를 통해 새 y축을 중심으로 회전한다는 점을 기억해라. 이 두 회전 후에 새로운 x축은 위성을 가리킨다. 두 회전은 각각 아래의 두 쿼터니언으로 설명된다.

![Quaternion_36](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_36.PNG)

좌표계를 회전시키므로 두 연산자 $L_{p^*}$와 $L_{q^*}$​가 순차적으로 적용된다. 복합 회전 연산자는 station frame의 좌표를 tracking frame 좌표로 변환하는 $L_{(pq)^*}$이다. 그리고 구성 회전을 설명하는 쿼터니언은 다음과 같은 곱 $pq$이다.

![Quaternion_37](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_37.PNG)

복합 회전의 축은 벡터로 정의된다.

![Quaternion_38](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_38.PNG)

그리고 회전 각 $\theta$는 다음을 만족한다.

![Quaternion_39](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_39.PNG)

코사인은 "Rotation in Space"라는 제목의 Section 3에서 얻은 것과 같다.

![Quaternion_40](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_40.PNG)

해당 섹션의 회전 축과 각도는 tracking frame의 좌표를 station frame의 좌표로 변환한다. 이것은 (7)의 축 $v$가 해당 섹션에서 얻은 것과 반대인 반면 각도는 동일한 이유를 설명한다.

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

로 지정된다. Quaternion $q$​ 는 스칼라 $q_0$와 벡터 $q = (q_1, q_2, q_3)$​​의 합으로 정의된다.

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

$|q|$​​​​로 표시되는 쿼터니언 $q$​​​의 노말은, 스칼라 $|q| = \sqrt{q∗q}$​​​이다. 쿼터니언은 노말이 1인 경우 단위 쿼터니언이라고 한다. 두 쿼터니언 $p$​​​와 $q$​​​의 노말은 개별 노말의 곱이다. 그래서 우리는 다음을 알 수 있다.

![Quaternion_14](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_14.PNG)

쿼터니언 q의 역함수는 다음과 같이 정의된다.

![Quaternion_15](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_15.PNG)

$q^-1q = qq^-1 = 1$. $q$가 단위 쿼터니언인 경우 역함수는 켤레 $q^*$이다.

​    

> Quaternion Rotation Operator

​	$\mathbb{R}^4$​​​​​에 있는 쿼터니언이 $\mathbb{R}^3$​​​​​에 있는 벡터에서 어떻게 작동할 수 있을까? 먼저, 벡터 v ∈ $\mathbb{R}^3$​​​​​은 실수부가 0인 순수 쿼터니언이다. 단위 쿼터니언 $q = q_0 + q$​만 고려하자. ${q_0}^2 + \|q\|^2 = 1$​은 다음과 같은 angle $\theta$​​​ 가 있어야 함을 의미한다.

![Quaternion_16](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_16.PNG)



실제로 $\cos \theta$​​​​​​​​​ = $q_0$​​​​​​​​​ , $\sin \theta  \|q\|$​​​인 고유한 $\theta \in [0, \pi]$​​​ 구간이 존재한다. 단위 쿼터니언은 이제 angle $\theta$​​​와 단위 벡터 $u = q/\|q\|$​​​​​​​​로 작성할 수 있다:

![Quaternion_17](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_17.PNG)

단위 쿼터니언 $q$를 사용해 벡터 v $\in$ $\mathbb{R}^3$​​​ 에 대한 연산자를 정의한다.

![Quaternion_18](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_18.PNG)

여기서 우리는 두 가지를 관찰한다. 첫째, 쿼터니언 연산자 (3)은 벡터 $v$의 길이를 변경하지 않는다.

![Quaternion_19](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_19.PNG)

둘째, $v$​​의 방향은 $q$​를 따르는 경우 연산자 $L_q$​​​ 에 의해 변경되지 않은 상태로 유지된다. 이를 확인하기 위해 $v = kq$​로 두고 다음을 수행한다.

![Quaternion_20](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_20.PNG)

두 관찰은 연산자 $L_q$​가 $q$에 대한 회전과 같은 역할을 한다고 추측하게 한다. 이것은 다음 정리에 의해 정확해진다.

정리를 진행하기 전에 연산자 $L_q$​가 $\mathbb{R}^3$​에 대해 선형이라는 점에 주목한다. 임의의 두 벡터 $v_1, v_2$​ $\in$​  $\mathbb{R}^3$​ 및 임의의 $a_1, a_2 \in \mathbb{R}^3$​​​​ 에 대해 다음 식을 볼 수 있다.

![Quaternion_21](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_21.PNG)

![Quaternion_22](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_22.PNG)

그리고 임의 벡터 $v \in \mathbb{R}^3$ 에 대한 연산자 액션은​

![Quaternion_23](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_23.PNG)

$v$에 대한 것은 회전 축으로서 $u$에 대한 각도를 통한 벡터의 회전과 동일하다.

**Proof**	주어진 벡터 $v \in \mathbb{R}^3$, 우리는 그것을 $v = a + n$으로 분해한다. 여기서 $a$는 벡터 $q$를 따른 성분이고, $n$은 $q$에 수직인 성분이다. 그런 다음 연산자 $L_q$ 에서 $a$는 불변인 반면, $n$은 각도 $q$에 대해 회전한다는 것을 보여준다. 연산자는 선형이므로 이미지 $qvq^*$ 가 실제로 각도를 통해 $q$에 대한 $v$의 회전으로 해석된다는 것을 보여준다.

​	우리는 초기 추론에서 $L_q$에서 $a$가 불변임을 안다. 따라서 직교 성분 $n$에 대한 $L_q$의 영향에 초점을 맞추겠다. 우리는 다음 식을 가지고 있다.

![Quaternion_24](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_24.PNG)

위의 마지막 단계에서 $u = q/\|q\|$​ 를 소개했다. $n\perp = u \times n$​을 나타낸다. 그래서 마지막 방정식은 다음과 같다.

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

​	쿼터니언 연산자 $L_q(v) = qvq^{*}$​​​ 는 (고정된) 좌표 프레임에 대한 점 또는 벡터 회전으로 해석될 수 있다. 쿼터니언 연산자 $L_{q^{*}}(v) = q^{*}vq$​​​ 는 (고정된) 점의 공간에 대한 좌표 프레임 회전으로 해석될 수 있다.

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

​    

> Application: 3-D Shape Registration

​	모델 기반 인식의 중요한 문제는 shape model에 대해 이러한 점을 가장 잘 일치시키는 데이터 점 세트의 변환을 찾는 것이다. 이 프로세스를 종종 data registration이라고 한다. data point는 일반적으로 range sensors, touch sensors 등에 의해 실제 물체에서 측정되며, cartesian 좌표(데카르트 좌표)로 제공된다. quality of match는 종종 data point에서 model까지의 총 제곱 거리로 설명된다. 여러 shape model이 가능한 경우 총 거리가 가장 작은 모델이 물체의 모양으로 인식된다.

​	쿼터니언은 위의 최소 제곱 기반 registration 문제를 해결하는 데 매우 효과적이다. 3D로 문제를 공식화하는 것부터 시작하자. $\{p_1, p_2, ... , p_3 \}$는 data point의 집합이다. 우리는 $p_1,...,p_n$​가 shape model의 점 $q_1,...,q_n$​과 일치한다고 가정한다. 즉, data point와 model의 data point 간의 대응이 미리 결정되었다. 그런 다음 문제는 $det(R) = 1$​인 직교 행렬 $R$로 표시되는 회전과 다음 최소화에 대한 솔루션으로 translation $b$​를 찾는 것이다.

![Quaternion_41](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_41.PNG)

우리는 두 점 세트의 중심을 계산하는 것으로 시작한다:

![Quaternion_42](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_42.PNG)

모든 점의 중심에 대한 상대 좌표는 $1<i\leq n$에 대해 다음과 같이 얻는다.

![Quaternion_43](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_43.PNG)

다음과 같은 식을 얻는다.

![Quaternion_44](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_44.PNG)

(8)의 objective function을 $\overline{p}, \overline{q}, p_i', q_i'$를 사용해 다시 작성해보자:

![Quaternion_45](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_45.PNG)

translation $b$를 최소화하면 마지막 방정식의 두 번째 향이 0보다 커야 하므로 다음을 산출한다:

![Quaternion_46](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_46.PNG)

따라서 우리는 data registration 문제를 두 단계로 분해했다. 첫 번째 단계는 방정식 (11)에 의해 주어진 최적의 translation을 결정하고, 두 번째 단계는 set $\{p_i\}$의 최적 회전을 결정한다. 모든 포인트 $p_i$는 $q_i$와 일치하기 전에 $R(p_i - \overline p) + \overline q$로 변환된다. 동등하게, 두 점 집합 $\{p_i\}$와 $\{q_i\}$의 가장 좋은 일치를 찾기 위해 먼저 $\{p_i\}$를 변환해 중심이 $\{q_i\}$의 중심과 일치하도록 한 다음 공통 중심을 중심으로 회전한다.

지금까지의 추론에 따르면 최적 회전은 다음 공식으로 풀 수 있다.

![Quaternion_47](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_47.PNG)

여기서 우리는 쿼터니언을 사용해 [3]에 설명된 대로 (12)에 대한 정확한 솔루션을 제시한다. equivalent 쿼터니언 기반 솔루션은 [2]에 나와있다. 점별 대응을 가정하는 두 개의 곡선(또는 표면)을 일치시키는 버전은 쿼터니언을 사용하지 않고, 다소 유사한 방식으로 [8]에서 정확히 해결된다.

먼저 (12)의 합을 다음과 같이 다시 작성한다:

![Quaternion_48](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_48.PNG)

위의 마지막 방정식의 첫 번째 명사는 회전에 의존하지 않으므로 두 번째 명사만 최소화하면 된다. 동일하게 이것은 최대화를 통해 수행할 수 있다.

![Quaternion_49](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_49.PNG)

회전 행렬 $R$에는 9개의 항목이 있으며 그 중 4개만 $R$의 직교성과 단위 결정자로 인해 독립적이다. 대신 단위 쿼터니언을 사용해 회전을 나타낸다. 본질적으로, 우리는 최대화하는 단위 쿼터니언 $q$를 찾는다.

![Quaternion_50](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_50.PNG)

여기에서 우리는 쿼터니언을 $\mathbb{R}^4$​에 있는 벡터로 본다. $q = (q_0,q_1,q_2,q_3)^T$​,  $q^* = (q_0, -q_1,-q_2,-q_3)^T$​라고 가정하자. 또한, 점 $p_1',...,p_n'$​ 그리고 $q_1',...,q_n'$​은 표기법 남용을 더해 $p_i' = (0, p_{i1}', p_{i2}', p_{i3}')^T$​ 그리고 $q_i' = (0, q_{i1}',q_{i2}',q_{i3}')^T$​ 로 나타난다.

쿼터니언 곱의 정의를 적용하면 다음을 나타내는 것이 어렵지 않다.

![Quaternion_51](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_51.PNG)

다음으로 (14)의 명사를 행렬 곱으로 다시 작성하려고 한다. 이를 위해 행렬을 정의한다

![Quaternion_52](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_52.PNG)

$1\leq i\leq n$​에 대해. 그러면 쿼터니언 곱 $qp_i'$ 및 $q_i'q$는 행렬 곱 $P_iq$ 및 $Q_iq$와 동일하다. 따라서 우리는

![Quaternion_53](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_53.PNG)

![Quaternion_54](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_54.PNG)

각 행렬 $P_i^TQ_i$가 대칭인지 확인하는 것은 쉽다. 4x4 행렬도 마찬가지이다.

![Quaternion_55](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_55.PNG)

따라서 M은 실수 고유값만을 갖는다.  $\lambda_1 \geq \lambda_2 \geq \lambda_3 \geq \lambda_4$​  의 조건을 가진  $\lambda_1,\lambda_2,\lambda_3,\lambda_4$​가 있다. $v_1,v_2,v_3,v_4$

를 해당 직교 단위 고유 벡터라고 하자. 서로 다른 고유값에 해당하는 고유 벡터는 서로 직교해야 한다. 동일한 고유값에 해당하는 다중 고유 벡터는 서로 직교하도록 선택된다. 쿼터니언 $q$는 다음 고유 벡터의 선형 조합이다.

![Quaternion_56](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_56.PNG)

다음 식을 얻을 수 있다

![Quaternion_57](../../..\assets\images\Computer Science\Computer Graphics\Quaternion_57.PNG)

곱 $q^TMq$는 $\alpha_1 = 1, \alpha_2 = \alpha_3 = \alpha_4 = 0$​일 때 최대 값을 달성한다. 따라서 (14)를 최대화하는 단위 쿼터니언 q는 행렬 M의 최대 고유값에 해당하는 고유 벡터이다. (for data registration)

​	대응하는 점 $q_1,...,q_2$가 알려지지 않은 경우, Iterative Closet Point(CIP) [1]라고 하는 잘 알려진 방법이 registration 문제를 해결한다. 주어진 data point set $\{p_1,...,p_n\}$, ICP 알고리즘은 초기 대응 점 $q_1^{(0)},...,q_n^{(0)}$은 surface 모델에서 $p_1^{(0)} = p_q,...,p_n^{(0)}$에 각각 가장 가까운 점이다. 그런 다음 도입된 쿼터니언 기반 방법을 적용해 $\{p_i^{(0)}\}$와 $\{q_i^{(0)}\}$가 가장 잘 일치하는 회전 및 변환을 결정한다. 두 번째 반복은 방금 발견된 변환을 모든 $p_i^{(0)}$에 적용해  $p_i^{(1)}$을 얻은 다음 모델에서 해당하는 새로운 점 $q_i^{(1)}$를 $p_i^{(1)}$​에 가장 가까운 점으로 결정한다. 쿼터니언 등을 사용해 최상의 회전 및 변환을 다시 계산한다. 새 translation의 변경이 충분히 작아지면 알고리즘이 중지된다.

​    

> Other Application of Quaternions

​	물리학에서 쿼터니언은 양자 역학 수준에서 우주의 본질과 상관 관계가 있다. 그것들은 현대 상대성 이론의 기초를 형성하는 로렌츠 변환의 우아한 표현으로 이어진다. 신호 처리에서 QFT(Quaternion Fourier Transform)는 강력한 도구이다. QFT는 더 이상 나눗셈 대수가 아닌 비용으로 손실된 교환 속성을 복원한다. 예를 들어 컬러 이미지에 워터마크를 삽입하는 데 사용할 수 있다. QFT의 다른 응용 프로그램에는 얼굴 인식(Quaternion Wavelet Transform과 함께) 및 음성 인식이 포함된다 [6].

​    

> Quaternions vs Homogeneous Coordinates

​	배율 조정 및 회전과 함께 변환을 곱하기 위해 homogeneous 좌표가 도입되었다. point, line, plane 등을 표현하는데 편리하며, 투영법 연구의 기초가 된다. 쿼터니언과 마찬가지로 homogeneous 좌표는 4-tuple이다. 이것은 일종의 쿼터니언 연산자를 사용해 scailing 및 translation을 수행하는 방법이 있을 수 있음을 시사한다. 현재로서는 쿼터니언과 그 회전 연산자가 대수적으로 homogeneous 좌표와 호환되지 않는 방식이 발견되지 않았다.

​    

​    

**References**

- [1] P. J. Besl and N. D. McKay. A method for registration of 3-D shapes. IEEE Transactions on
  pattern analysis and machine intelligence, 14(2):239–256, 1992.
- [2] O. D. Faugeras and M. Hebert. The representation, recognition, and locating of 3-D objects.
  International Journal of Robotics Research, 5(3):27–52, 1986.
- [3] B. K. P. Horn. Closed-form solution of absolute orientation using unit quaternions. Journal of
  Optical Society of America A, 4(4):629–642, 1987.
- [4] T. W. Hungerford. Algebra. Springer-Verlag, 1974.
- [5] N. Jacobson. Basic Algebra. W. H. Freeman & Co.,1985.
- [6] S. Oldenburger. Applications of Quaternions. Written project of the course “Problem Solving
  Techniques in Applied Computer Science” (Com S 477/577), Department of Computer Science,
  Iowa State University, 2005.
- [7] J. B. Kuipers. Quaternions and Rotation Sequences. Princeton University Press, 1999.
- [8] J. T. Schwartz and M. Sharir. Identification of partially obscured objects in two and three
  dimensions by matching noisy characteristic curves. International Journal of Robotics Research,
  6(2):29–44, 1987.

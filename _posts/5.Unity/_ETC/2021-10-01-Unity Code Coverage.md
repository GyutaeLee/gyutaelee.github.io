---
title: "Unity Test Runner의 결과를 시각화된 자료로 보자"
excerpt: "Unity : Code Coverage"

categories:
- Game Development

tags:
- Unity
---

> 시작하면서

​	이 글을 읽기 전에 [Unity Test Runner](https://gyutaelee.github.io/game%20development/Unity-Test-Runner/) 글을 읽고 오자. **Code Coverage**는 Unity Test Runner로 테스트한 결과를 좀 더 시각적으로 보여주는 패키지이다.



> 패키지 다운로드

​	먼저 패키지 다운로드가 필요하다. **Window -> Package Manager -> Code Coverage**를 Import한다.

![image-20210929155538330](../../../assets\images\Unity\Unity_TestRunner_01.png)

​	설치 후에는 **Window -> Analysis -> Code Coverage**로 접근해서 Code Coverage를 실행한다.

![image-20211001125444449](../../..\assets\images\Unity\Unity_CodeCoverage_01.png)

​	경고가 몇개씩 뜨는데 읽고 enable 후 Unity를 재시작해서 해결해주면 된다.

![image-20211001130537254](../../..\assets\images\Unity\Unity_CodeCoverage_02.png)

​	Unity를 재시작한 후에는 **Start Recording** 버튼이 활성화 되어있다. 해당 버튼을 누르고 **Test Runner**에서 테스트를 진행하고, 끝난 후 **Stop Recroding**버튼을 누른다.

![image-20211001130726917](../../..\assets\images\Unity\Unity_CodeCoverage_03.png)

​	그럼 폴더가 하나 생긴다. 여러 파일이 있는데 그 중에서 **index**를 실행한다.

![image-20211001130837732](../../..\assets\images\Unity\Unity_CodeCoverage_04.png)

​	해당 파일을 실행하면 테스트 결과를 볼 수 있다. 

​    

> 마무리

​	아직 제대로 된 기능을 몰라서 글이 빈약하다. 추후에 기능들을 좀 더 알게 되면 내용을 보충할 예정이다.

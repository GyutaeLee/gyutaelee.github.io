---
title: "Unity Test Runner를 사용해서 Unit Test를 해보자"
excerpt: "Unity : Test Runner"

categories:
- Game Development

tags:
- Unity
---

> 시작하면서

​	테스트는 프로그래머에게 고통이다. 코드를 수정할 때마다 꼭 해야하지만, 이전에 작성한 코드들까지 연계되어 있는 작업일 경우 전부 다 테스트해봐야한다(이 작업을 더욱 꼼꼼하게 해주시는 QA분들 감사합니다). 이 작업에 대한 부담을 덜어주고, 보다 효율적이게 하려면 **테스트 자동화(Test Automation)**가 필요하다. Unity에서는 **Test Runner**를 통해 테스트 자동화를 만들 수 있다. 오늘은 이에 대해 알아보자



> 패키지 다운로드

​	먼저 패키지 다운로드가 필요하다. **Window -> Package Manager -> Test Framework**를 Import한다. (나중에 사용할 **Code Coverage**도 미리 설치해도 좋다)

![image-20210929155538330](C:\Users\134461\Desktop\_GIT-PROJECT\gyutaelee.github.io\assets\images\Unity\Unity_TestRunner_01.png)

​	설치 후에는 **Window -> General -> Test Runner**로 접근해서 Test Runner를 실행한다.

![image-20210929155855923](C:\Users\134461\Desktop\_GIT-PROJECT\gyutaelee.github.io\assets\images\Unity\Unity_TestRunner_02.png)

​	실행하면 위와 같은 화면이 뜬다. 상단을 보면 **PlayMode**와 **EditMode**가 있다. PlayMode에서는 게임 관련 테스트를 진행하고, EditMode에서는 커스텀 에디터 테스트가 가능하다. [관련 링크](https://docs.unity3d.com/Packages/com.unity.test-framework@1.1/manual/edit-mode-vs-play-mode-tests.html)

​	이 글에서는 PlayMode만 설명하겠다. PlayMode를 들어가서 **Create PlayMode Test Assembly Folder** 버튼을 클릭해준다.

![image-20210929161359746](C:\Users\134461\Desktop\_GIT-PROJECT\gyutaelee.github.io\assets\images\Unity\Unity_TestRunner_03.png)

​	그럼 다음과 같이 Assembly Folder가 자동으로 생성된다. Assembly에 대해 잘 모른다면 [해당 링크](https://docs.unity3d.com/kr/2019.4/Manual/ScriptCompilationAssemblyDefinitionFiles.html)를 한 번 읽고 오는 것을 추천한다.

![image-20210929161618524](C:\Users\134461\Desktop\_GIT-PROJECT\gyutaelee.github.io\assets\images\Unity\Unity_TestRunner_04.png)

![image-20210929161754467](C:\Users\134461\Desktop\_GIT-PROJECT\gyutaelee.github.io\assets\images\Unity\Unity_TestRunner_05.png)

​	위와 같은 파일이 생기는데 지금은 우선 넘어가겠다. (궁금하면 가장 하단에 있는 링크 참조)

![image-20210929161843609](C:\Users\134461\Desktop\_GIT-PROJECT\gyutaelee.github.io\assets\images\Unity\Unity_TestRunner_06.png)

​	다시 Test Runner로 돌아오면 **Create Test Script in current folder**라는 버튼이 생겨있다. 이 버튼을 클릭하면 스크립트가 하나 생성된다.

![image-20210929161922107](C:\Users\134461\Desktop\_GIT-PROJECT\gyutaelee.github.io\assets\images\Unity\Unity_TestRunner_07.png)

​	해당 스크립트를 열어보면 다음과 같은 코드가 있다.

```c#
using System.Collections;
using System.Collections.Generic;
using NUnit.Framework;
using UnityEngine;
using UnityEngine.TestTools;

public class NewTestScript
{
    // A Test behaves as an ordinary method
    [Test]
    public void NewTestScriptSimplePasses()
    {
        // Use the Assert class to test conditions
    }

    // A UnityTest behaves like a coroutine in Play Mode. In Edit Mode you can use
    // `yield return null;` to skip a frame.
    [UnityTest]
    public IEnumerator NewTestScriptWithEnumeratorPasses()
    {
        // Use the Assert class to test conditions.
        // Use yield to skip a frame.
        yield return null;
    }
}

```

​	하나씩 살펴보면

1. [Test] : 단순 기능 테스트를 진행할 수 있다. **Assert** 클래스를 사용해 조건 비교로 테스트를 진행한다.
2. [UnityTest] : 여러 테스트를 절차적으로 진행하면서 테스트한다. 유니티 코루틴처럼 동작한다. **Assert**클래스를 사용해 조건 비교로 테스트를 진행한다.

​	여기서 다시 Test Runner로 돌아오면 작성된 테스트 함수들이 보인다.

![image-20210929162143097](C:\Users\134461\Desktop\_GIT-PROJECT\gyutaelee.github.io\assets\images\Unity\Unity_TestRunner_08.png)

​	테스트 방법은

1. 좌측 위의 **Run All** 버튼을 클릭해서 모든 테스트를 진행할 수 있다.
2. 각 키워드에 있는 **동그라미** 버튼을 누르면 트리 구조로 해당 노드를 포함한 하단 테스트들이 모두 진행된다.
3. 다중 클릭 후 **Run Selected**를 클릭해서 선택한 테스트들만 진행할 수 있다.
4. **Run All Tests** 버튼을 누르면 연결되어있는 기기(혹은 개발 기기 그 자체)에서 빌드 후 테스트를 진행한다.

​	**1~3**은 에디터에서 테스트가 진행되고, **4**는 빌드 후 해당 플랫폼에서 테스트가 진행된다.

![image-20210929162713297](C:\Users\134461\Desktop\_GIT-PROJECT\gyutaelee.github.io\assets\images\Unity\Unity_TestRunner_09.png)

​	테스트 진행 후에는 별 다른 문제가 없으면 위와 같이 체크 표시가 뜨고, 문제가 있으면 빨갛게 경고 표시가 뜬다.

​    

> 마무리

​	간단하게 Unity Test Framework 사용법에 대해 정리했다. 각자의 게임에 적합한 테스트를 진행하기 위해서는 여러 방법이 있을 것이다. 이 글은 단지 **기초적인 사용법**까지만 정리했으니, 더 심오하고 다양한 기능이 필요하면 하단의 **Unity Docs**와 **Manual**을 참조하길 추천한다.

​    

**[관련 링크]**

[Unit Testing Docs](https://docs.unity3d.com/Manual/testing-editortestsrunner.html)

[Test Framework Manual](https://docs.unity3d.com/Packages/com.unity.test-framework@1.1/manual/index.html)

[Code Sample Docs](https://docs.unity3d.com/kr/2018.4/Manual/PlaymodeTestFramework.html)

[Unity Test Runner Docs](https://docs.unity3d.com/kr/2018.4/Manual/testing-editortestsrunner.html)


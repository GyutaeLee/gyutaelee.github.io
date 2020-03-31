---
title: "ECS Tutorial - RTS [1]"
excerpt: "Make an RTS with Unity's Entity Component System"

categories:
- Game Development

tags:
- Unity
- ECS Tutorial

last_modified_at : 2020-03-03
---

> 시작하면서

 원래는 문서를 모두 읽고 코드  작성을 해볼 생각이었지만, 문서를 읽다보니 머리에 잘 들어오지 않았다. 그래서 예제가 될만한 튜토리얼들을 구글링해보았다. 하지만 안타깝게도 최근 들어서 많은 업데이트가 있었고, 비교적 짧다고 생각되는 1년 지난 튜토리얼들도 코드와 유니티 버전, 패키지 버전들이 상이해서 대부분이 제대로 사용하기가 힘들었다. 그 중에서 가장 최근이고, 시작하기에 알맞은 튜토리얼을 하나 찾았다.

[[ECS Tutorial] Making an RTS with Unity's Entity Component System [1]](https://www.youtube.com/watch?v=36Q6HO19O6U&list=PL13LVknaRwqyN4vKyeZwjcVlkjuvYgYwq&index=1)

 소스코드도 친절하게 깃에 업로드 되어 있고, 비교적 최근 버전인 상태여서 오류도 적었다(하지만 첫 영상부터 바뀐 소스가 많은 것은 함정).    

​    

> 이슈

 결론부터 말하자면 박스 3개 띄우는데 거의 3시간 걸렸다... 프로젝트가 깃에 올라와 있어서 비교적 쉽게 진행할 수 있겠다고 생각했다. 하지만 이 생각은 굉장히 잘못됐다. 유니티 기본 명령어들이 많이 바뀌었고, 필수로 들어가야하는 요소들조차도 문서에 제대로 업데이트 되지 않아있었다. 구글링하면서 이것저것 많이 고쳤는데, 가장 큰 이슈는 아래 커밋에 나온 코드이다.    

[commit link](https://github.com/GyutaeLee/CG_SHOOT/commit/866956e42e4e6ef785746ea263b6a89b75f274ab)

<img src="..\..\assets\images\ECS\ECS_RTS_Tutorial_00.PNG" alt="ECS_RTS_Tutorial_00" style="zoom:200%;" />

 2019.3.x 버전으로 올라오면서 "RenderBoudns" 추가가 필요해졌고, 튜토리얼에는 없었는데 버전이 올라오면서 "LocalToWorld"가 생긴 것을 구글링을 통해 겨우 찾았다.    
** 관련 링크 : [1]( https://answers.unity.com/questions/1701725/ecs-rendermesh-not-work.html ) / [2]( https://forum.unity.com/threads/the-entities-is-not-visible.674518/ )    

 이를 수정하고 큐브 3개를 정상적으로 띄웠다.    

![ECS_RTS_Tutorial_01.PNG](..\..\assets\images\ECS\ECS_RTS_Tutorial_01.PNG)

 사진을 잘 보면 알겠지만 이전 유니티로 작업한 것과는 다르게 Hierarchy에 생성한 큐브들이 보이지 않는다. GameObject가 아닌 Entity로 생성되서 그런듯하다.    

  

  

> 마무리

 이번 첫 튜토리얼을 한줄평하자면 **"지옥과 천당"**이다. 이것도 수정해보고 저것도 수정해보고, 구글링도 해서 따라해봐도 계속 안 되어서 너무 힘들었다. 하지만 계속 찾고 시도해본 끝에 큐브를 잘 띄웠고, 마치 게임 공부를 처음 할 때 큐브를 띄웠던 것처럼 기분이 좋았다. (여담으로 해당 프로젝트가 만들어진 2019.2.a 버전으로는 그냥 잘 된다. 단지 추후에 프로젝트를 진행할 때 팀원과 최신 버전으로 계속 업데이트해가면서 프로젝트를 진행하자고 했기 때문에 힘들지만 삽질을 하면서 공부 중이다.) --> 여담이 길다?

 확실히 유니티의 최신 기술이고 계속 업데이트되면서 변경되어가다보니 자료들이 확실치 않고, 매우매우 부족하다. 영어로 된 자료들에 거부감이 조금씩 사라져가려고 했는데, 이번 공부에서는 영어로 된 자료마저도 부족하니 막막한 미래가 보인다... 나같은 삽질을 한 명이라도 덜 하게 하기 위해서라도 작업 후에 블로그 글을 열심히 써야겠다는 생각을 했다.

 튜토리얼 마무리까지 열심히 해보겠다!         

​      

​      

---



​    

​    

**프로젝트 커밋 링크 : [1]( https://github.com/GyutaeLee/CG_SHOOT/commit/d5bcfbd956368c3f0162667aa12b827b6a1d969e ) [2]( https://github.com/GyutaeLee/CG_SHOOT/commit/866956e42e4e6ef785746ea263b6a89b75f274ab )   **     

​        

**추천 링크**

[ReleaseNotes]( https://github.com/Unity-Technologies/EntityComponentSystemSamples/blob/master/ECSSamples/ReleaseNotes.md ) : Untiy ECS 릴리즈노트 (수정 안 되있는 부분도 많다)

[Unity ECS 정리본]( https://github.com/KorStrix/Unity_Study_ECS ) : "KorStrix"라는 분이 ECS 관련 자료를 잘 정리해주신 링크
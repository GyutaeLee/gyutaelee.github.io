---
title: "ECS Tutorial - RTS [3]"
excerpt: "Make an RTS with Unity's Entity Component System"

categories:
- Game Development

tags:
- Unity
- ECS Tutorial

last_modified_at : 2020-04-11
---

> 시작하면서

 Pure ECS - RTS 튜토리얼 3번째 글이다. 이번에는 오류가 계속 생기고, 고치지고 힘들어서 그냥 강의를 끝까지 보고 글을 작성한다. 다행히 마지막 튜토리얼 글에 업데이트 된 내용을 다시 정리해주는 강의가 있다. 하지만 2019.05.04 기준이어서 오류가 하나 있었다. 이를 제외하고는 무난하게 따라갈 수 있는 부분이다.

 [[ECS Tutorial] Making an RTS with Unity's Entity Component System [3]](https://www.youtube.com/watch?v=d1Pl0c_6uqo&list=PL13LVknaRwqyN4vKyeZwjcVlkjuvYgYwq&index=7)    

​    

​    



> 이슈

 여러 이슈들이 남은 영상들을 따라하면서 발생했지만 마지막 강의에서 모두 고쳐주었으니 따로 적지는 않겠다. 영상과 깃허브를 참고해 코드를 모두 작성했는데 다음 오류가 발생했다.

<img src="..\..\..\assets\images\ECS\RTS_Tutorial\ECS_RTS_Tutorial_03.PNG" alt="ECS_RTS_Tutorial_03">

 이슈를 읽자면 [ReadOnly]로 된 컨테이너에 값을 쓰려고 해서 경고가 발생했다.

<img src="..\..\..\assets\images\ECS\RTS_Tutorial\ECS_RTS_Tutorial_04.PNG" alt="ECS_RTS_Tutorial_04">

 경고가 발생한 라인은 다음과 같이 CommandBuffer를 [ReadOnly]로 선언하고 AddComponent를 사용해서이다. 그런데 튜토리얼 Git의 [코드](https://github.com/skhamis/Unity-ECS-RTS/blob/master/Assets/Systems/PlayerUnitSelectSystem.cs)를 보면 너무나도 멀쩡히 [ReadOnly]로 선언하고, AddComponent를 사용하고 있다.

<img src="..\..\..\assets\images\ECS\RTS_Tutorial\ECS_RTS_Tutorial_05.PNG" alt="ECS_RTS_Tutorial_05">

 그래서 한 번 [ReadOnly]를 지우고 시도해보았다. 하지만 다음과 같은 오류가 발생했다.

<img src="..\..\..\assets\images\ECS\RTS_Tutorial\ECS_RTS_Tutorial_06.PNG" alt="ECS_RTS_Tutorial_06">

```
InvalidOperationException: PlayerUnitSelectJob.Data.CommandBuffer is not declared [ReadOnly] in a IJobParallelFor job. The container does not support parallel writing. Please use a more suitable container type.
```

 IJobParallerFor job에서 CommandBuffer가 [ReadOnly]로 선언되지 않았다... 해당 컨테이너가 parallel writing을 지원하지 않아서 적합한 container를 사용하라고 하는데,  다음 코드(UnitSpawnerSystem.cs)에서는 멀쩡히 잘 동작하고 있다.

<img src="..\..\..\assets\images\ECS\RTS_Tutorial\ECS_RTS_Tutorial_07.PNG" alt="ECS_RTS_Tutorial_07">

<img src="..\..\..\assets\images\ECS\RTS_Tutorial\ECS_RTS_Tutorial_08.PNG" alt="ECS_RTS_Tutorial_08">

 해당 코드를 보면 CommandBuffer를 [ReadOnly]로 선언하지 않고 사용해서 정상적으로 작동하고 있다. 혹시 BeginInitializationEntityCommandBufferSystem 의 차이일까 싶어서 변경해서 작업도 해보았지만 달라진 것이 없었다.

 구글링을 해보아도 원하는 답변이 나오지 않았고, 튜토리얼에서 사용하던 버전에서 코드를 타고 들어가보고, 내가 사용하는 버전에서 코드를 타고 들어가보니 AddComponent에 오버로딩 된 함수들이 변경된 것이 보였다. 아마 업데이트 되면서 함수 내부 구조가 바뀌어서 발생된 문제인듯 싶다. (내부 구현을 찾아보고 싶었는데 못 찾음)    

​    

 완벽한 해결법은 아니지만 하나의 해결법을 찾았다. [LINK](https://stackoverflow.com/questions/50853372/how-to-pass-entities-to-a-job-to-add-components) 

<img src="..\..\..\assets\images\ECS\RTS_Tutorial\ECS_RTS_Tutorial_09.PNG" alt="ECS_RTS_Tutorial_09">

 해당과 같이 [NativeDisableParallelForRestriction] 키워드를 사용하면 이슈없이 코드가 잘 작동한다. 무슨 키워드인지 궁금해서 Unity Document에 검색을 해봤는데 아무것도 안 나온다... ([LINK](https://docs.unity3d.com/ScriptReference/Unity.Collections.NativeDisableParallelForRestrictionAttribute.html))

<img src="..\..\..\assets\images\ECS\RTS_Tutorial\ECS_RTS_Tutorial_10.PNG" alt="ECS_RTS_Tutorial_10">

 구글링을 하면서 다른 사람들이 사용한 것을 찾아보니 병렬 작업할 때 [ReadOnly]를 사용하고, 수정할 때 오류가 뜨는 것을 강제로 무시하게 하는 키워드인 것 같다. (추후에 알게 되면 추가하겠습니다)    

​    

​    

> 마무리 

 <img src="..\..\..\assets\images\ECS\RTS_Tutorial\ECS_RTS_Tutorial_11.PNG" alt="ECS_RTS_Tutorial_11">

 결론적으로는 튜토리얼을 완료하고, 마우스 좌클릭으로 선택 및 하이라이트 띄우기 + 우클릭으로 이동 + 충돌 인지 까지 Pure ECS로 구현을 완료했다. 하지만 아직 모든 것을 이해한 것은 아니고, ECS가 어떠한 구조로 돌아가는지 맛보기 정도까지만 했다.

 튜토리얼을 진행하면서 깨달은 점은. . . ECS에 관한 자료가 너무 부족하고, 불안정하다는 것이다. 계속 업데이트가 되다보니 구글링으로 찾은 자료조차도 믿지 못하고, 믿지 못하다보니 찾는 과정 자체가 스트레스라서 열정이 조금씩 식어가는 안 좋은 현상이 발생하게 된다.

 특히 한글로 된 자료들은 더더욱 부족한 실정이다. 앞으로 공부하면서 글을 더 열심히 쓰고 정리해서 다른 사람들에게 도움이 되었으면 좋겠다. 그리고 그러한 글쓰기가 동기 부여가 되서 나 또한 발전해나가는 좋은 효과를 기대한다.    

​    

​      

---

**프로젝트 환경**

- Unity 2019.3.3f1
- Entities : Version preview.8 - 0.8.0
- Mathematics : version 1.1.0

**GitHub : [LINK](https://github.com/GyutaeLee/CG_SHOOT/tree/master/CG_SHOOT/Assets/_GYUTAE/Practice) **

​    
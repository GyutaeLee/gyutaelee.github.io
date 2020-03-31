---
title: "ECS Tutorial - RTS [2]"
excerpt: "Make an RTS with Unity's Entity Component System"

categories:
- Game Development

tags:
- Unity
- ECS Tutorial

last_modified_at : 2020-03-31
---

> 시작하면서

 두 번째 튜토리얼이다. 저번 튜토리얼에서는 많이 해맸었는데 과연... 이번 튜토리얼은 무난하게 흘러갈까? 걱정하면서 시작한다.

 [[ECS Tutorial] Making an RTS with Unity's Entity Component System [2]](https://www.youtube.com/watch?v=Fyk2bF8GYHk&list=PL13LVknaRwqyN4vKyeZwjcVlkjuvYgYwq&index=2)    

​    

> 이슈

 역시나 이번 프로젝트도 바뀐게 여러개 많았다. 하지만 무난하게 릴리즈 노트와 구글링으로 변경된 키워드들을 바꾸어가면서 해결했다. 하지만 ISharedComponentData와 관련된 이슈는 해결하지 못했다. 다음 오류이다.

<img src="..\..\assets\images\ECS\ECS_RTS_Tutorial_02.PNG" alt="ECS_RTS_Tutorial_02">

```
ArgumentException: type UnitSpawner is a ISharedComponentData and has managed references, you must implement IEquatable<T>
```

 해당 이슈에 관해서 구글링했을 때 자료들이 나오기는 하는데 전부 따라해봐도 내가 원하는 결과는 나오지 않았다. 그래서 우선 패스했다.

```
ArgumentException: Unknown Type:`UnitSpawner` All ComponentType must be known at compile time. For generic components, each concrete type must be registered with [RegisterGenericComponentType].
```

 해당 이슈에 관해서 구글링을 해보면 compile 이전에 component type을 선언해주어야 한다해서 다음 코드를 입력하라고 했다.

```c#
[assembly: RegisterGenericComponentType(typeof(UnitSpawner))]
```

 하지만~~~ 전혀 해결되지 않았다. 그래서 해당 코드에 관해서 구글링을 해봤는데 자료가 잘 나오지 않았다. 그래서 우선 패스했다.

 결론적으로 이번 이슈는 삽질을 많이 했지만 명확한 답은 얻지 못했다. 그래서 유니티 샘플 프로젝트와 해당 튜토리얼의 완성본을 찾아보니 ISharedComponentData를 사용하지 않고 있었다. 해당 튜토리얼의 완성본에서까지도 사용 안 하고 주석으로 처리된 것을 보고 두 가지 추측을 할 수 있엇다. **키워드가 바뀌었거나, Shared까지 사용할 프로젝트 크기가 아니거나**.    

​    

​    

> 마무리

 그래서 우선 해당 이슈는 패스하기로 결정했다. 추후에 튜토리얼 완성본에도 없는 내용이기 때문에 아직 하기는 이르다고 생각했기 때문이다(그리고 이미 너무 많이 구글링했다...). 아쉽게도 이번 튜토리얼은 이렇게 넘어가야 한다. 추후 튜토리얼을 진행하면서 해당 내용에 대해 해결점을 찾으면 다시 이 글에 추가하도록 하겠다.    

​    

​    

---

​    

**관련 구글링 링크** : [1](https://forum.unity.com/threads/all-componenttype-must-be-known-at-compile-time.713900/)
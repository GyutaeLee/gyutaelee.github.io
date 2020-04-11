---
title: "ECS_2_CoreECS_Components"
excerpt: "Entity Component System : Components"

categories:
- Game Development

tags:
- Unity
- ECS

last_modified_at: 2020-03-24
---

> 시작하면서

 이번 목차는 ECS에서 C인 **Component**이다. 데이터를 의미하는 것으로 얼추 알고 있다. 첫 단추인만큼 제대로 이해하고 넘어가는 것이 목표이다!

 

  

---

---

[Unity Original Document Link](https://docs.unity3d.com/Packages/com.unity.entities@0.0/manual/ecs_components.html)

Components
====

 Component는 ECS 아키텍처의 세 가지 기본 요소 중 하나이다. 그것들은 게임이나 프로그램의 data를 나타낸다. entity는 기본적으로 시스템에서 동작을 제공하는 component 컬렉션을 인덱싱하는 식별자이다.

 구체적으로 ECS의 Component는 다음 "marker interfaces" 중 하나를 가진 구조체이다:

- IComponentData
- ISharedComponentData
- ISystemStateComponentData
- ISharedSystemStateComponentData

 EntityManager는 entity에 나타나는 고유한 component 조합을 Archetype으로 구성한다. Chunk라는 메모리 블록에 동일한 Archetype을 가진 모든 entity의 component를 함께 저장한다. 주어진 청크의 entity는 모두 동일한 component 아키타입을 갖는다.

 Shared components는 Shared components의 특정 값(Archetype과 함께)을 기반으로 entity를 세분화하는데 사용할 수 있는 특수한 종류의 data component이다. Entity에 Shared Component를 추가하면 EntityManager가 동일한 공유 데이터 값을 가진 모든 entity를 동일한 청크에 배치한다. Shared Component를 사용하면 시스템이 같은 entity를 함께 처리할 수 있다. 예를 들어, Hybrid.rendering 패키지의 일부인 Shared Component Rendering.RenderMesh는 메시, 재질, receiveShadows 등을 포함해 여러 필드를 정의한다. 렌더링 할 때 모든 3D 객체를 처리하는 것이 가장 효율적이다. 해당 필드에 대해 동일한 값을 갖는다. 이러한 속성은 Shared Component에 지정되므로 EntityManager는 일치하는 entity를 메모리에 함께 배치하므로 렌더링 시스템에서 이를 효율적으로 반복할 수 있다.

**Note**: Entity에서 Component를 추가 또는 제거하거나, SharedComponent 값을 변경하면 EntityManager가 entity를 다른 청크로 이동해 필요한 경우 새 청크를 만든다.

 System state component는 entity를 삭제할 때 일반 component 또는 Shared component처럼 작동한다. EntityManager는 System state component를 제거하지 않으며 제거 될 때까지 Entity ID를 재활용하지 않는다. 이러한 동작의 차이로 인해 System이 entity가 파괴 될 때 내부 상태를 정리하거나 자원을 해제 할 수 있다.

  

  

General Purpose Components
====

ComponentData
----

 Unity의 ComponentData(표준 ECS 용어로 Component라고도 함)는 Entity의 instance data만 포함하는 구조체이다. ComponentData는 메소드를 포함할 수 없다. 이것을 구 Unity 시스템의 관점에서 보면 구 Component 클래스와 비슷하지만, 변수만 포함하는 클래스와 비슷하다.

 Unity ECS는 코드에서 구현할 수 있는 IComponentData라는 인터페이스를 제공한다.

  

IComponentData
----

 기존 Unity Component(MonoBehaviour 포함)는 동작에 대한 데이터 및 메서드가 포함된 객체 지향 클래스이다. IComponentData는 순수한 ECS 스타일 구성 요소로, 동작이 아니라 데이터만 정의함을 의미한다. IComponentData는 클래스가 아닌 구조체이므로 기본적으로 참조 대신 **값**으로 복사된다. 일반적으로 데이터를 수정하려면 다음 패턴을 사용해야한다.

```c#
var transform = group.transform[index]; // Read

transform.heading = playerInput.move;   // Modify
transform.position += deltaTime * playerInput.move * settings.playerMoveSpeed;

group.transform[index] = transform; 	// Write
```

  IComponentData 구조체는 관리 객체에 대한 참조를 포함하지 않을 수 있다. 모든 ComponentData는 단순한 non-garbage-collected 추적된 청크 메모리에 존재하기 때문이다.

See file:  : */Packages/com.unity.entities/Unity.Entities/IComponentData.cs*

  

  

Shared Component Data
====

 IComponentData는 World position 저장과 같이 Entity마다 다른 데이터에 적합하다. ISharedComponentData는 많은 엔티티에 공통점이 있는 경우에 유용하다. 예를 들어 Boid 데모에서는 동일한 프리팹에서 많은 entity를 인스턴스화하므로 많은 Boid entity 간의 RenderMesh가 정확히 동일한 경우이다.

```c#
[System.Serializable]
public struct RenderMesh : ISharedComponentData
{
    public Mesh                 mesh;
    public Material             material;

    public ShadowCastingMode    castShadows;
    public bool                 receiveShadows;
}
```

 ISharedComponentData의 가장 큰 장점은 entity별로 메모리 비용이 전혀 없다는 것이다.

 ISharedComponentData를 사용해 동일한 InstanceRenderer 데이터를 사용하는 모든 entity를 그룹화한 다음 렌더링을 위해 모든 행렬을 효율적으로 추출한다. 결과 코드는 데이터가 액세스되는 그대로 정확하게 배치되므로 간단하고 효율적이다.

- RenderMeshSystemV2 (see file:  *Packages/com.unity.entities/Unity.Rendering.Hybrid/RenderMeshSystemV2.cs* )

  

Some important notes about SharedComponentData
---

- 같은 SharedComponentData를 가진 Entity는 동일한 청크로 그룹화된다. SharedComponentData에 대한 인덱스는 Entity가 아닌 청크당 한 번 저장된다. 결과적으로 SharedComponentData는 entity별로 메모리 오버 헤드가 없다.
- EntityQuery를 사용하면 동일한 유형의 모든 entity를 반복할 수 있다.
- 또한, EntityQuery.SetFilter()를 사용해 특정 SharedComponentData 값이 있는 entity를 구체적으로 반복 할 수 있다. 데이터 레이아웃으로 인해 이 반복은 오버 헤드가 적다.
- EntityManager.GetAllUniqueSharedComponents를 사용해 살아있는 entity에 추가된 모든 고유한 SharedComponentData를 검색 할 수 있다.
- SharedComponentData는 자동으로 참조 카운트된다.
- SharedComponentData는 거의 변경되지 않는다. SharedComponentData를 변경하려면 memcpy를 사용해 해당 entity의 모든 ComponentData를 다른 청크로 복사해라.

  

  

System State Components
===

 SystemStateComponentData의 목적은 시스템 내부의 리소스를 추적하고, 개별 콜백에 의존하지 않고 필요에 따라 해당 리소스를 적절하게 생성 및 파괴 할 수 있는 기회를 제공하는 것이다.

 SystemStateComponentData 및 SystemStateSharedComponentData는 한 가지 중요한 점을 제외하고는 각각 정확히 ComponentData 및 SharedComponentData와 같다:

1. Entity가 삭제될 때 SystemStateComponentData는 삭제되지 않는다.

DestroyEntity는 shorthand(속기?)이다.

1. 이 특정 Entity ID를 참조하는 모든 component를 찾는다.
2. 해당 component를 삭제한다.
3. 재사용을 위해 Entity ID를 재활용한다.

 그러나 SystemStateComponentData가 있으면 제거되지 않는다. 이를 통해 시스템은 Entity ID와 연관된 모든 자원 또는 상태를 정리할 수 있다. Entity ID는 모든 SystemStateComponentData가 제거된 후에만 재사용된다.

  

Motivation
----

- System은 ComponentData를 기반으로 내부 상태를 유지해야 할 수도 있다. 예를 들어, 자원이 할당될 수 있다.
- 다른 시스템에서 값과 상태를 변경하면 시스템에서 해당 상태를 관리할 수 있어야 한다. 예를 들어, Component의 값이 변경되거나 관련 Component가 추가 또는 삭제된 경우이다.
- "No callbacks"는 ECS 설계 규칙의 중요한 요소이다.

  

Concept
----

 SystemStateComponentData의 일반적인 사용은 내부 state를 제공해 사용자 component를 미러링하는 것으로 예상된다.

 예를 들어:

- FooComponent (ComponentData, user assigned)
- FooStateComponent (SystemComponentData, system assigned)

  

Detecting Component Add
----

 사용자가 FooComponent를 추가하면 FooStateComponent가 존재하지 않는다. FooSystem 업데이트는 FooStateComponent없이 FooComponent를 쿼리해 추가 된 것으로 추론할 수 있다. 이 시점에서 FooSystem은 FooStateComponent 및 필요한 내부 상태를 추가한다.

  

Detecting Component Remove
----

 사용자가 FooComponent를 제거해도 FooStateComponent는 여전히 존재한다. FooSystem 업데이트는 FooComponent없이 FooStateComponent를 쿼리해 제거된 것으로 추론할 수 있다. 이 시점에서 FooSystem은 FooStateComponent를 제거하고 필요한 내부 상태를 수정한다.

  

Detecting Destroy Entity
----

 DestroyEntity는 실제로 다음을 위한 간단한 유틸리티이다.

- 주어진 엔티티 ID를 참조하는 components를 찾는다.
- 발견된 components를 삭제한다.
- Entity ID를 재활용한다.

 그러나 SystemStateComponentData는 DestroyEntity에서 제거되지 않으며 마지막 구성 요소가 삭제 될 때까지 Entity ID가 재활용되지 않는다. 이를 통해 시스템은 component 제거와 동일한 방식으로 내부 상태를 정리할 수 있다.

  

SystemStateComponent
----

 SystemStateComponentData는 ComponentData와 유사하며 유사하게 사용된다.

```c#
struct FooStateComponent : ISystemStateComponentData
{
}
```

 SystemStateComponentData의 가시성은 component와 동일한 방식으로 (개인, 공용, 내부를 사용해) 제어되지만 일반적으로 SystemStateComponentData는 이를 생성하는 시스템 외부에서 ReadOnly가 될 것으로 예상된다.

  

SystemStateSharedComponent
----

 SystemStateSharedComponentData는 SharedComponentData와 유사하며 유사하게 사용된다.

```c#
struct FooStateSharedComponent : ISystemStateSharedComponentData
{
  public int Value;
}
```

  

  

Dynamic Buffer Components
====

Dynamic Buffers
----

 DynamicBuffer는 가변 크기의 "stretchy" buffer를 Entity와 연관시킬 수 있는 Component data type이다. 특정 수의 요소 내부 용량을 전달하는 component type으로 작동하지만 내부 용량이 소진되면 heap memory block을 할당 할 수 있다.

 이 방법을 사용하면 메모리 관리가 완전히 자동으로 이루어진다. DynamicBuffers와 연관된 메모리는 EntityManager에 의해 관리되므로 DynamicBuffer component가 제거 될 때 연관된 힙 메모리도 자동으로 해제된다.

 DynamicBuffers는 제거된 고정 배열 지원을 대체한다.

  

Declaring Buffer Element Types
----

 버퍼를 선언하려면 버퍼에 넣을 요소 유형으로 버퍼를 선언해라:

````c#
// 버퍼의 각 인스턴스에 대해 청크 데이터로 예약해야하는 버퍼 요소 수를 나타냅니다.
// 이 경우 버퍼 헤더의 크기와 함께 8개의 정수 (32 바이트)가 예약됩니다
// (현재 64 비트 대상에서 16 바이트).
[InternalBufferCapacity(8)]
public struct MyBufferElement : IBufferElementData
{
    // 이러한 암시적 변환은 선택 사항이지만 타이핑 줄이는 데 도움이 될 수 있습니다.
    public static implicit operator int(MyBufferElement e) { return e.Value; }
    public static implicit operator MyBufferElement(int e) { return new MyBufferElement { Value = e }; }

    // 각 버퍼 요소가 저장할 실제 값.
    public int Value;
}
````

 버퍼 자체가 아닌 요소 유형을 설명하는 것이 이상해 보이지만 이 디자인은 ECS에서 두 가지 주요 이점을 제공한다:

1. float3 유형의 DynamicBuffer 또는 다른 공통 값 유형을 둘 이상 가질 수 있다. 요소가 최상위 레벨 구조로 고유하게 랩핑되어있는 한 동일한 값 유형을 활용하는 여러 버퍼를 추가 할 수 있다.
2. EntityArchetypes에 Buffer 요소 유형을 포함시킬 수 있으며 일반적으로 component가 있는 것처럼 동작한다.

  

Adding Buffer Types To Entities
----

 Entity에 Buffer를 추가하기 위해 Entity에 Component type을 추가하는 일반적인 방법을 사용할 수 있다:

**Using AddBuffer()**

```c#
entityManager.AddBuffer<MyBufferElement>(entity);
```

**Using an archetype**

```c#
Entity e = entityManager.CreateEntity(typeof(MyBufferElement));
```

  

Accessing Buffers
----

 DynamicBuffer에 액세스하는 방법에는 여러 가지가 있는데, 이 방법은 일반 component 데이터에 대한 액세스 방법을 병렬로 수행한다.

**Direct, main-thread only access**

```c#
DynamicBuffer<MyBufferElement> buffer = entityManager.GetBuffer<MyBufferElement>(entity);
```

  

Entity based access
----

 JobComponentSystem에서 Entity 별로 버퍼를 찾을 수도 있다:

```c#
	var lookup = GetBufferFromEntity<EcsIntElement>();
    var buffer = lookup[myEntity];
    buffer.Append(17);
    buffer.RemoveAt(0);
```

  

Reinterpreting Buffers (experimental)
----

 버퍼는 같은 크기의 유형으로 해석 될 수 있다. 제어된 type-punning을 허용하고, 방해가 될 때 wrapper element type을 제거하는 것이 목적이다. Reinterpret하려면 간단히 Reinterpret<T>를 호출해라:

```c#
var intBuffer = entityManager.GetBuffer<EcsIntElement>().Reinterpret<int>();
```

 Reinterpret된 버퍼는 원래 버퍼의 safety handle을 가지고 있으며 사용하기 안전하다. 이들은 동일한 기본 BufferHeader를 재사용하므로, 재해석된 하나의 버퍼에 대한 수정 사항이 다른 버퍼에 즉시 반영된다.

 유형 검사가 포함되어 있지 않으므로 uint 및 float 버퍼의 별칭을 지정할 수 있다.

  

  

> 마무리 

 와.. 무슨 말인지 잘 모르겠다. 생소하다... 어렵다... 역시 새로운 지식을 공부할 때는 읽기만 하는 것이 아니라 직접 해봐야 이해가 되나보다. 글을 3번 정도 읽었으니 코드를 조금씩 써보면서 기술을 익혀야겠다.
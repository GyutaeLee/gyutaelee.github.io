---
title: "ECS_1_Overview"
excerpt: "Entity Component System : Overview"

categories:
- Game Development

tags:
- Unity
- ECS

last_modified_at: 2020-03-23
---

> 시작하면서

 Unity3D에서 최근에 생긴 시스템인 ECS(Entity Component System)을 공부해서 사용해보려고 한다. 우선 처음에는 역시 유니티 공식 문서와 튜토리얼을 보면서 하는게 좋을 것 같아서 유니티 공식 문서부터 번역하며 공부하려고 한다!

 문서와 튜토리얼을 마친 후에는 간단한 연습 프로젝트를 하나 진행하고, 후에는 해당 시스템과 URP(Universal Render Pipeline)을 곁들인 프로젝트를 진행하는 것이 최종 목표이다. (URP 부분 스터디는 함께 프로젝트를 진행할 동료가 공부를 하고, 서로 알려주기로 했음)

 어떤 프로젝트를 진행할지는 아직 정하지 않았지만, 어디에 내놓아도 멋있을만한 포트폴리오를 만드는 것이 목표이다...! 화이팅!     

  

  

---

---

[Unity Original Document Link](https://docs.unity3d.com/Packages/com.unity.entities@0.0/manual/index.html)

Entity Component System
----

 ECS (Entity Component System)은 Unity 데이터 지향 기술 스택의 핵심이다. 이름에서 알 수 있듯이 ECS에는 세 가지 주요 부분이 있다.

- **Entity** : 게임이나 프로그램을 채우는 Entity 또는 Things
- **Components** : Entity와 관련이 있지만 Entity가 아닌 Data 자체로 구성되는 data이다.  (이 조직의 차이점은 객체 지향 디자인과 데이터 지향 디자인의 주요 차이점 중 하나이다.)
- **Systems** : 구성 요소 데이터를 현재 상태에서 다음 상태로 변환하는 로직이다. 예를 들어, 시스템은 모든 이동 엔티티의 위치를 이전 프레임 이후의 시간 간격으로 곱해 업데이트 할 수 있다.

  

  

---

[Unity Original Document](https://docs.unity3d.com/Packages/com.unity.entities@0.0/manual/ecs_entities.html)

Entities
----

Entity는 엔티티 구성 요소 시스템 아키텍처의 세 가지 기본 요소 중 하나이다. 그것들은 당신의 게임이나 프로그램에서 개별적인 "things"를 나타낸다. 엔티티에는 behavior이나 data가 없다. 대신, 어떤 데이터가 함께 속하는지 식별한다. System은 동작을 제공하고, Component는 데이터를 저장한다. 

 Entity는 본질적으로 ID이다. 기본적으로 이름조차도 없는 lightweight GameObject로 생각할 수 있다. Entity ID는 안정적이다. 다른 Component나 Entity에 대한 참조를 저장하는 유일한 안정적인 방법이다.

 EntityManager는 월드의 모든 엔티티를 관리한다. EntityManager는 엔티티 목록을 유지하고 최적의 성능을 위해 엔티티와 연관된 데이터를 구성한다.

 Entity에는 유형이 없지만 엔티티 그룹은 관련 데이터 구성 요소의 유형으로 분류 할 수 있다. 엔티티를 만들고 구성 요소를 추가 할 때 EntityManager는 기존 엔티티에서 구성 요소의 고유한 조합을 추적한다. 이러한 고유한 조합을 Archetype이라고 한다. 엔티티에 구성 요소를 추가하면 EntityManager가 EntityArchetype 구조체를 만든다. 기존 EntityArchetypes를 사용해 해당 아키 타입에 맞는 새 엔티티를 만들 수 있다. EntityArchetype을 미리 만들고 이를 사용해 엔티티를 만들 수도 있다.

  

Creating Entities
----

  Entity를 생성하는 가장 쉬운 방법은 Unity 에디터이다. 씬에 배치된 게임 오브젝트와 프리 팹을 런타임에 엔티티로 변환하도록 설정할 수 있다. 게임이나 프로그램의 보다 역동적인 부분을 위해 작업에서 여러 엔티티를 만드는 스폰 시스템을 만들 수 있다. 마지막으로 EntityManager.CreateEntity 함수 중 하나를 사용해 한 번에 하나씩 엔티티를 작성할 수 있다.

  

Creating Entities with an EntityManager
----

 EntityManager.CreateEntity 함수 중 하나를 사용해 entity를 작성해라. Entity는 EntityManager와 동일한 World에서 작성된다.

다음과 같은 방법으로 엔티티를 하나씩 만들 수 있다:

- ComponentType 객체의 배열을 사용해 component가 있는 entity를 만든다.
- EntityArchetype을 사용해 component가 있는 entity를 작성한다.
- Instantiate를 사용해 현재 데이터를 포함한 기존 entity 복사 component가 없는 entity를 작성한 다음 component를 추가해라. (component를 즉시 추가하거나 추가 component가 필요할 수 있다)

 한 번에 여러 entity를 만들 수도 있다:

- CreateEntity를 사용해 동일한 Archetype을 가진 새 entity로 NativeArray를 채운다.
- Instantiate를 사용해 현재 데이터를 포함해 기존 entity의 복사본으로 NativeArray를 채운다.
- CreateChunk를 사용해 지정된 Archetype으로 지정된 수의 엔티티로 채워진 청크를 명시적으로 만든다.

  

Adding and Removing Components
----

 Entity를 만든 후에는 component를 추가하거나 제거할 수 있다. 이렇게 하면 영향을 받는 entity의 Archetype이 변경되고, EntityManager는 변경된 데이터를 새로운 메모리 청크로 이동하고 원래 청크의 component 배열을 압축해야한다.

 구조적 변경(즉, SharedComponentData의 값을 변경하는 component 추가 또는 제거, entity 삭제)을 유발하는 entity에 대한 변경은 작업이 수행 중인 데이터를 무효화 할 수 있으므로 작업 내부에서 수행 될 수 없다. 대신, 이러한 유형의 EntityCommandBuffer를 변경하는 명령을 추가하고 작업이 완료된 후 이 명령 버퍼를 실행해라.

 EntityManager는 NativeArray의 모든 entity 뿐만 아니라 단일 entity에서 component를 제거하는 기능을 제공한다. 자세한 내용은 [Components]( https://docs.unity3d.com/Packages/com.unity.entities@0.0/manual/ecs_components.html )를 참조해라.

  

Iterating entities
----

 일치하는 component 세트가 있는 모든 entity를 반복하는 것은 ECS 아키텍처의 중심에 있다. [Accessing entity Data]( https://docs.unity3d.com/Packages/com.unity.entities@0.0/manual/chunk_iteration.html ) 목차를 참조해라. 

  

  

World
----

 World는 EntityManager와 ComponentSystem 세트를 모두 소유한다. 원하는만큼 월드 오브젝트를 만들 수 있다. 일반적으로 simulation World와 rendering 혹은 presentation World를 만든다.

 기본적으로 재생 모드로 들어갈 때 단일 월드를 만들고 프로젝트에서 사용 가능한 모든 ComponentSystem 객체로 채운다. 그러나 기본 월드 생성을 비활성화하고 전역 정의를 통해 고유한 코드로 바꿀 수 있다.

- **Default World creation code** (see file:
  *Packages/com.unity.entities/Unity.Entities.Hybrid/Injection/DefaultWorldInitialization.cs*) 
- **Automatic boostrap entry point** (see file:
  *Packages/com.unity.entities/Unity.Entities.Hybrid/Injection/AutomaticWorldBootstrap.cs*) 

  

  

---

---

  

  

> 마무리

 솔직히 아직 이 파트만 봐서는 무슨 말인지 명확하게 이해가 가지 않는다... 문서를 쭉 읽어가면서 튜토리얼도 병행해야지 이해가 좀 더 빠를듯 싶다. 하지만 새로운 개념을 배워가는 것은 언제나 즐겁다. 힘내자!


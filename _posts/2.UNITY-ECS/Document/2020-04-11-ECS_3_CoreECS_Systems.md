---
title: "ECS_3_CoreECS_Systems"
excerpt: "Entity Component System : System"

categories:
- Game Development

tags:
- Unity
- ECS

last_modified_at : 2020-04-11
---

> 시작하면서

 이번 목차는 ECS에서 S인 **System**이다. 여러번 읽고 정확하게 이해하는 것이 목표이다.

​    

---

---

[Unity Original Document Link](https://docs.unity3d.com/Packages/com.unity.entities@0.0/manual/ecs_systems.html)

Systems
====

 ECS의 S인 System은 component data를 현재 상태에서 다음 상태로 변환하는 논리를 제공한다. 예를 들어, 시스템은 모든 이동 entity의 위치를 이전 프레임 이후 시간 간격의 속도로 시간을 업데이트 할 수 있다.

​    

​    

ComponentSystem
====

 Unity의 ComponentSystem(표준 ECS 용어로 시스템이라고 함)은 entity에 대한 작업을 수행한다. ComponentSystem은 인스턴스 데이터를 포함할 수 없다. 이것을 구 Unity 시스템의 관점에서 보면 구 Component 클래스와 비슷하지만 메소드만 포함하는 클래스와 비슷하다. 하나의 ComponentSystem은 일치하는 component 세트(EntityQuery라고 하는 구조체 내에 정의됨)로 모든 entity를 업데이트한다.

 Unity ECS는 코드에서 확장 할 수 있는 ComponentSystem이라는 추상 클래스를 제공한다.

See file:   */Packages/com.unity.entities/Unity.Entities/ComponentSystem.cs*. 

​    

​    

JobComponentSystem
====

Automatic job dependency management
----

 종속성 관리는 어렵다. 이것이 JobComponentSystem에서 자동으로 수행하는 이유이다. 규칙은 간단하다. 다른 시스템의 작업은 동일한 유형의 IComponentData에서 병렬로 읽을 수 있다. 작업 중 하나가 데이터에 쓰는 경우 병렬로 실행할 수 없으며 작업간 종속성으로 예약된다.

```c#
public class RotationSpeedSystem : JobComponentSystem
{
    [BurstCompile]
    struct RotationSpeedRotation : IJobForEach<Rotation, RotationSpeed>
    {
        public float dt;

        public void Execute(ref Rotation rotation, [ReadOnly]ref RotationSpeed speed)
        {
            rotation.value = math.mul(math.normalize(rotation.value), quaternion.axisAngle(math.up(), speed.speed * dt));
        }
    }

    // Any previously scheduled jobs reading/writing from Rotation or writing to RotationSpeed 
    // will automatically be included in the inputDeps dependency.
    protected override JobHandle OnUpdate(JobHandle inputDeps)
    {
        var job = new RotationSpeedRotation() { dt = Time.deltaTime };
        return job.Schedule(this, inputDeps);
    } 
}
```

​     

How does this work?
----

 모든 작업 및 시스템은 읽거나 쓰는 ComponentType을 선언한다. 결과적으로 JobComponentSystem이 JobHandle을 리턴하면 EntityManager 및 읽기 또는 쓰기 여부에 대한 정보를 포함한 모든 유형에 자동으로 등록된다.

 따라서 시스템이 Component A에 쓰고, 나중에 다른 시스템이 Component A에서 읽는 경우 JobComponentSystem은 읽고 있는 유형 목록을 살펴보고 첫 번째 시스템의 작업에 대한 종속성을 전달한다.

 JobComponentSystem은 필요한 경우 작업을 종속 항목으로 연결하기 때문에 메인 스레드에서 중단이 발생하지 않는다. 그러나 non-job ComponentSystem이 동일한 데이터에 액세스하면 어떻게 될까? 모든 액세스가 선언되므로 ComponentSystem은 OnUpdate를 호출하기 전에 시스템이 사용하는 구성 요소 유형에 대해 실행중인 모든 작업을 자동으로 완료한다.    

​    

Dependency management is conservative & deterministic
---

 종속성 관리는 보수적이다. ComponentSystem은 사용된 모든 EntityQuery 객체를 추적하고, 이를 기반으로 작성하거나 읽은 유형을 저장한다.

 또한, 단일 시스템에서 여러 작업을 스케줄 할 때 종속성이 다른 작업이 덜 필요하더라도 모든 작업에 종속성을 전달해야한다. 이것이 성능 문제인 것으로 판명되면 가장 좋은 해결책은 시스템을 두 개로 나누는 것이다.

 Dependency management approach는 보수적이다. 매우 간단한 API를 제공하면서 결정적이고 올바른 동작을 허용한다.    

​    

Sync points
----

 모든 구조적 변화에는 확실한 동기점이 있다. CreateEntity, Instantiate, Destroy, AddComponent, RemoveComponent, SetSharedComponentData에는 모두 하드 동기점이 있다. 예를 들어 JobComponentSystem을 통해 스케줄된 모든 작업이 엔티티를 작성하기 전에 완료됨을 의미한다. 이것은 자동으로 발생한다. 예를 들어, 프레임 중간에서 EntityManager.CreateEntity를 호출하면 월드에서 이전에 스케줄된 모든 작업이 완료되기를 기다리는 큰 실속이 발생할 수 있다.

 게임 플레이 중에 Entity를 만들 때 동기 점을 피하는 방법에 대한 자세한 내용은 EntityCommandBuffer를 참조해라.    

​    

Multiple Worlds
----

 모든 World에는 고유한 EntityManager가 있으므로 별도의 JobHandle 종속성 관리 세트가 있다. 한 world의 hard sync point는 다른 world에 영향을 미치지 않는다. 결과적으로 스트리밍 및 절차 생성 시나리오의 경우 한 world에서 entity를 만든 다음 프레임 시작시 한 transaction에서 다른 world로 entity를 이동하는 것이 유용하다.

 절차 생성 및 스트리밍 시나리오 및 시스템 업데이트 순서에 대한 sync point를 피하는 방법에 대한 자세한 내용은 ExclusiveEntityTransaction을 참조해라.    

​    

​    

Entity Command Buffer
====

 EntityCommandBuffer 클래스는 두 가지 중요한 문제를 해결한다.

1. job을 진행하는 도중에는 EntityManager에 접근할 수 없다.
2. EntityManager에 접근해(즉, Entity 작성) 삽입된 모든 배열 및 EntityQuery를 무효화한다.

 EntityCommandBuffer 추상화를 사용하면 작업 또는 기본 스레드에서 변경 사항을 큐에 추가해 나중에 기본 스레드에서 적용할 수 있다. EntityCommandBuffer를 사용하는 두 가지 방법이 있다.

 메인 스레드에서 업데이트되는 ComponentSystem 서브 클래스는 자동으로 호출되는 PostUpdateCommands를 사용할 수 있다. 사용하려면 속성을 참조하고, 변경 사항을 대기열에 추가해라. 시스템의 업데이트 기능에서 돌아온 직후 자동으로 world에 적용된다.

 예를 들면 다음과 같다:

```c#
PostUpdateCommands.CreateEntity(TwoStickBootstrap.BasicEnemyArchetype);
PostUpdateCommands.SetComponent(new Position2D { Value = spawnPosition });
PostUpdateCommands.SetComponent(new Heading2D { Value = new float2(0.0f, -1.0f) });
PostUpdateCommands.SetComponent(default(Enemy));
PostUpdateCommands.SetComponent(new Health { Value = TwoStickBootstrap.Settings.enemyInitialHealth });
PostUpdateCommands.SetComponent(new EnemyShootState { Cooldown = 0.5f });
PostUpdateCommands.SetComponent(new MoveSpeed { speed = TwoStickBootstrap.Settings.enemySpeed });
PostUpdateCommands.AddSharedComponent(TwoStickBootstrap.EnemyLook);
```

 보다시피 API는 EntityManager API와 매우 유사하다. 이 모드에서는 자동 EntityCommandBuffer를 편리하게 생각해 시스템 내부에서 배열 무효화를 방지하면서도 여전히 world를 변경할 수 있다.

 For jobs, 기본 스레드의 entity command buffer system에서 EntityCommandBuffer를 요청해 작업에 전달해야 한다. EntityCommandBufferSystem이 업데이트되면 명령 버퍼가 작성된 순서로 기본 스레드에서 재생된다. 이 추가 단계는 메모리 관리를 중앙 집중화하고 생성된 entity 및 component의 결정을 보장할 수 있도록 필요하다.

 다시 두 개의 stick shooter 샘플을 보고 이것이 실제로 어떻게 작동하는지 보자.    

​    

Entity Command Buffer Systems
----

 기본 World 초기화는 초기화-시뮬레이션-프레젠테이션을 위해 각 프레임마다 순서대로 업데이트되는 세 가지 시스템 그룹을 제공한다. 그룹 내에는 그룹의 다른 시스템보다 먼저 실행되고 그룹의 다른 모든 시스템 다음에 실행되는 entity command buffer 시스템이 있다. 동기화 지점을 최소화하기 위해 자체 명령을 작성하는 대신 기존 명령 버퍼 시스템 중 하나를 사용해야 한다. 기본 그룹 및 명령 버퍼 시스템 목록은 기본 시스템 그룹을 참조해라.    

​    

Using EntityCommandBuffers from ParallelFor jobs
----

 EntityCommandBuffer를 사용해 ParellelFor 작업에서  EntityManager 명령을 실행할 때 EntityCommandBuffer.Concurrent 인터페이스는 스레드 안정성 및 결정적인 재생을 보장하는데 사용된다. 이 인터페이스의 공용 메소드는 추가 jobindex 매개 변수를 사용하며, 이는 기록된 명령을 결정적인 순서로 재생하는데 사용된다. jobIndex는 각 작업의 고유 ID이어야 한다. 성능상의 이유로 jobIndex는 IJobParallelFor.Execute()에 전달된 (증가) 인덱스 값이어야한다. 실제로 수행중인 작업을 알지 못하면 인덱스를 jobIndex로 사용하는 것이 가장 안전한 선택이다. 다른 jobIndex 값을 사용하면 올바른 출력이 생성되지만 경우에 따라 성능에 심각한 영향을 미칠 수 있다.    

​     

​     

System Update Order
====

 Component System 그룹을 사용해 시스템의 업데이트 순서를 지정해라. 시스템 클래스 선언에서 [UpdateInGroup] 속성을 사용해 시스템을 그룹에 배치 할 수 있다. 그런 다음 [UpdateBefore] 및 [UpdateAfter] 속성을 사용해 그룹 내 업데이트 순서를 지정할 수 있다.

 ECS 프레임 워크는 프레임의 올바른 단계에서 시스템을 업데이트하는데 사용할 수 있는 기본 시스템 그룹 세트를 작성한다. 그룹의 모든 시스템이 올바른 단계로 업데이트된 다음 그룹 내 순서에 따라 업데이트 되도록 한 그룹을 다른 그룹 안에 중첩시킬 수 있다.    

​    

Component System Groups
----

ComponentSystemGroup 클래스는 특정 순서로 함께 업데이트해야 하는 관련 구성 요소 시스템 목록을 나타낸다. ComponentSystemGroup은 ComponentSystemBase에서 파생되므로 모든 중요한 방식으로 구성 요소 시스템처럼 작동한다. 다른 시스템과 관련해 주문할 수 있고 OnUpdate() 메서드가 있다. 가장 관령성이 높은 것은 component system group에 중첩되어 계층 구조를 형성할 수 있음을 의미한다.

 기본적으로 ComponentSystemGroup의 Update() 메서드가 호출되면 정렬된 멤버 시스템 목록의 각 시스템에서 Update()를 호출한다. 멤버 시스템 자체가 시스템 그룹인 경우 자체 멤버를 재귀적으로 업데이트한다. 결과 시스템 순서는 트리의 깊이 우선 순회를 따른다.    

​    

System Ordering Attributes
----

 기존 system ordering 속성은 semantic과 restriction이 약간 다르게 유지된다.

- [UpdateInGroup] - 이 시스템에 속해 있어야하는 ComponentSystemGroup을 지정한다. 이 속성을 생략하면 시스템이 기본 World's SimulationSystemGroup에 자동으로 추가된다.
- [UpdateBefore] and [UpdateAfter] - 다른 시스템들과 순서를 정렬한다. 이러한 속성에 지정된 시스템 유형은 동일한 그룹의 구성원이어야 한다. 그룹 경계를 넘어서는 순서는 두 시스템을 모두 포함하는 가장 적절한 그룹에서 처리한다.
  - **Example** : SystemA가 GroupA에 있꼬, SystemB가 GroupB에 있고, GroupA와 GroupB가 모두 GroupC의 구성원인 경우 GroupA와 GroupB의 순서는 SystemA와 SystemB의 상대적 순서를 내재적으로 결정한다. 시스템의 명시적인 순서는 필요하지 않다.
- [DisableAutoCreation] - 기본 world 초기화 동안 시스템이 생성되는 것을 방지한다. 시스템을 명시적으로 작성하고 업데이트 해야 한다. 그러나 이 태그가 있는 시스템을 ComponentSystemGroup의 업데이트 목록에 추가하면 해당 목록의 다른 시스템과 마찬가지로 자동으로 업데이트된다.    

​    

Default System Groups
----

 기본 World에는 ComponentSystemGroup instance의 계층 구조가 포함된다. 단 하나의 루트 레벨 시스템 그룹만 Unity 플레이어 루프에 추가된다. (다음 목록은 각 그룹의 사전 정의된 멤버 시스템도 표시함)

- InitializationSystemGroup (플레이어 루프의 초기화 단계가 끝날 때 업데이트 됨)
  - BeginInitializationEntityCommandBufferSystem
  - CopyInitialTransformFromGameObjectSystem
  - SubSceneLiveLinkSystem
  - SubSceneStreamingSystem
  - EndInitializationEntityCommandBufferSystem
- SimulationSystemGroup (플레이어 루프의 업데이트 단계가 끝날 때 업데이트 됨)
  - BeginSimulationEntityCommandBufferSystem
  - TransformSystemGroup
    - EndFrameParentSystem
    - CopyTransformFromGameObjectSystem
    - EndFrameTRSToLocalToWorldSystem
    - EndFrameTRSToLocalToParentSystem
    - EndFrameLocalToParentSystem
    - CopyTransformToGameObjectSystem
  - LateSimulationSystemGroup
  - EndSimulationEntityCommandBufferSystem

- PresentationSystemGroup (플레이어 루프가 PreLateUpdate 단계가 끝날 때 업데이트 됨)
  - BeginPresentationEntityCommandBufferSystem
  - CreateMissingRenderBoundsFromMeshRenderer
  - RenderingSystemBootstrap
  - RenderBoundsUpdateSystem
  - RenderMeshSystem
  - LODGroupSystemV1
  - LodRequirementsUpdateSystem
  - EndPresentationEntityCommandBufferSystem

 이 목록의 특정 내용은 변경 될 수 있다.    

​    

Multiple Worlds
----

 위에서 설명한 기본 월드 외에 여러 월드를 만들 수 있다. 동일한 ComponentSystem 클래스를 둘 이상의 월드에서 인스턴스화 할 수 있으며 각 인스턴스는 업데이트 순서의 다른 지점에서 다른 속도로 업데이트 될 수 있다.

 현재 특정 세계의 모든 시스템을 수동으로 업데이트 할 수 있는 방법이 없다. 대신 어떤 시스템에서 어떤 시스템을 만들고 어떤 시스템을 추가해야하는지 제어 할 수 있다. 따라서 사용자 정의 WorldB는 SystemX와 SystemY를 인스턴스화해 기본 World의 SimulationSystemGroup에 SystemX를 추가하고 기본 World의 PresentationSystemGroup에 SystemY를 추가 할 수 있다. 이러한 시스템은 평상시처럼 그룹 형제 자매와 관련해 주문할 수 있으며 해당 그룹과 함께 업데이트된다.

 이 사용 사례를 지원하기 위해 새로운 ICustomBoostrap 인터페이스를 사용할 수 있다:

```c#
public interface ICustomBootstrap
{
    // Returns the systems which should be handled by the default bootstrap process.
    // If null is returned the default world will not be created at all.
    // Empty list creates default world and entrypoints
    List<Type> Initialize(List<Type> systems);
}
```

 이 인터페이스를 구현하면 기본 world 초기화 전에 component system 유형의 전체 목록이 클래스 Initialize() 메서드로 전달된다. custom boostrapper는 이 목록을 반복하고 원하는 세계에서 시스템을 만들 수 있다. Initialize() 메서드에서 시스템 목록을 반환 할 수 있으며 일반적인 기본 world 초기화의 일부로 시스템이 생성된다.

 예를 들어, 다음은 사용자 정의 MyCustomBoostrap.Initialize() 구현의 일반적인 절차이다.

1. 추가 월드 및 해당 최상위 ComponentSystemGroup을 작성해라.
2. System Type 목록의 각 유형에 대해:
   1. 이 시스템 유형의 최상위 그룹을 찾으려면 ComponentSystemGroup 계층 구조를 위로 이동해라.
   2. 1단계에서 만든 그룹 중 하나인 경우 해당 월드에서 시스템을 만들고 group.AddSystemToUpdateList()를 사용해 계층에 추가해라.
   3. 그렇지 않으면 이 유형을 목록에 추가해 DefaultWorldInitialization으로 돌아가라.
3. new 최상위 그룹에서 group.SortSystemUpdateList()를 호출해라.
   1. 선택적으로 기본 world 그룹 중 하나에 추가
4. 처리되지 않은 시스템 목록을 DefaultWorldInitalization으로 되돌린다.

**Note**: ECS 프레임 워크는 reflection을 통해 ICustomBoostrap 구현을 찾는다.    

​     

Tips and Best Practies
----

- **[UpdateInGroup]을 사용해 작성하는 각 시스템의 시스템 그룹을 지정해라.** 지정하지 않으면 내재된 기본 그룹은 SimulationSystemGroup이다.
- **수동으로 선택된 ComponentSystemGroup을 사용해 Unity 플레이어 루프의 다른 곳에서 시스템을 업데이트해라.** [DisableAutoCreation] 속성을 component system(혹은 System Group)에 추가하면 해당 속성이 기본 시스템 그룹에 작성되거나 추가되지 않는다. World.GetOrCreateSystem()을 사용해 시스템을 수동으로 작성하고, 기본 스레드에서 MySystem.Update()를 수동으로 호출해 시스템을 업데이트 할 수 있다.
  이는 Unity 플레이어 루프의 다른 곳에 시스템을 삽입하는 쉬운 방법이다. (예: 프레임에서 나중에 또는 이전에 실행해야하는 시스템이 있는 경우)
- **가능하면 기존 시스템을 새로 추가하는 대신 기존 EntityCommandBufferSystem을 사용해라.** EntityCommandBufferSystem은 기본 스레드가 미해결 EntityCommandBuffer를 처리하기 전에 작업자 스레드가 완료되기를 기다리는 동기화 지점을 나타낸다. 각 루트 수준 시스템 그룹에서 predefined Begin/End system 중 하나를 재사용하면 새로운 것을 생성하는 것보다 프레임 파이프 라인에 새로운 "bubble"이 발생할 가능성이 줄어든다.
- **ComponentSystemGroup.OnUpdate()에 custom logic을 넣지 말아라.**  ComponentSystemGroup은 기능적으로 컴포넌트 시스템 자체이므로 OnUpdate() 메소드에 사용자 정의 처리를 추가해 일부 작업을 수행하고, 일부 작업을 생성하는 등의 유혹이 있을 수 있다. 일반적으로 외부에서 명확하지 않기 때문에 일반적으로 이에 대해 권장하지 않는다. 그룹 구성원 업데이트 전 후에 사용자 지정 논리가 실행되는지 여부 시스템 그룹을 그룹화 메커니즘으로 제한하고, 그룹과 관련해 명시적으로 정렬된 별도의 구성 요소 시스템에서 원하는 로직을 구현하는 것이 좋다.    

​     

​      

> 마무리

 튜토리얼을 진행하면서 봤던 키워드들이 많이 나왔다. 바로 전부 이해하려고 노력하기 보다는 샘플을 진행하면서 자연스럽게 습득하는 것이 좋아보이는 파트이다. (너무 많기도 하고…) ECS의 System을 효율적으로 사용하려면 프로젝트를 진행하면서도 구글링을 계속해서 최적화가 가능하도록 하는 키워드와 예제들을 잘 살펴봐야 할듯하다.
---
title: "Implementing Job System"
excerpt: "Implementing Job System"

categories:
- Game Development

tags:
- Unity
- C#
- ECS

last_modified_at: 2020-03-23
---

> 시작하면서

 ECS를 공부하면서 좋은 튜토리얼이 있나 [Unity Tutorial](https://learn.unity.com/tutorial/entity-component-system?language=en#) 페이지를 찾아보았다. 하지만 아쉽게도 첫 번째 영상은 시간이 오래 지나서 지금 버전에 맞지 않고, 2,3번 영상은 내용이 딱히 없어서 따라할 수가 없었다. 그래서 4번 영상의 Job System을 간단히 따라하면서 진행해보았다.

  

> 코드가 없다?

 그나마 따라할 수 있는 프로젝트가 있어서 좋아했는데 코드랑 에셋이 보이지 않았다 ㅜㅜ... 그래서 그냥 큐브 에셋에 동영상에 있는 코드를 따라 적으면서 진행했다.

  

#MovementJob.cs

```c#
using Unity.Jobs;
using UnityEngine;
using UnityEngine.Jobs;

namespace Shooter.JobSystem
{
    public struct MovementJob : IJobParallelForTransform
    {
        public float MoveSpeed;
        public float TopBound;
        public float BottomBound;
        public float DeltaTime;

        public void Execute(int index, TransformAccess transform)
        {
            Vector3 position = transform.position;

            position += MoveSpeed * DeltaTime * (transform.rotation * new Vector3(0f, 0f, 1f));

            if (position.z < BottomBound)
            {
                position.z = TopBound;
            }

            transform.position = position;
        }
    }
}
```

  

  

#GameManager.cs

```c#
using Unity.Collections;
using Unity.Entities;
using Unity.Jobs;
using UnityEngine;
using UnityEngine.Jobs;

namespace Shooter.JobSystem
{
    public class GameManager : MonoBehaviour
    {
        #region GAME_MANAGER_STUFF
        public static GameManager GameManagerInstance;

        [Header("Simulation Settings")]
        public float TopBound = 16.5f;
        public float BottomBound = -13.5f;
        public float LeftBound = -23.5f;
        public float RightBound = 23.5f;

        [Header("Enemy Settings")]
        public GameObject EnemyShipPrefab;
        public float EnemySpeed = 1.0f;

        [Header("Spawn Settings")]
        public int EnemyShipCount = 1;
        public int EnemyShipIncremement = 1;

        int Count;
        #endregion

        TransformAccessArray _TransformAccessArray;
        MovementJob _MovementJob;
        JobHandle _JobHandle;

        private void OnDisable()
        {
            _JobHandle.Complete();
            _TransformAccessArray.Dispose();
        }

        private void Start()
        {
            _TransformAccessArray = new TransformAccessArray(0, -1);

            AddShips(EnemyShipCount);
        }

        private void Update()
        {
            _JobHandle.Complete();

            if (Input.GetKeyDown("space"))
            {
                AddShips(EnemyShipIncremement);
            }

            _MovementJob = new MovementJob()
            {
                MoveSpeed = EnemySpeed,
                TopBound = TopBound,
                BottomBound = BottomBound,
                DeltaTime = Time.deltaTime,
            };

            _JobHandle = _MovementJob.Schedule(_TransformAccessArray);

            JobHandle.ScheduleBatchedJobs();
        }

        private void AddShips(int amount)
        {
            _JobHandle.Complete();

            _TransformAccessArray.capacity = _TransformAccessArray.length + amount;

            for (int i = 0; i < amount; i++)
            {
                float xVal = Random.Range(LeftBound, RightBound);
                float zVal = Random.Range(0f, 10f);

                Vector3 position = new Vector3(xVal, 0f, zVal + TopBound);
                Quaternion rotation = Quaternion.Euler(0f, 180f, 0f);

                var obj = Instantiate(EnemyShipPrefab, position, rotation) as GameObject;

                _TransformAccessArray.Add(obj.transform);
            }

            Count += amount;
        }
    }
}
```

  

  

> 마무리

 이 코드들은 따로 설명은 적지 않으려고 한다. (방식만 새로울 뿐 딱히 설명은 필요 없는듯 해서) 해당 코드를 작성하고, Ship Prefab에 오브젝트를 붙여주면 된다.

 기존에 사용하던 로직들과 달라서 새로운 언어를 배우는 기분(?)이었다. 재밌군!
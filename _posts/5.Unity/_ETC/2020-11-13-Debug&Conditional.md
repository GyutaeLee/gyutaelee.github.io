---
title: "Unity Debug 모드에서만 함수를 호출하려면 어떻게 해야할까?"
excerpt: "Unity Tip : Conditional"

categories:
- Game Development

tags:
- Unity
---

> 시작하면서

​	Unity 작업 도중에 로그를 남겨야하는 상황이 많았다. Unity에서는 Debug.Log 등의 디버그 함수들을 릴리즈 모드에서 자동으로 삭제시켜주지 않는다. 릴리즈 모드에서 디버그 로그를 출력하는 것은 불필요한 연산이므로 디버그 모드에서만 로그가 출력되도록 하는 기능이 필요했다.

  

> 방법

​	먼저 Debug.Log 등 유니티의 디버그 함수들을 함수로 따로 묶어서 만드는 구조를 생각했다(로그를 사용할때마다 예외 처리를 따로 적으면 작업이 귀찮아지므로). 평소에 c/c++에서 사용하던대로 #define을 사용해서 예외처리를 하고자 했다. 그런데 여기서 고민이 생겼다. #define으로 함수 내부에서 디버그 모드에만 호출하도록 해도 결국 함수는 호출되므로 불필요하게 함수가 스택에 들어갔다가 나와야한다. 나는 이 과정조차도 없애고 싶었고, Unity로 작업하는 동기들에게 문의를 했다.

​	문의를 한 결과 **[System.Diagnostics.Conditional("@@@")]** 키워드를 알려주었다. 해당 키워드가 선언된 함수를 호출하는 코드는 "@@@"가 선언되어있지 않으면 컴파일 단계에서 호출하는 라인을 삭제한다. 딱 내가 원하는 결과였다.

​	하지만 해당 함수는 그대로 들어간다. 그래서 내부에도 #if 를 한 번 더 씌워서 릴리즈 빌드에서는 로그 관련 코드도 들어가지 않도록 했다. (사실 큰 차이는 없지만 습관을 들이기 위해서)

​    

```c#
using System;
using UnityEngine;

public static class Debug
{
    public static bool isDebugBuild
    {
        get { return UnityEngine.Debug.isDebugBuild; }
    }

    [System.Diagnostics.Conditional("DEBUG_MODE")]
    public static void Log(object message)
    {
#if (DEBUG_MODE)
        UnityEngine.Debug.Log(message);
#endif
    }

    [System.Diagnostics.Conditional("DEBUG_MODE")]
    public static void Log(object message, UnityEngine.Object context)
    {
#if (DEBUG_MODE)
        UnityEngine.Debug.Log(message, context);
#endif
    }

    [System.Diagnostics.Conditional("DEBUG_MODE")]
    public static void LogFormat(string format, params object[] args)
    {
#if (DEBUG_MODE)
        UnityEngine.Debug.LogFormat(format, args);
#endif
    }

    [System.Diagnostics.Conditional("DEBUG_MODE")]
    public static void LogError(object message)
    {
#if (DEBUG_MODE)
        UnityEngine.Debug.LogError(message);
#endif
    }

    [System.Diagnostics.Conditional("DEBUG_MODE")]
    public static void LogError(object message, UnityEngine.Object context)
    {
#if (DEBUG_MODE)
        UnityEngine.Debug.LogError(message, context);
#endif
    }

    [System.Diagnostics.Conditional("DEBUG_MODE")]
    public static void LogWarning(object message)
    {
#if (DEBUG_MODE)
        UnityEngine.Debug.LogWarning(message.ToString());
#endif
    }

    [System.Diagnostics.Conditional("DEBUG_MODE")]
    public static void LogWarning(object message, UnityEngine.Object context)
    {
#if (DEBUG_MODE)
        UnityEngine.Debug.LogWarning(message.ToString(), context);
#endif
    }

    [System.Diagnostics.Conditional("DEBUG_MODE")]
    public static void DrawLine(Vector3 start, Vector3 end, Color color = default(Color), float duration = 0.0f, bool depthTest = true)
    {
#if (DEBUG_MODE)
        UnityEngine.Debug.DrawLine(start, end, color, duration, depthTest);
#endif
    }

    [System.Diagnostics.Conditional("DEBUG_MODE")]
    public static void DrawRay(Vector3 start, Vector3 dir, Color color = default(Color), float duration = 0.0f, bool depthTest = true)
    {
#if (DEBUG_MODE)
        UnityEngine.Debug.DrawRay(start, dir, color, duration, depthTest);
#endif
    }

    [System.Diagnostics.Conditional("DEBUG_MODE")]
    public static void Assert(bool condition)
    {
#if (DEBUG_MODE)
        if (!condition) throw new Exception();
#endif
    }
}
```

​      

> 마무리

​	구글링을 해보면 관련해서 코드들이 많이 있었다. 기존 골격에 내가 필요한 기능들을 좀 더 넣어서 코드를 완성했다. 코드에 관해 고민을 할 때마다 드는 생각이지만 언제나 답은 다른 선구자들이 만들어놓았다는 것이다. 답을 찾아서 내가 사용할 때 무식하지 않게 사용하도록 항상 경계하자.
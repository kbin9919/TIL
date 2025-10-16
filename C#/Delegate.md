# C# 델리게이트(Delegate) 정리

## 1. 개념
델리게이트(delegate)는 **메서드를 가리키는 타입 안전한 함수 포인터**이다.  
즉, 특정 시그니처(매개변수와 반환형)를 가진 메서드를 가리키고 호출할 수 있는 객체이다.

---

## 2. 사용 목적
- 콜백(callback) 구현을 위한 구조이다.  
- 이벤트(event)의 기반이 되는 구조이다.  
- 런타임에 호출할 메서드를 동적으로 바인딩하기 위해 사용된다.  
- 여러 메서드를 하나의 델리게이트에 연결하여 순차 호출하는 멀티캐스트(multicast)를 지원한다.  
- 함수를 값처럼 전달하는 고차함수 구현에 사용된다.

---

# 델리게이트(Delegate)의 내부 구조와 동작 원리

---

## 델리게이트의 본질

델리게이트(delegate)는 **`System.MulticastDelegate`** 클래스를 상속하는 **참조형 객체(reference type)**이다.  
모든 델리게이트 타입(`Action`, `Func`, `EventHandler`, 직접 정의한 delegate 등)은 결국 이 클래스를 상속받는 런타임 타입이다.

```csharp
// 선언부
delegate void MyDelegate(string msg);

// 변환되는 클래스 형태
sealed class MyDelegate : MulticastDelegate
{
    public MyDelegate(object target, IntPtr method);
    public virtual void Invoke(string msg);
    public virtual IAsyncResult BeginInvoke(string msg, AsyncCallback callback, object @object);
    public virtual void EndInvoke(IAsyncResult result);
}
```

## 델리게이트의 내부 필드 구조

.NET Reference Source 기준, MulticastDelegate 클래스는 다음과 같은 필드를 가진다.

```csharp
class MulticastDelegate : Delegate
{
    protected object _target;          // 인스턴스 메서드일 때 바인딩된 객체
    protected IntPtr _methodPtr;       // 실제 호출될 함수 포인터
    protected IntPtr _invokeImpl;      // Invoke 메서드 포인터
    protected IntPtr _method;          // MethodInfo 포인터
    protected Delegate _prev;          // 멀티캐스트 연결 시 이전 델리게이트 참조
}
```

ex)
┌────────────────────────────────────────┐
│           MulticastDelegate            │
│────────────────────────────────────────│
│ _target          : object              │ → 인스턴스 메서드일 경우 this 객체
│ _methodPtr       : IntPtr              │ → 함수 포인터
│ _method          : IntPtr(MethodInfo)  │ → 메서드 메타데이터
│ _invocationList  : Delegate[]          │ → 멀티캐스트 연결 시 목록
│ _invocationCount : int                 │ → 연결된 델리게이트 개수
└────────────────────────────────────────┘

# C# 델리게이트(Delegate) 동작 상세 정리

## 1. 델리게이트의 정의와 구조

1.  **본질**: 델리게이트는 {System.MulticastDelegate}를 상속하는 **참조형 클래스**이다.
2.  **역할**: 메서드를 안전하게 캡슐화하는 **타입 안전한 함수 포인터**이다.
3.  **핵심 구성**: 델리게이트는 **(함수 주소, 대상 객체) 쌍**을 가진 호출 핸들러이다.
    * methodPtr: 호출할 메서드의 **함수 주소**이다.
    * target: 인스턴스 메서드일 경우 **대상 객체 참조**이다.
    * invocationList: 멀티캐스트 시 연결된 델리게이트의 **목록**이다.

---

## 2. 델리게이트 동작 과정

1.  **생성**: 델리게이트를 생성하는 것은 **힙에 객체를 생성**하는 과정이다.
2.  **초기화**: 객체 생성 시 함수 주소(methodPtr)와 대상 객체(target)가 저장된다.
3.  **호출**: d() 또는 d.Invoke() 호출 시 CLR은 methodPtr을 통해 **실제 메서드를 실행**하는 것이다.
    * 인스턴스 메서드 호출 시 target이 this로 사용된다.
    * 정적 메서드는 target이 null이다.
4.  **성능**: JIT 최적화 후 호출 성능은 거의 **함수 포인터 호출 수준**이다.

---

## 3. 멀티캐스트와 불변성

1.  **연산**: += 또는 -= 연산 시 Delegate.Combine 또는 Delegate.Remove가 내부적으로 호출되는 것이다.
2.  **불변성**: 이 연산들은 항상 **새로운 델리게이트 객체를 생성**한다. 따라서 델리게이트는 **불변(immutable) 객체**이다.
3.  **반환값**: 멀티캐스트 델리게이트는 여러 메서드 중 **마지막 메서드의 반환값만 반환**된다.
4.  **예외 처리**: 호출 중 하나라도 예외가 발생하면 **이후 메서드는 실행되지 않는다.**
5.  **이벤트**: 델리게이트는 **이벤트 모델의 기반**이다.
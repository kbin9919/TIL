# Interlock

## 개념 요약

`Interlocked`는 .NET에서 제공하는 **원자적(atomic) 연산을 수행하는 정적 클래스**이다.
`Interlocked`의 메서드는 CPU와 런타임 수준에서 단 하나의 불가분 연산으로 실행되므로 **멀티스레드 환경에서 레이스 컨디션을 방지**하는 데 사용된다.

---

## 왜 필요한가

일반적인 대입·증가 연산(`x++`)은 사실상 **읽기 → 계산 → 쓰기**의 3단계로 이루어져서 여러 스레드가 동시에 실행하면 값이 꼬일 수 있다.
`Interlocked`는 이런 연산을 **하나의 원자적 명령으로 보장**하여 동시성 안전을 제공한다.

---

## 근본 원리 — CAS(Compare-And-Swap)

많은 `Interlocked` 연산은 내부적으로 **CAS(비교 후 교환, Compare-And-Swap)**를 사용한다.
CAS는 다음을 한 번에 수행하는 연산이다:

* 메모리 위치의 현재 값을 읽어 기대값과 비교한다.
* 같으면 새 값으로 교체하고 성공(True)을 반환한다.
* 다르면 아무 변경도 하지 않고 실패(False)을 반환한다.

CAS는 락 없이도 동기화를 구현할 수 있게 해주며, `CompareExchange` 계열 메서드가 그 기능을 직접 제공한다.

---

## 주요 메서드와 동작(대표적)

* `Interlocked.Increment(ref int location)`
  *location의 값을 1 증가시키고 그 결과를 반환한다(원자적).*

* `Interlocked.Decrement(ref int location)`
  *location의 값을 1 감소시키고 그 결과를 반환한다(원자적).*

* `Interlocked.Add(ref int location, int value)`
  *location에 value를 더하고 결과를 반환한다(원자적).*

* `Interlocked.Exchange(ref int location, int value)`
  *location을 value로 교체하고 이전 값을 반환한다(원자적).*

* `Interlocked.CompareExchange(ref int location, int value, int comparand)`
  *location의 현재 값이 comparand와 같으면 location에 value를 대입하고 이전 값을 반환한다(원자적).*

* `Interlocked.Read(ref long location)` / `Interlocked.Exchange(ref long, long)`
  *64비트(long) 값에 대해서도 원자적 읽기/교체를 보장한다(특히 32비트 플랫폼에서 중요함).*

* 제네릭/참조 타입용 `CompareExchange<T>(ref T location, T value, T comparand)` 도 제공되어 **참조 교체**에도 사용된다.

---

## 사용 예시(코드)이다

### 1) 단순 카운터 (증가)

```csharp
using System;
using System.Threading;

class Example {
    static int counter = 0;

    static void Worker() {
        for (int i = 0; i < 10000; i++) {
            Interlocked.Increment(ref counter);
        }
    }

    static void Main() {
        Thread t1 = new Thread(Worker);
        Thread t2 = new Thread(Worker);
        t1.Start(); t2.Start();
        t1.Join(); t2.Join();
        Console.WriteLine(counter); // 20000이 정확히 나온다
    }
}
```

### 2) CompareExchange로 조건부 교체 (CAS 패턴)

```csharp
int expected, newValue;
do {
    expected = sharedValue; // 읽기 (비원자적 읽기라도 OK, CAS로 보완)
    newValue = expected + 1;
} while (Interlocked.CompareExchange(ref sharedValue, newValue, expected) != expected);
```

이 루프는 **다른 스레드가 중간에 바꿨으면 다시 시도**하는 락-프리 알고리즘의 전형이다.

### 3) 참조 교체로 락-프리 연결 리스트 헤드 교체

```csharp
class Node { public int Value; public Node Next; }

void Push(Node newNode) {
    do {
        newNode.Next = head;
    } while (Interlocked.CompareExchange(ref head, newNode, newNode.Next) != newNode.Next);
}
```

---

## 장점과 단점

### 장점

* **성능 우수**하다 — 가벼운 동기화, 락 오버헤드 없음(특히 짧은 연산에서 유리).
* **데드락 없음** — 락을 사용하지 않으므로 데드락을 회피할 수 있다.
* **원자성 보장** — 단일 메서드 호출로 원자적 연산 수행.

### 단점 / 주의사항

* **복잡한 원자 연산 조합은 어렵다** — 여러 변수에 대한 원자적 일괄 업데이트는 여전히 락이 필요할 수 있다.
* **루프(스핀)으로 재시도할 수 있음** — CAS 실패 시 재시도 루프가 CPU를 소모할 수 있다.
* **ABA 문제**가 발생할 수 있다 — 값이 A→B→A로 바뀌면 CAS가 변화를 감지하지 못하는 상황. (해결: 버전 태그를 함께 사용하거나 `Interlocked.CompareExchange`로 복합 값 교체)
* **메모리 배리어/가시성** 고려가 필요하다 — Interlocked는 메모리 가시성 보장을 일부 제공하지만, 복잡한 메모리 모델에서는 추가 조심이 필요하다.

---

## ABA 문제와 해결 방식

* **ABA 문제**: 스레드가 A를 읽고 작업 중, 다른 스레드가 A→B→A로 바꾸면 원래 스레드는 아무 변화가 없다고 판단해 잘못 동작할 수 있다.
* 해결 방법: **값과 버전(태그)** 을 묶어서(예: `Tuple<Value, Version>` 또는 64비트에 value와 counter를 인코딩) CAS할 때 버전도 비교하여 ABA를 방지한다.

---

## 메모리 모델·가시성 참고

* `Interlocked`는 내부에서 **메모리 배리어(memory barrier)** 를 사용하여 연산 전후의 메모리 가시성을 보장하는 경우가 있다.
* 따라서 `volatile`과 `Interlocked`를 혼용할 때는 의도한 가시성이 유지되는지 검토해야 한다(대체로 Interlocked만으로도 충분한 경우가 많다).
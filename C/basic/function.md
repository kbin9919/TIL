# C와 C#

## 공통점

1. **함수 정의 방식**

   * 두 언어 모두 `반환형 + 함수이름 + 매개변수` 형태를 사용한다.

   ```c
   // C
   int add(int a, int b) {
       return a + b;
   }
   ```

   ```csharp
   // C#
   int Add(int a, int b) {
       return a + b;
   }
   ```

2. **반환값(Return)**

   * 둘 다 `return` 키워드로 함수 결과를 반환한다.
   * 반환형이 `void`이면 값을 반환하지 않는다.

3. **매개변수 전달 방식**

   * 기본적으로 "값에 의한 전달(Call by Value)"을 한다.
   * 필요할 때 참조로 전달할 수 있다(C는 포인터, C#은 ref/out 키워드).

---

## 차이점

1. **언어 성격 차이**

   * **C**: 절차적 언어 → 함수는 독립적으로 존재한다.
   * **C#**: 객체지향 언어 → 대부분의 함수는 클래스 내부에 "메서드(Method)" 형태로 존재한다.

   ```c
   // C (전역 함수 가능)
   void hello() {
       printf("Hello\n");
   }
   ```

   ```csharp
   // C# (클래스 안에 메서드로 작성)
   class Program {
       static void Hello() {
           Console.WriteLine("Hello");
       }
   }
   ```

---

2. **접근 제한자**

   * C언어는 **접근제한자가 없다**. (`static`으로 파일 단위 제한만 가능)
   * C#은 `public`, `private`, `protected`, `internal` 등을 제공해 메서드의 접근 범위를 명확히 지정한다.

---

3. **오버로딩(Overloading)**

   * C: **지원하지 않음** → 같은 이름의 함수를 매개변수만 다르게 정의할 수 없음.
   * C#: **지원함** → 매개변수 타입이나 개수에 따라 같은 이름의 메서드를 여러 개 정의 가능.

   ```csharp
   // C# 메서드 오버로딩
   void Print(int x) { Console.WriteLine(x); }
   void Print(string s) { Console.WriteLine(s); }
   ```

---

4. **기본 제공 기능**

   * C: 표준 라이브러리 함수(printf, scanf 등)만 제공.
   * C#: .NET Framework 라이브러리를 통한 풍부한 내장 메서드 제공 (예: `Console.WriteLine`, `List<T>.Add`).

---

## 정리

* **공통점**: 함수 정의 구조, 반환/매개변수 개념, 값/참조 전달 방식.
* **차이점**:

  * C는 절차적, C#은 객체지향 (함수 vs 메서드).
  * 접근제한자(C#만 있음).
  * 오버로딩(C#만 지원).
  * 표준 라이브러리 범위 차이.

--
## 비교

| 구분     | C 함수              | C# 메서드                                 |
| ------ | ----------------- | -------------------------------------- |
| 소속     | 전역(파일 단위)         | 클래스/구조체 내부                             |
| 접근 제한  | 없음 (`static` 정도만) | `public`, `private` 등 세부적              |
| 데이터 결합 | 불가능 (독립적)         | 필드와 결합 가능                              |
| 메모리 로딩 | 코드 영역에 한 번만 올라감   | static은 클래스당 1개, 인스턴스 메서드는 객체별 this 참조 |
| 객체지향   | X                 | O                                      |

---

## 예시

### C
```c
#include <stdio.h>

static void hello_private() { // 이 파일에서만 사용 가능
    printf("Private function in C\n");
}

void hello_public() { // 다른 파일에서도 참조 가능
    printf("Public function in C\n");
}
```

### C\#

```csharp
using System;

class Greeter {
    private void HelloPrivate() { // 클래스 내부에서만 사용 가능
        Console.WriteLine("Private method in C#");
    }

    public void HelloPublic() { // 외부에서도 사용 가능
        Console.WriteLine("Public method in C#");
    }
}
```

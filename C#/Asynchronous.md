# 비동기(Asynchronous) 프로그래밍이란

비동기란 작업이 완료될 때까지 기다리지 않고 다음 작업을 계속 실행하는 방식이다.  

동기(Synchronous)와의 가장 큰 차이점은  
대기 시간 동안 스레드를 점유하지 않는다는 것이다.

async와 await는 C#에서 비동기 프로그래밍을 간결하게 구현하기 위해 사용하는 키워드이다.
이 키워드들은 시간 소모가 큰 작업을 비동기로 처리하여 UI가 멈추는 현상을 방지하고 애플리케이션의 응답성을 유지하는 데 중요한 역할을 한다.

1. async 키워드
정의: async 키워드는 비동기 메서드를 정의할 때 사용한다.

용도: 메서드 앞에 async를 붙이면 해당 메서드가 비동기로 실행될 수 있음을 의미한다. 이 메서드 내에서 await 키워드를 사용할 수 있게 된다.

반환 타입: async 메서드는 보통 Task나 Task<T>를 반환하며, 비동기적으로 실행 후 완료 상태를 나타낸다. 반환값이 없는 경우 Task 타입, 값을 반환하는 경우 Task<T> 타입을 사용한다.

ex) public async Task MyAsyncMethod(), public async Task<int> CalculateAsync()

2. await 키워드
정의: await 키워드는 비동기 작업이 완료될 때까지 기다리는 역할을 한다.
작동 방식: await를 만나면 비동기 작업이 완료될 때까지 대기하고, 완료되면 그 다음 코드가 실행된다. await를 사용하는 동안 메인 스레드는 다른 작업을 계속 수행할 수 있어 UI가 멈추지 않는다.
사용 방법: await는 Task나 Task<T>를 반환하는 메서드 앞에서만 사용할 수 있다.

```csharp
async와 await의 작동 방식 예제
// 비동기 메서드 정의
public async Task<int> FetchDataAsync()
{
    Console.WriteLine("데이터 가져오기 시작...");

    // 예제: 3초 지연 후 데이터를 가져온다고 가정
    await Task.Delay(3000); // 실제 비동기 작업으로 생각 (예: API 호출)
  
    Console.WriteLine("데이터 가져오기 완료!");
    return 42; // 예제 데이터
}

// 호출하는 메서드
public async Task ProcessDataAsync()
{
    Console.WriteLine("데이터 처리 시작...");

    // FetchDataAsync()의 작업이 완료될 때까지 기다리지만 UI는 계속 응답함
    int result = await FetchDataAsync();
  
    Console.WriteLine($"가져온 데이터: {result}");
}
```

왜 async와 await를 사용하는가?
UI 응답성 유지: 시간 소모가 큰 작업(API 호출, 파일 입출력 등)에서 async와 await를 사용하면 UI가 멈추지 않고 사용자가 계속 애플리케이션을 사용할 수 있다.
가독성: 복잡한 비동기 작업을 콜백 방식이 아닌 순차적 코드처럼 작성할 수 있어 가독성이 높아진다.
에러 처리 용이성: 비동기 코드에서 예외 처리를 try-catch로 쉽게 처리할 수 있다.
요약
async는 비동기 메서드를 정의하고 await를 사용할 수 있게 한다.
await는 비동기 작업이 완료될 때까지 대기하는 역할을 하며, 작업이 끝나면 다음 코드를 실행한다.
async와 await는 비동기 프로그래밍을 쉽게 구현하게 하여 프로그램의 응답성과 가독성을 높인다.
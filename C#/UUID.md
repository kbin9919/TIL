# UUID

UUID(Universally Unique Identifier)는 전 세계에서 유일하게 식별 가능한 128비트 길이의 고유 식별자이다.  
주로 데이터베이스의 키, 파일 식별, 분산 시스템에서 객체 식별 등에 사용된다.  
UUID는 생성될 때마다 거의 중복될 가능성이 없기 때문에, 여러 시스템에서 동시에 생성해도 충돌 위험이 낮다.

## UUID 형식

UUID는 16바이트(128비트)를 32자리 16진수로 표현하며, 일반적으로 8-4-4-4-12 형태의 하이픈 구분 문자열로 나타낸다.  
예시: `550e8400-e29b-41d4-a716-446655440000`

## C#에서 UUID 사용 예시

C#에서는 `System.Guid` 구조체를 사용하여 UUID를 다룰 수 있다.

```csharp
using System;

class Program
{
    static void Main()
    {
        // 새로운 UUID 생성
        Guid id = Guid.NewGuid();
        Console.WriteLine("생성된 UUID: " + id.ToString());

        // 문자열로부터 UUID 생성
        string uuidString = "550e8400-e29b-41d4-a716-446655440000";
        Guid parsedId = Guid.Parse(uuidString);
        Console.WriteLine("파싱된 UUID: " + parsedId);

        // UUID 비교
        Guid anotherId = Guid.NewGuid();
        bool isEqual = id == anotherId;
        Console.WriteLine("두 UUID가 같은가? " + isEqual);
    }
}

# UUID 사용 사례

## 1. 데이터베이스 기본 키(PK)
- 여러 서버나 클라이언트에서 동시에 데이터를 생성해도 충돌 없이 고유 ID를 보장함
- 예시:
  - 사용자 ID
  - 주문 ID
  - 트랜잭션 ID

## 2. 분산 시스템 / 마이크로서비스
- 중앙 DB 없이 각 시스템에서 고유 객체를 식별할 수 있음
- 예시:
  - 메시지 큐 메시지 ID
  - 분산 로그 이벤트 ID

## 3. 파일 / 리소스 식별
- 파일, 이미지, 문서 등 고유 이름을 부여할 때 사용됨
- 예시:
  - 업로드 파일 이름
  - 캐시 키

## 4. 세션 / 토큰 관리
- 인증 세션 ID, API 토큰 등에서 유일성 필요
- 예시:
  - JWT ID
  - 세션 쿠키 ID

## 5. 소프트웨어 / 앱 내부 객체 식별
- 앱 내 객체 구분, 설정 저장, 임시 데이터 매핑 등에서 활용됨
- 예시:
  - 게임 캐릭터 ID
  - 앱 설정 레코드 ID


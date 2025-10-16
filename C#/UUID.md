# UUID

UUID(Universally Unique Identifier)는 전 세계에서 유일하게 식별 가능한 128비트 길이의 고유 식별자이다.  
주로 데이터베이스의 키, 파일 식별, 분산 시스템에서 객체 식별 등에 사용된다.  
UUID는 생성될 때마다 거의 중복될 가능성이 없기 때문에, 여러 시스템에서 동시에 생성해도 충돌 위험이 낮다.

## UUID 형식

UUID는 16바이트(128비트)를 32자리 16진수로 표현하며, 일반적으로 8-4-4-4-12 형태의 하이픈 구분 문자열로 나타낸다.  
예시: `550e8400-e29b-41d4-a716-446655440000`

## C#에서 UUIDD 사용 예시

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

# UUID 생성 방식 (버전별)

UUID(Universally Unique Identifier)는 128비트 길이의 고유 식별자로, 버전마다 생성 방식이 다르다.  
대표적으로 사용되는 **v1, v4, v3, v5** 기준으로 정리한다.

---

## 1. UUID 버전별 특징

| 버전 | 생성 방식 | 특징 |
|------|----------|------|
| **v1** | 시간(Time) + MAC 주소 | - 생성 시간 순서대로 정렬 가능<br>- 동일 네트워크 환경에서 유일성 보장<br>- MAC 주소 기반 |
| **v4** | 랜덤(Random) | - 완전히 랜덤<br>- 충돌 가능성 극히 낮음 (1/2^122 수준)<br>- 분산 시스템에서 가장 많이 사용 |
| **v3** | MD5 해시 기반 | - 이름(Name) 기반 생성<br>- 동일 입력이면 항상 같은 UUID<br>- 고유성을 입력에 의존 |
| **v5** | SHA1 해시 기반 | - v3과 동일하지만 SHA1 해시 사용<br>- 이름 기반 UUID, 동일 입력에 대해 항상 동일 결과 |

---
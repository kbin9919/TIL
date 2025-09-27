# 헤더 파일 (Header File)

- 헤더 파일은 **함수 원형, 매크로, 상수, 자료형 정의 등을 모아둔 선언용 파일이다**  
- 확장자는 보통 `.h`  
- `#include` 전처리기로 불러와 사용한다  
- **선언부만** 포함하고, 구현부는 `.c` 파일에 둔다  
- 여러 소스 파일에서 공통 코드를 재사용하고, 모듈화를 돕는다  

---

## 예시

```c
/* mathutils.h */
#ifndef MATHUTILS_H
#define MATHUTILS_H

int add(int a, int b);   // 함수 원형
#define PI 3.14159       // 매크로
typedef unsigned int uint;

#endif

/* mathutils.c */
#include "mathutils.h"

int add(int a, int b) {
    return a + b;
}

/* main.c */
#include <stdio.h>
#include "mathutils.h"

int main() {
    printf("%d\n", add(2, 3));
    return 0;
}
```

# #ifndef
* ifndef는 전처리기 지시문(Preprocessor directive) 이다.
* ifndef → if not defined
* “특정 매크로가 정의되지 않았다면” 이라는 조건을 검사한다.

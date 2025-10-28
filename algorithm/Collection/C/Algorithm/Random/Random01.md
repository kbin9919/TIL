# 랜덤 함수 구현

## 0~99까지 랜덤으로 반환하는 함수 구현하기

- 현재 시간을 나노초 단위로 가져온 후 100만큼 나눈다. 
- 원하는 범위의 랜덤값인 0~99까지 값을 추출하기 위해서 추가로 % 100을 진행한다.
- 결과값을 반환시킨다.

* 나노초 : 10억분의 1초($10^{-9}$초)를 나타내는 시간 단위


```C#
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

// 0~99까지 랜덤으로 반환하는 함수 구현
int random() {
	int randNum = 0;

    struct timespec ts;

    // 현재 시간 가져오기
    timespec_get(&ts, TIME_UTC);

    long currentTime = ts.tv_nsec;
    randNum = currentTime / 100;
	randNum = (randNum % 100); // 0~99까지의 수로 변환

	return randNum;
}

int main() {
    for (int i = 0; i < 10; i++) {
        int x = random();
		printf("Random number %d: %d\n", i + 1, x + 1);
    }
}
```

```C#
출력
Random number 1: 65
Random number 2: 38
Random number 3: 49
Random number 4: 59
Random number 5: 56
Random number 6: 63
Random number 7: 72
Random number 8: 60
Random number 9: 97
Random number 10: 37
```

## p.s
1. 나노초 단위로 가져온 값을 그대로 파싱해서 반환시키는 값은 랜덤이 아닌 것 같다. 가져온 값에서 랜덤처리를 하기 위한 방법이 필요.
2. 구현함 함수를 100만번 호출하고, 0~99 각 숫자가 나올 확률이 어떻게 되는지 확인해보기.

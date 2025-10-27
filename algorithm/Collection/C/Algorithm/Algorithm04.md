# C언어 알고리즘 문제풀이

## 겹치지 않는 랜덤 숫자 만들기

1에서 10까지 겹치지 않는 랜덤 숫자를 출력하는 프로그램을 작성

```csharp
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int maxCnt = 10;
int* CheckCard;

void init() {
	CheckCard = calloc(maxCnt+1, sizeof(int));
}

int DrawCard() {
	int drawCardNum = rand() % maxCnt + 1;
	int check = CheckCard[drawCardNum];

	if (check == 0) {
		CheckCard[drawCardNum] = 1;
		return drawCardNum;
	}
	else {
		DrawCard();
	}
}

int PrintCardNumber(int cnt) {
	if (cnt > maxCnt) {
		// 입력값이 최대 개수를 초과한 경우
		return 0;
	}
	int startCnt = 0;

	while (startCnt < cnt) {
		int drawCard = DrawCard();
		printf("뽑은 카드 숫자 : %d \n", drawCard);
		startCnt++;
	}
}

int main() {
	init();
	PrintCardNumber(10);
}
```

## p.s
1. DrawCard함수는 재귀함수로 구현하기 보다는 while문으로 구현하는 것이 더 쉬운 코드로 작성할 수 있다.
2. 재귀함수는 중첩순회를 시켜야 할 때, 사용하면 유용하다.

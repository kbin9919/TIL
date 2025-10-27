# C언어 알고리즘 문제풀이

## 플레잉 카드 한 벌을 랜덤하게 출력하기

0~51까지의 겹치지 않는 랜덤 숫자 52개와 이를 해석하여 카드로 출력하는 프로그램을 작성
* 0~12 클로버
* 13~25 하트
* 26~38 스페이드
* 39~51 다이아

```csharp
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int maxCnt = 51;
// 0~12 클로버
// 13~25 하트
// 26~38 스페이드
// 39~51 다이아
int* CheckCard;

void init() {
	CheckCard = calloc(maxCnt+1, sizeof(int));
}

int DrawCard() {
	int drawCardNum = rand() % maxCnt;
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
		char* cardType = "";

		if (drawCard <= 12) {
			cardType = "클로버";
		}
		else if (drawCard <= 25) {
			cardType = "하트";
		}
		else if (drawCard <= 38) {
			cardType = "스페이드";
		}
		else if (drawCard <= 51) {
			cardType = "다이아";
		}

		printf("뽑은 카드 : %s %d \n", cardType, drawCard);
		startCnt++;
	}
}

int main() {
	init();
	PrintCardNumber(51);
}
```

## p.s
1. 첫 순회에서 52장을 뽑고, 중복을 제거한 후 다음 순회부터는 아직 나오지 않은 카드의 수 만큼 뽑는 방식으로 로직을 변경하면, 순회를 최소화시킬 수 있다.


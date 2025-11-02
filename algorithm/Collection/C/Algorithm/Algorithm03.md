# C언어 알고리즘 문제풀이

## 몬테카를로 시뮬레이션으로 크랩스 게임 확률 계산하기

주사위 두 개를 백만 번 던져서 나온 숫자의 합을 배열에 저장하고 활률을 출력하는 프로그램을 작성

```c
#include <stdio.h>
#include <time.h>

int sumDice[13];

int CrapsGame(int tryCnt) {
	for (int i = 0; i < tryCnt; i++)
	{
		int fisrtDice = rand() % 6 + 1;
		int secondDice = rand() % 6 + 1;

		sumDice[fisrtDice + secondDice]++;
	}

	for (int i = 2; i <= 12; i++) {
		int prob = sumDice[i] * 100 / tryCnt ;
		printf("%2d 가 나올 확률 : %2d%\n", i, prob);
	}
}

int main() {
	CrapsGame(1000000);
}
```

## p.s
1. 다음 스터디 부터는 출력(printf) 함수랑 알고리즘 함수 분리할 것.
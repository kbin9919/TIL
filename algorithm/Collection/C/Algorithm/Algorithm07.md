# C언어 알고리즘 문제풀이

## 몬테카를로 시뮬레이션으로 원주율 구하기

몬테카를로 시뮬레이션으로 원주율 구하는 프로그램을 작성

```c
//A / N*4#include <stdio.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int iCnt = 0;
int oCnt = 0;

int GetPie()
{
	int x = rand() % 100;
	int y = rand() % 100;
	int r = 50;

	(x - r)* (x - r) + (y - r) * (y - r) <= r * r ? iCnt++ : oCnt++;

	double pie = 0;
}

void PrintPointPie() {
	int printI = 10;

	for (int i = 1; i <= 1000000000; i++) {
		GetPie();

		if (i == printI) {
			double pie = (double)iCnt / (iCnt + oCnt) * 4;
			printf("i : %d pie : %.18f \n ", i, pie);
			printI = printI * 10;
		}
	}
	double pie = (double)iCnt / (iCnt + oCnt) * 4;
	printf("iCnt : %d, oCnt : %d, pie : %.18f \n ", iCnt, oCnt, pie);
}

int main() {
	PrintPointPie();
}
```

## p.s
1. 동시성 문제를 고려해서, 구조체를 사용해보고 DDD형태로 로직을 개선해보기.
2. 결과 표기는 교재에서 제공해주는 값이 아닌, 직접 테스트하면서 추가할 것
	


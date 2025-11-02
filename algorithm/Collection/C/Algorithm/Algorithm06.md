# C언어 알고리즘 문제풀이

## 원주율의 계산

반복문을 이용해서 원주율을 구하는 프로그램을 작성

```c
#include <stdio.h>
#include <stdbool.h>
#include <stdlib.h>
#include <time.h>

int GetPie()
{
	double pie = 0;
	double mulPie = 0;
	bool plus = true;

	for (int i = 1; i < 100000000; i += 2) {

		if (plus)
		{
			mulPie += 1.0 / i;
			plus = false;
		}
		else
		{
			mulPie -= 1.0 / i;
			plus = true;
		}

		double currentPie = mulPie * 4;
		if (i < 10 || i > 99999990) {
			printf("i : %d pie : %.18f \n ", i, currentPie);
		}
	}
	pie = mulPie * 4;
	printf("pie : %.18f \n ", pie);
	return pie;

}

int main() {

	GetPie();
}
```

## p.s
1. 구조체 활용을 생각해볼것.
	
	


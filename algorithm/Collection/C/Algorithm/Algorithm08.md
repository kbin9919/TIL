# C언어 알고리즘 문제풀이

## 열거형으로 커피 가격표 출력하기

열거형 enum을 이용하여 커피 가격표를 출력하는 프로그램을 작성하기

```c
#include <stdio.h>
enum Coffee { Espresso, Americano, Cappuccino, Latte, Mocha };
int price[5] = {1000, 2000, 3000, 3000, 3000};

void PriceInfo() {
	printf("0 : Espresso \n");
	printf("1 : Americano \n");
	printf("2 : Cappuccino \n");
	printf("3 : Latte \n");
	printf("4 : Mocha \n");
	printf("Select Coffee Menu : \n");
	int num;
	int x = scanf("%d", &num);

	if (num == Espresso) {
		printf("Select Coffee Price : %d", price[Espresso]);
	}
	else if (num == Americano) {
		printf("Select Coffee Price : %d", price[Americano]);
	}
	else if (num == Cappuccino) {
		printf("Select Coffee Price : %d", price[Cappuccino]);
	}
	else if (num == Americano) {
		printf("Select Coffee Price : %d", price[Latte]);
	}
	else if (num == Mocha) {
		printf("Select Coffee Price : %d", price[Mocha]);
	}
}

int main() {
	PriceInfo();
}
```
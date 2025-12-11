# C언어 알고리즘 문제풀이

## 반복문은 사용해서 별그림 출력하기

반복문은 사용해서 별그림 출력하는 프로그램을 작성하기

```c
#include <stdio.h>
#include <stdlib.h>

void PrintStart0() {
	for (int i = 0; i < 5; i++) {
		for (int j = 0; j < 10; j++) {
			printf("*");
		}
		printf("\n");
	}
}

void PrintStart1() {
	for (int i = 0; i < 5; i++) {
		for (int j = 0; j < i + 1; j++) {
			printf("*");
		}
		printf("\n");
	}
}

void PrintStart2() {

	for (int i = 1; i < 10; i += 2) {
		for (int j = 0; j < i; j++) {
			printf("*");
		}

		printf("\n");
	}
}

void PrintStart3() {

	for (int i = 5; i > 0; i--) {
		for (int j = 0; j < i; j++) {
			printf("*");
		}

		printf("\n");
	}
}

void PrintStart4() {
	char* text = calloc(11, sizeof(char));

	// 첫 번째 반복문은 5번 순회
	// j는 공백의 개수를 나타내며, i가 증가할수록 감소
	for (int i = 0, j = 10; i < 10; i++, j--) {

		for (int k = 0, l = 0; k < 10; k++) {
			if (k < 5) {
				text[k] = ' ';
			}
			else if (k == 5) {
				text[k] = '*';
			}
			else if (k > 10) {
				text[k] = ' ';
			}

		}

		text[11] = '\0';
		printf("%s", text);
		printf("\n");
	}
}

void PrintStart2_1() {

	char* text = (char*)malloc(10 * sizeof(char));

	for (int i = 0, j = 0; i < 9; i += 2, j = i - 1) {
		text[i] = '*';
		text[j] = '*';

		printf("%s\n", text);
		printf("\n");
	}
}


int main() {
	PrintStart2_1();
}
```
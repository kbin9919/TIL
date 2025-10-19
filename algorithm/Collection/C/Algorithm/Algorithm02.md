2. 랜덤 숫자 만들기

주사위를 30번 던졌을 때 나오는 숫자를 시뮬레이션하기 위해 1~6번까지의 정수를 랜덤하게 30개 만들어서 배열에 저장하고 확인하는 프로그램을 작성
: rand() 함수를 사용할 때, 0~32767까지의 랜덤한 값이 나온다.
: 이 값을 1~6개의 범윗값으로 나눠야 함으로 나머지가 0으로 나누어 떨어지는 32766까지의 값으로 max값으로 설정한다 이외의 값은 다시 랜덤함수를 호출시킨다.
: 6으로 나눈 몫인  5,461만큼 짤라서 1~6값으로 지정한다.

```csharp
#include <stdio.h>
#include <time.h>

int main() {
	int a[10] = { 0 };
	int dice[30];
	int rand_min = 1;
	int tand_Max = 6;

	srand((unsigned int)time(0));

	for (int i = 0; i < 10; i++) {
		int temp = rand() % 6 + 1;
		a[i] = temp;
		//if (temp < 5461) {
		//	a[i] = 1;
		//}
		//else if (temp < 10922) {
		//	a[i] = 2;
		//}
		//else if (temp < 16383) {
		//	a[i] = 3;
		//}
		//else if (temp < 21844) {
		//	a[i] = 4;
		//}
		//else if (temp < 27305) {
		//	a[i] = 5;
		//}
		//else if (temp < 32766) {
		//	a[i] = 6;
		//}
		//else {
		//	// 최댓값이 나온 경우 : 32767 다시 진행
		//	i--;
		//}
	}

	// 배열 출력
	printf("배열 값: ");
	for (int i = 0; i < 10; i++) {
		printf("%d ", a[i]);
	}
	printf("\n");
}
```

## ps
1. 랜덤 함수 직접 구현 해보기
2. % 나머지 연산자를 사요할 수 있는 예시 찾아보기

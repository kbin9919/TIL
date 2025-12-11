# C언어 알고리즘 문제풀이

## 2~1000 까지의 소수 출력하기

2~1000 까지의 소수 출력하는 프로그램을 작성하기

```c
#include <stdio.h>
#include <stdlib.h>


// 2~1000 까지의 소수를 출력하시오
// *소수는 1과 자기 자신으로만 나눠지는 숫자를 의미한다.

int GetPrimeNumberCount() {
   int primeNumberCnt = 0;
   
   int arr[1001];
   // 2~1000 // 홀수만 돌리면 시간복잡도 감소
   for (int i = 2; i <= 1000 / 2; i++) {
      int factor = 0;

      if (arr[i] == 0) {
         continue;
      }

      for (int j = 2; j <= i; j++) {

         if (i % j == 0) {
            factor++;
         }

         if (factor > 1) {
            break;
         }
      }

      // 소수일 때
      if (factor == 1) {
         for (int j = i, k = 1; j <= 1000; k++, j = i * k) {
            arr[j] = 0;
         }

         arr[i] = 1;
      }
   }

   for (int i = 2; i < 1000; i++) {
      if (arr[i] != 0) {
         primeNumberCnt++;
      }
   }
   return primeNumberCnt;
}

int main() {
   int cnt = GetPrimeNumberCount(1000);

   printf("약수의 수 : %d", cnt);
}
```
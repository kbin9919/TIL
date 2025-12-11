# C언어 알고리즘 문제풀이

## 에라토스테네스의 체(소수 판별 알고리즘)를 사용하여 2~1000 까지의 소수 출력하기

에라토스테네스의 체(소수 판별 알고리즘)를 사용하여 2~1000 까지의 소수 출력하는 프로그램을 작성하기

```c
#include <stdio.h>

// 2~1000 까지의 소수를 출력하시오
// *소수는 1과 자기 자신으로만 나눠지는 숫자를 의미한다.

int GetPrimeNumberCount(int MaxCnt) {
   int primeNumberCnt = 0;

   // 2~1000 
   // 홀수만 돌리면 시간복잡도 줄일 수 있음
   for (int i = 2; i <= MaxCnt; i++) {
      int factor = 0;

      for (int j = 2; j <= i; j++) {

         if (i % j == 0) {
            factor++;
         }

         if (factor > 1) {
            break;
         }
      }

      if (factor == 1) {
         primeNumberCnt++;
      }
   }

   return primeNumberCnt;
}

int main() {
   int cnt = GetPrimeNumberCount(1000);

   //168
   printf("약수의 수 : %d", cnt);
}
```
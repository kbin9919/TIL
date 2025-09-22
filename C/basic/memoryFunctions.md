### 1. **malloc (memory allocation)**

* `malloc`은 지정한 **바이트 크기만큼 메모리를 연속적으로 할당하는 함수**이다.
* 성공하면 할당된 메모리의 시작 주소를 반환하고, 실패하면 `NULL`을 반환한다.
* 할당된 메모리는 초기화되지 않고 **쓰레기 값**을 가진다.

```c
int* arr = (int*)malloc(5 * sizeof(int)); // int 5개 공간 할당
```

---

### 2. **calloc (contiguous allocation)**

* `calloc`은 지정한 **개수 × 크기** 만큼 메모리를 연속적으로 할당하는 함수이다.
* `malloc`과 다르게 **0으로 초기화된 메모리**를 반환한다.
* 성공하면 시작 주소를 반환하고, 실패하면 `NULL`을 반환한다.

```c
int* arr = (int*)calloc(5, sizeof(int)); // int 5개 공간 0으로 초기화 후 할당
```

---

### 3. **realloc (re-allocation)**

* `realloc`은 기존에 `malloc`이나 `calloc`으로 할당받은 메모리 블록의 **크기를 변경하는 함수**이다.
* 크기를 늘리면 새 메모리를 확보하고 기존 데이터를 복사해준다.
* 크기를 줄이면 초과 부분을 잘라내고 새 크기에 맞춰준다.

```c
arr = (int*)realloc(arr, 10 * sizeof(int)); // 기존 공간을 10개 int 크기로 늘림
```

---

### 4. **free**

* `free`는 `malloc`, `calloc`, `realloc`으로 할당받은 메모리를 **해제하는 함수**이다.
* 해제 후에는 다시 그 포인터를 사용하면 **정의되지 않은 동작(UB)** 이 발생할 수 있다.
* 보통 `free` 후 포인터를 `NULL`로 초기화한다.

```c
free(arr);  
arr = NULL;
```

---

👉 한 줄 요약

* `malloc` → 메모리 할당, 초기화 없음
* `calloc` → 메모리 할당, **0으로 초기화**
* `realloc` → 메모리 크기 **변경**
* `free` → 메모리 **해제**


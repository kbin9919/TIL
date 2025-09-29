# 이중 원형 연결리스트 C
* 구조체 기반 -> head, last기반 구조 수정
* 함수 원형 선언 (Function Prototype) 추가

# 구현
```C
#include <stdio.h>
#include <stdlib.h>

Node* GetNode(int index);
void Add(void* data);
void AddIndex(int index, void* data);
void Delete(int index);
void* Get(int index);
void Edit(int index, void* data);

typedef struct Node {
	void* Data;
	struct Node* Next;
	struct Node* Prev;
} Node;

Node* head;
Node* last;
int Count;

void Add(void* data) {

	// 신규 노드 생성
	Node* node = (Node*)malloc(sizeof(Node));
	if (node == NULL) {
		// 메모리 할당 실패 처리
		return;
	}
	node->Data = data;

	// 첫 노드인 경우
	if (Count == 0) {
		node->Next = node;
		node->Prev = node;

		head = node;
		last = node;

		Count++;
		return;
	}

	// 추가
	last->Next = node;
	node->Next = head;

	// 새로운 노드의 이전 노드를 마지막 노드로 추가
	node->Prev = last;
	// 처음 노드의 이전 노드를 마지막 노드로 업데이트
	head->Prev = node;
	// 새로운 노드를 마지막 노드로 업데이트
	last = node;

	Count++;
}

void AddIndex(int index, void* data) {

	if (index < 0 || index > Count) {
		return; // 예외 처리 방법 찾아보기
	}

	// 신규 노드 생성
	Node* node = (Node*)malloc(sizeof(Node));
	if (node == NULL) {
		// 메모리 할당 실패 처리
		return;
	}
	node->Data = data;

	// 첫 노드인 경우
	if (Count == 0) {
		head = node;
		last = node;
	}

	// 탐색
	Node* currentNode = GetNode(index);
	Node* beforeNode = currentNode->Prev;

	// 추가
	beforeNode->Next = node;
	currentNode->Prev = node;

	node->Next = currentNode;
	node->Prev = beforeNode;

	Count++;

	if (index == 0) {
		// 노드를 추가한 인덱스가 0인 경우 FirstNode, LastNode를 업데이트
		head = node;
		last = node;
	}
	if (index == Count) {
		// 노드를 추가한 인덱스가 리스트의 크기랑 같은 경우 LastNode를 업데이트
		last = node;
	}
}

void Delete(int index) {

	if (index < 0 || index > Count) {
		return; // 예외 처리 방법 찾아보기
	}

	// 탐색
	Node* currentNode = GetNode(index);
	Node* beforeNode = currentNode->Prev;

	// 삭제
	beforeNode->Next = currentNode->Next;
	currentNode->Next->Prev = beforeNode;

	Count--;

	if (index == 0)
	{
		// 노드를 삭제한 인덱스가 0인 경우 FirstNode를 업데이트
		head = head->Next;
	}
	if (index == Count)
	{
		// 마지막 노드가 삭제된 노드일 경우 lastNode 업데이트
		last = beforeNode;
	}
	free(currentNode);
}

void* Get(int index) {

	// 예외 처리
	if (index < 0 || index >= Count || Count < 1) {
		return; // 예외 처리 방법 찾아보기
	}
	Node* node = GetNode(index);
	return node->Data;
}


void Edit(int index, void* data) {

	// 예외 처리
	if (index < 0 || index >= Count || Count < 1) {
		return; // 예외 처리 방법 찾아보기
	}

	Node* currentNode = GetNode(index);
	currentNode->Data = data;
}

Node* GetNode(int index) {

	// 예외 처리, Count가 0일 때 추가
	if (index < 0 || index >= Count || Count < 1) {
		return NULL; // 예외 처리 방법 찾아보기
	}

	// 리스트 전체 길이의 절반
	int temp = index / 2;
	// 0 : 앞에서부터 탐색, 1 : 뒤에서부터 탐색
	int isFirst = 0;

	// 인덱스가 리스트의 절반 길이보다 작으면 앞에서 탐색, 크면 뒤에서 탐색
	if (index <= temp) {
		isFirst = 0;
	}
	else {
		isFirst = 1;
	}

	// 앞에서부터 탐색
	if (isFirst == 0) {

		Node* currentNode = head;

		int i = 0;

		while (i < Count) {
			if (i == index) {
				break;
			}
			currentNode = currentNode->Next;
			i++;
		}
		return currentNode;
	}
	// 뒤애서부터 탐색
	else if (isFirst == 1) {

		Node* currentNode = last;

		int i = Count - 1;

		while (i >= 0) {
			if (i == index)
			{
				break;
			}
			currentNode = currentNode->Prev;
			i--;
		}
		return currentNode;
	}
}

int main() {

	int a = 10, b = 20, c = 30;

	// 값 추가
	Add(&a);
	Add(&b);
	Add(&c);

	// 추가된 값 출력
	printf("LinkedList 출력:\n");
	for (int i = 0; i < Count; i++) {
		int* val = (int*)Get(i);
		printf("Index %d: %d\n", i, *val);
	}

	// 값 수정
	int d = 100;
	Edit(1, &d);

	printf("\n수정 후 LinkedList 출력:\n");
	for (int i = 0; i < Count; i++) {
		int* val = (int*)Get(i);
		printf("Index %d: %d\n", i, *val);
	}

	// 노드 삭제
	Delete(0);

	printf("\n삭제 후 LinkedList 출력:\n");
	for (int i = 0; i < Count; i++) {
		int* val = (int*)Get(i);
		printf("Index %d: %d\n", i, *val);
	}

	return 0;
}
```

```C
출력

LinkedList 출력:
Index 0: 10
Index 1: 20
Index 2: 30

수정 후 LinkedList 출력:
Index 0: 10
Index 1: 100
Index 2: 30

삭제 후 LinkedList 출력:
Index 0: 100
Index 1: 30
```
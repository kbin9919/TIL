# 이중 원형 연결리스트 C

# 트러블 슈팅 내역
* malloc함수를 사용하려면 반드시 <stdlib.h>를 #include해야 한다.
* free함수를 사용할 땐, 현재 위치에서 메모리를 해제해도 이후 동작에 문제가 없는지 확인해야 한다.

# 구현
```C
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
	void* Data;
	struct Node* Next;
	struct Node* Prev;
} Node;

typedef struct LinkedList {

	Node* firstNode;
	Node* lastNode;
	int Count;
} LinkedList;

LinkedList CreateLinkedList() {
	LinkedList list;
	list.Count = 0;
	list.firstNode = NULL;
	list.lastNode = NULL;
	return list;
}

Node* GetNode(LinkedList* list, int index) {

	// 예외 처리, list가 비어있는 경우 추가할 것.
	if (index < 0 || index >= list->Count) {
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

		Node* currentNode = list->firstNode;

		for (int i = 0; i < list->Count; i++) {
			if (i == index) {
				break;
			}
			currentNode = currentNode->Next;
		}

		return currentNode;
	}
	// 뒤애서부터 탐색
	else if (isFirst == 1) {

		Node* currentNode = list->lastNode;

		for (int i = list->Count - 1; i >= 0; i--)
		{
			if (i == index)
			{
				break;
			}
			currentNode = currentNode->Prev;
		}

		return currentNode;
	}
}


void Add(LinkedList* list, void* data) {

	// 예외 처리, list가 비어있는 경우 추가할 것.

	// 신규 노드 생성
	Node* node = (Node*)malloc(sizeof(Node));
	if (node == NULL) {
		// 메모리 할당 실패 처리
		return;
	}
	node->Data = data;

	// 첫 노드인 경우
	if (list->firstNode == NULL) {
		list->firstNode = node;
		list->lastNode = node;

		list->firstNode->Next = list->lastNode;
		list->firstNode->Prev = list->lastNode;

		list->lastNode->Next = list->firstNode;
		list->lastNode->Prev = list->firstNode;

		list->Count++;
		return;
	}

	// 추가
	list->lastNode->Next = node;
	node->Next = list->firstNode;

	// 새로운 노드의 이전 노드를 마지막 노드로 추가
	node->Prev = list->lastNode;
	// 처음 노드의 이전 노드를 마지막 노드로 업데이트
	list->firstNode->Prev = node;
	// 새로운 노드를 마지막 노드로 업데이트
	list->lastNode = node;

	list->Count++;
}

void AddIndex(LinkedList* list, int index, void* data) {

	// 예외 처리, list가 비어있는 경우 추가할 것.
	if (index < 0 || index > list->Count) {
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
	if (list->Count == 0) {
		list->firstNode = node;
		list->lastNode = node;
	}

	// 탐색
	Node* currentNode = GetNode(list, index);
	Node* beforeNode = currentNode->Prev;

	// 추가
	beforeNode->Next = node;
	currentNode->Prev = node;

	node->Next = currentNode;
	node->Prev = beforeNode;

	list->Count++;

	if (index == 0) {
		// 노드를 추가한 인덱스가 0인 경우 FirstNode, LastNode를 업데이트
		list->firstNode = node;
		list->lastNode = node;
	}
	if (index == list->Count) {
		// 노드를 추가한 인덱스가 리스트의 크기랑 같은 경우 LastNode를 업데이트
		list->lastNode = node;
	}
}

void Delete(LinkedList* list, int index) {

	// 예외 처리, list가 비어있는 경우 추가할 것.
	if (index < 0 || index > list->Count) {
		return; // 예외 처리 방법 찾아보기
	}

	// 탐색
	Node* currentNode = GetNode(list, index);
	Node* beforeNode = currentNode->Prev;

	// 삭제
	beforeNode->Next = currentNode->Next;
	currentNode->Next->Prev = beforeNode;

	list->Count--;

	if (index == 0)
	{
		// 노드를 삭제한 인덱스가 0인 경우 FirstNode를 업데이트
		list->firstNode = list->firstNode->Next;
	}
	if (index == list->Count)
	{
		// 마지막 노드가 삭제된 노드일 경우 lastNode 업데이트
		list->lastNode = beforeNode;
	}
	free(currentNode);
}

void* Get(LinkedList* list, int index) {

	// 예외 처리, list가 비어있는 경우 추가할 것.
	if (index < 0 || index >= list->Count) {
		return; // 예외 처리 방법 찾아보기
	}

	Node* node = GetNode(list, index);
	return node->Data;
}


void Edit(LinkedList* list, int index, void* data) {

	// 예외 처리, list가 비어있는 경우 추가할 것.
	if (index < 0 || index >= list->Count) {
		return; // 예외 처리 방법 찾아보기
	}

	Node* currentNode = GetNode(list, index);
	currentNode->Data = data;
}

int main() {
	LinkedList list = CreateLinkedList();

	int a = 10, b = 20, c = 30;

	// 값 추가
	Add(&list, &a);
	Add(&list, &b);
	Add(&list, &c);

	// 추가된 값 출력
	printf("LinkedList 출력:\n");
	for (int i = 0; i < list.Count; i++) {
		int* val = (int*)Get(&list, i);
		printf("Index %d: %d\n", i, *val);
	}

	// 값 수정
	int d = 100;
	Edit(&list, 1, &d);

	printf("\n수정 후 LinkedList 출력:\n");
	for (int i = 0; i < list.Count; i++) {
		int* val = (int*)Get(&list, i);
		printf("Index %d: %d\n", i, *val);
	}

	// 노드 삭제
	Delete(&list, 0);

	printf("\n삭제 후 LinkedList 출력:\n");
	for (int i = 0; i < list.Count; i++) {
		int* val = (int*)Get(&list, i);
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
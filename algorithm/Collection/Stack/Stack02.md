# 스택 기능 구현
1. Node를 활용한 Stack구현
2. Push, Pop, Peek 기능 구현
3. 테스트 로직 작성 및 수행

## 내 생각
* 스택은 LIFO 구조라서 항상 '마지막에 들어온 요소'만 접근하게 됨으로 Stack구조체 안에서 마지막 노드의 주소만 가지고 있으면 된다고 생각했고,
이를 기반으로 구현 스택을 구현.

```C#
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
	void* Data;
	struct Node* Prev;
} Node;

typedef struct Stack {
	Node* lastNode;
	int Count;
} Stack;

Stack CreateStack() {
	Stack stack;
	stack.Count = 0;
	stack.lastNode = NULL;
	return stack;
}

void Push(Stack* stack, void* data) {
	// 신규 노드 생성
	Node* node = (Node*)malloc(sizeof(Node));
	if (node == NULL) {
		// 메모리 할당 실패 처리
		return;
	}

	node->Data = data;

	// 첫 노드가 아닌 경우 이전 노드 연결
	if (stack->lastNode != NULL) {
		node->Prev = stack->lastNode;
	}

	stack->lastNode = node;
	stack->Count++;
}

Node* Pop(Stack* stack) {
    // 스택이 비어있는 경우
	if (stack->lastNode == NULL) {
		return NULL; 
	}
	Node* node = stack->lastNode;

	// 이전 노드가 존재하지 않을 경우 스택을 비움
	if (stack->lastNode->Prev == NULL) {
		stack->lastNode = NULL;
	}
	// 이전 노드가 존재할 경우 마지막 노드를 이전 노드로 업데이트
	else if (stack->lastNode->Prev != NULL) {
		stack->lastNode = stack->lastNode->Prev;
	}

	stack->Count--;

	return node;
}

Node* Peek(Stack* stack) {
    // 스택이 비어있는 경우
	if (stack->lastNode == NULL) {
		return NULL; 
	}
	Node* node = stack->lastNode;

	return node;
}

int main() {
	Stack stack = CreateStack();

	// 테스트용 정수
	int a = 10, b = 20, c = 30;

	printf("=== Push 테스트 ===\n");
	Push(&stack, &a);
	Push(&stack, &b);
	Push(&stack, &c);

	printf("현재 스택 카운트: %d\n", stack.Count);
	printf("현재 top 데이터: %d\n", *(int*)Peek(&stack)->Data);

	printf("\n=== Pop 테스트 ===\n");
	Node* popped = Pop(&stack);
	printf("Pop된 데이터: %d\n", *(int*)popped->Data);
	free(popped); // Pop으로 반환된 노드 메모리 해제

	printf("현재 스택 카운트: %d\n", stack.Count);
	printf("현재 top 데이터: %d\n", *(int*)Peek(&stack)->Data);

	printf("\n=== 나머지 Pop ===\n");
	while (stack.Count > 0) {
		Node* n = Pop(&stack);
		printf("Pop: %d\n", *(int*)n->Data);
		free(n);
	}

	printf("모든 데이터 Pop 완료\n");
	printf("현재 스택 카운트: %d\n", stack.Count);

	return 0;
}
```

```C#
출력

=== Push 테스트 ===
현재 스택 카운트: 3
현재 top 데이터: 30

=== Pop 테스트 ===
Pop된 데이터: 30
현재 스택 카운트: 2
현재 top 데이터: 20

=== 나머지 Pop ===
Pop: 20
Pop: 10
모든 데이터 Pop 완료
현재 스택 카운트: 0
```
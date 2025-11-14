### 선형 큐 (Linear Queue) 구현하기

1. 구조체 형태로 구현하기
2. 연결리스트를 활용하기

```C#
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
   void* Data;
   struct Node* Next;
   struct Node* Prev;
} Node;

typedef struct Queue {
   Node* firstNode;
   Node* lastNode;
   int Count;
} Queue;

Queue CreateQueue() {
   Queue queue;
   queue.Count = 0;
   queue.firstNode = NULL;
   queue.lastNode = NULL;

   return queue;
}

void Enqueue(Queue* queue, void* data) {

   // 신규 노드 생성
   Node* node = (Node*)malloc(sizeof(Node));
   if (node == NULL) {
      // 메모리 할당 실패 처리
      return;
   }

   node->Data = data;

   // 첫 노드인 경우
   if (queue->Count == 0) {
      queue->firstNode = node;
      queue->lastNode = node;

      queue->firstNode->Next = queue->lastNode;
      queue->firstNode->Prev = queue->lastNode;

      queue->lastNode->Next = queue->firstNode;
      queue->lastNode->Prev = queue->firstNode;

      queue->Count++;
      return;
   }

   // 기존 LastNode의 Next를 신규 노드로 추가, 신규 노드의 Next를 FirstNode로 추가
   queue->lastNode->Next = node;
   node->Prev = queue->lastNode;
   // 새로운 노드를 마지막 노드로 업데이트
   queue->lastNode = node;

   queue->Count++;
}


void* Dequeue(Queue* queue) {
   if (queue->Count == 0)
   {
      // 큐가 비어있는 경우 예외처리
      return NULL;
   }

   // 가장 먼저 추가된 노드 발췌
   Node* firstNode = queue->firstNode;

      // 첫 노드가 마지막 노드인 경우(노드가 1개인 경우)
      if (queue->Count == 1) {
         queue->firstNode = NULL;
         queue->lastNode = NULL;
         queue->Count--;
         return firstNode->Data;
      }

   // 첫 노드의 다음 노드를 첫 노드로 업데이트, 변경된 첫 노드의 이전 노드를 NULL로 설정 
   Node* SecontNode = firstNode->Next;

   SecontNode->Prev = NULL;
   queue->firstNode = SecontNode;
   queue->Count--;

   // 값 반환
   return firstNode->Data;
}
void* Peek(Queue* queue) {

   // 가장 먼저 추가된 노드 발췌
   Node* firstNode = queue->firstNode;

   // 값 반환
   return firstNode->Data;
}



int main() {
   Queue queue = CreateQueue();

   int a = 10, b = 20, c = 30;

   // 값 추가
   Enqueue(&queue, &a);
   Enqueue(&queue, &b);
   Enqueue(&queue, &c);

   // 추가된 값 출력
   printf("LinkedList 출력:\n");

   int cnt = queue.Count;

   for (int i = 0; i < cnt; i++) {
      int* val = (int*)Peek(&queue);
      printf("Peek %d: %d\n", i, *val);
      int* val2 = (int*)Dequeue(&queue);
      printf("Dequeue %d: %d\n", i, *val);
   }

   return 0;
}
```
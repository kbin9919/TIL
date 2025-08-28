### 원형 연결리스트(환형 연결리스트)

원형 연결리스트는 **마지막 노드가 다시 첫 번째 노드를 가리키도록 연결된 연결리스트**이다.

* **단순 연결리스트**는 마지막 노드의 포인터가 `NULL`을 가리킨다.
* 하지만 **원형 연결리스트**에서는 마지막 노드의 포인터가 첫 번째 노드를 가리킨다.
* 따라서 리스트 전체가 \*\*원 모양(순환 구조)\*\*으로 이어져 있다.

---

### 종류

1. **단순 원형 연결리스트**

   * 각 노드가 **다음 노드**만 가리킨다.
   * 마지막 노드가 첫 번째 노드를 가리켜 원형을 이룬다.

2. **이중 원형 연결리스트**

   * 각 노드가 **이전 노드와 다음 노드**를 모두 가리킨다.
   * 양방향 탐색이 가능하고, 마지막 노드와 첫 번째 노드가 서로 연결된다.

---

### 특징

* **장점**

  1. 리스트의 처음과 끝이 연결되어 있어, 어느 노드에서 시작해도 전체 순회가 가능하다.
  2. 큐(특히 원형 큐)나 라운드 로빈 방식 스케줄링 같은 곳에서 활용된다.
  3. 삽입/삭제 시, 노드 개수와 상관없이 일정한 방식으로 처리할 수 있다.

* **단점**

  1. 종료 조건을 명확히 처리하지 않으면 무한 루프에 빠질 수 있다.
  2. 구현 시 단순 연결리스트보다 복잡하다.

---

한 문장으로 정리하면,
**원형 연결리스트는 마지막 노드가 첫 번째 노드를 가리켜, 리스트 전체가 원처럼 연결된 자료구조**이다.
```C#
using System;

class Node
{
    public object Data;
    public Node Next;
    public Node(Object data)
    {
        this.Data = data;
    }
    public Node()
    {
    }
}

class LinkedList
{
    Node[] nodes;
    int maxSize = 1;
    int currentIndex = 0;
    public LinkedList()
    {
        nodes = new Node[maxSize];
    }

    public void Add(Object data)
    {
        Node[] newNodes = new Node[maxSize + 1];

        for (int i = 0; i < currentIndex; i++)
        {
            newNodes[i] = nodes[i];
        }

        Node node = new Node(data);
        node.Next = nodes[0];

        newNodes[currentIndex] = node;

        nodes = newNodes;

        if (currentIndex != 0)
        {
            var beforeNode = nodes[currentIndex - 1];
            beforeNode.Next = node;
        }
        currentIndex++;
        maxSize++;
    }
    public Object Get(int index)
    {
        return nodes[index].Data;
    }
    public Object NextData(int index)
    {
        return nodes[index].Next.Data;
    }
    public int Size
    {
        get { return currentIndex; }
    }
}

public class Test
{
    static void Main(string[] args)
    {
        LinkedList list = new LinkedList();
        list.Add(1);
        list.Add(2);
        list.Add(3);
        list.Add(4);
        list.Add(5);

        for (int i = 0; i < list.Size; i++)
        {
            Console.WriteLine("현재 노드 데이터 : " + list.Get(i));
            Console.WriteLine("다음 노드 데이터 : " + list.NextData(i));
        }
    }
}
```

이중 환형 연결리스트는 각 노드가 **이전 노드와 다음 노드에 대한 참조를 모두 가지고 있으며, 마지막 노드와 첫 번째 노드가 서로 연결되어 원형을 이루는 구조**이다.

---

### 특징

1. **양방향 탐색 가능**

   * 단일 연결리스트와 달리 이전 노드와 다음 노드로 이동할 수 있어 탐색이 자유롭다.

2. **원형 구조 유지**

   * 마지막 노드의 `next`가 첫 노드를 가리키고, 첫 노드의 `prev`가 마지막 노드를 가리킨다.

3. **어느 노드에서든 시작 가능**

   * 원형이므로 특정 노드를 알고 있다면 그 노드를 시작점으로 리스트 전체를 순회할 수 있다.

---

### 장점

1. **앞뒤 양쪽에서 삽입/삭제가 용이하다.**

   * 단일 원형 리스트는 이전 노드를 알아야 삭제할 수 있지만, 이중 환형은 `prev`가 있어 삭제가 빠르다.

2. **탐색 방향 선택이 가능하다.**

   * 원하는 경우 앞으로 혹은 뒤로 순회할 수 있어 특정 상황에서 탐색 효율이 좋아진다.

3. **큐(Queue)나 덱(Deque) 같은 자료구조 구현에 적합하다.**

   * 양방향에서 삽입/삭제가 가능하고, 원형이므로 인덱스 초과 문제 없이 순환 구조를 지원한다.

---

### 단점

1. **메모리 사용량이 증가한다.**

   * 각 노드가 `prev`와 `next` 두 개의 참조를 저장해야 하므로 단일 리스트보다 메모리를 더 사용한다.

2. **구현이 복잡하다.**

   * 삽입과 삭제 시 `prev`와 `next`를 모두 신경 써야 하므로 코드가 복잡해지고, 버그 발생 가능성이 높다.

3. **원형 구조로 인해 무한 루프 위험이 있다.**

   * 잘못된 조건으로 탐색하면 끝을 알 수 없어 무한히 순회할 수 있다.

---

# 요약
**이중 환형 연결리스트는 양방향 탐색과 삽입/삭제에 강점을 가지지만, 메모리 사용이 늘어나고 구현이 복잡하다는 단점이 있는 자료구조이다.**.

# 구현
```C#
class Node
{
    public object Data;
    public Node Next;
    public Node Prev;

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
    private Node firstNode;
    private Node lastNode;
    public int Count = 0;

    public void Add(Object data)
    {
        var node = new Node(data);

        // 탐색
        if (firstNode == null)
        {
            firstNode = node;
            lastNode = node;

            firstNode.Next = lastNode;
            firstNode.Prev = lastNode;

            Count++;
            return;
        }

        // 추가
        lastNode.Next = node;
        node.Next = firstNode;

        // 새로운 노드의 이전 노드를 마지막 노드로 추가
        node.Prev = lastNode;
        // 처음 노드의 이전 노드를 마지막 노드로 업데이트
        firstNode.Prev = node;

        // 새로운 노드를 마지막 노드로 업데이트
        lastNode = node;

        Count++;
    }

    public void Add(Object data, int index)
    {
        // 예외 발생
        if (index < 0 || index > Count)
        {
            throw new IndexOutOfRangeException();
        }
        var node = new Node(data);

        if (Count == 0)
        {
            firstNode = node;
            lastNode = node;
        }

        // 탐색
        var currentNode = firstNode;
        var beforeNode = lastNode;

        for (int i = 0; i < Count; i++)
        {
            if (i == index)
            {
                break;
            }

            beforeNode = currentNode;
            currentNode = currentNode.Next;
        }

        // 추가
        beforeNode.Next = node;
        currentNode.Prev = node;

        node.Next = currentNode;
        node.Prev = beforeNode;

        if (index == 0)
        {
            // 노드를 추가한 인덱스가 0인 경우 FirstNode를 업데이트
            firstNode = node;
        }
        if (index == Count)
        {
            // 노드를 추가한 인덱스가 리스트의 크기랑 같은 경우 LastNode를 업데이트
            lastNode = node;
        }
        Count++;
    }

    public void Delete(int index)
    {
        // 예외 발생
        if (index < 0 || index >= Count)
        {
            throw new IndexOutOfRangeException();
        }

        // 탐색
        var currentNode = firstNode;
        var beforeNode = lastNode;

        for (int i = 0; i < Count; i++)
        {
            if (i == index)
            {
                break;
            }
            beforeNode = currentNode;
            currentNode = currentNode.Next;
        }

        // 삭제
        if (currentNode.Next != null)
        {
            beforeNode.Next = currentNode.Next;
            currentNode.Next.Prev = beforeNode;
        }
        Count--;

        if (index == 0)
        {
            // 노드를 삭제한 인덱스가 0인 경우 FirstNode를 업데이트
            firstNode = firstNode.Next;
        }
        if (index == Count)
        {
            // 마지막 노드가 삭제된 노드일 경우 lastNode 업데이트
            lastNode = beforeNode;
        }
    }

    public object Get(int index)
    {
        // 예외 발생
        if (index < 0 || index >= Count)
        {
            throw new IndexOutOfRangeException();
        }

        var currentNode = firstNode;

        for (int i = 0; i < Count; i++)
        {

            if (i == index)
            {
                break;
            }
            currentNode = currentNode.Next;
        }
        return currentNode.Data;
    }

    public void Edit(Object data, int index)
    {
        // 예외 발생
        if (index < 0 || index >= Count)
        {
            throw new IndexOutOfRangeException();
        }

        var currentNode = firstNode;

        for (int i = 0; i < Count; i++)
        {
            if (i == index)
            {
                break;
            }
            currentNode = currentNode.Next;
        }

        currentNode.Data = data;
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
        list.Add(5, 0);
        list.Add(10, 2);
        list.Add(15, 3);
        list.Delete(1);
        list.Edit(20, 3);
        list.Delete(0);
        list.Delete(4);


        for (int i = 0; i < list.Count; i++)
        {
            var data = list.Get(i);
            Console.WriteLine("현재 노드 데이터 : " + data);
        }

    }
}
```

---
# 개선사항
* 이중 연결리스트의 장점인 양방향 탐색을 활용하기
* index없는 기본 Add 리펙토링하기
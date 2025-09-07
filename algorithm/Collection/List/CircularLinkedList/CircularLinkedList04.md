# 원형 연결리스트 개선

* 개선사항
1. 중복 처리 최소화
이전 구조에서는 index == 0일 때, Count == 0일 때, index == Count일 때 등 여러 분기문이 반복된다.
'삽입/삭제 로직을 일반화'해서 특수 케이스를 최소화하는것이 목표
ex) 맨 앞, 맨 뒤, 중간 모두 같은 루틴으로 처리

Add
* 맨 앞에서 Add할 때
1. 기존에 firstNode가 있을 때
- 첫 번째 노드를 신규 노드의 Next로 붙힌다.
- 필드의 firstNode를 신규 노드로 변경한다.
- 마지막 노드의 다음 노드를 신규 노드로 변경한다.

2. 기존에 firstNode가 없을 때
- 첫 번째 노드를 신규 노드의 Next로 붙힌다.
- 필드의 firstNode를 신규 노드로 변경한다.
- 마지막 노드의 다음 노드를 신규 노드로 변경한다.
- 마지막 노드를 신규 노드로 변경한다.

* 중간에 Add할 때
**index에 해당하는 노드와, 그 노드의 전 노드를 탐색한다.**
- 이전노드의 다음 노드를 신규 노드로 변경한다.
- 신규 노드의 다음 노드를 index에 해당하는 노드로 변경한다.
- index와 Count가 동일하면 마지막 노드를 신규 노드로 변경한다.

* 공통화시 중간에 Add하는 로직에서 추가해야 하는 것
- 필드의 firstNode를 신규 노드로 변경한다.
- firstNode가 없을 때에 대한 처리

# 개선사항 적용된 원형 연결리스트
```C#
class Node
{
    public object Data;
    public Node Next;
    public Node Bofore;

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

        if (firstNode == null)
        {
            firstNode = node;
            lastNode = node;

            firstNode.Next = lastNode;
            Count++;
            return;
        }

        // 새로운 노드를 마지막 노드의 다음 노드로 추가
        lastNode.Next = node;
        // 새로운 노드를 마지막 노드로 업데이트
        lastNode = node;
        // 처음노드를 마지막 노드의 다음노드로 추가
        node.Next = firstNode;

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

        beforeNode.Next = node;
        node.Next = currentNode;

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
    ^
    public void Delete(int index)
    {
        // 예외 발생
        if (index < 0 || index >= Count)
        {
            throw new IndexOutOfRangeException();
        }

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

        if (currentNode.Next != null)
        {
            beforeNode.Next = currentNode.Next;
        }

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
        Count--;
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
        //list.Add(5, 4);
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
```

```C#
출력

현재 노드 데이터 : 10
현재 노드 데이터 : 15
현재 노드 데이터 : 20
현재 노드 데이터 : 3
```

# 각 스텝 별 list 데이터

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

* 1
* 1 -> 2
* 1 -> 2 -> 3
* 1 -> 2 -> 3 -> 4
* 5 -> 1 -> 2 -> 3-> 4
* 5 -> 1 -> 10 -> 2 -> 3-> 4
* 5 -> 1 -> 10 -> 15 -> 2 -> 3-> 4
* 5 -> 10 -> 15 -> 2 -> 3-> 4
* 5 -> 10 -> 15 -> 20 -> 3-> 4
* 10 -> 15 -> 20 -> 3-> 4
* 10 -> 15 -> 20 -> 3


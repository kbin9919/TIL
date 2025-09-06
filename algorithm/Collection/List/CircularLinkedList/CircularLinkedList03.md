# 원형 연결리스트 기능 구현
1. 마지막 Node 추가
2. 특정 index에 Node 추가
3. 특정 index에 Delete 추가
4. 특정 index에 수정 추가

# 수정사항
1. 예외처리(인덱스가 리스트의 길이보다 클 때) 를 위한 index기반에서 Size기반으로 변경
2. LinkedList 구현 중 개선사항이었던, **Add(Object data, int index) 메서드만 i = 1로 시작하게 되어있음**부분 beforeNode 추가로 개선

```
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

        if (index == 0)
        {
            node.Next = firstNode;
            firstNode = node;
            lastNode.Next = node;

            // 첫 노드가 없을 때 마지막 노드를 추가된 노드로 설정
            if (index == Count)
            {
                lastNode = node;
            }
            Count++;
            return;
        }

        var currentNode = firstNode;
        var beforeNode = firstNode;

        // LinkedList 구현 중 개선사항 적용 부분
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

        // 마지막 노드가 추가된 노드일 경우 lastNode 업데이트
        if (index == Count)
        {
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

        if (index == 0)
        {
            firstNode = firstNode.Next;
            lastNode.Next = firstNode;
            Count--;
            return;
        }

        var currentNode = firstNode;
        var beforeNode = firstNode;

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

        currentNode = null;
        if (index == Count)
        {
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

* 1 **-> 1**
* 1 -> 2 **-> 1...**
* 1 -> 2 -> 3 **-> 1...**
* 1 -> 2 -> 3 -> 4 **-> 1...**
* 5 -> 1 -> 2 -> 3-> 4 **-> 5...**
* 5 -> 1 -> 10 -> 2 -> 3-> 4 **-> 5...**
* 5 -> 1 -> 10 -> 15 -> 2 -> 3-> 4 **-> 5...**
* 5 -> 10 -> 15 -> 2 -> 3-> 4 **-> 5...**
* 5 -> 10 -> 15 -> 20 -> 3-> 4 **-> 5...**
* 10 -> 15 -> 20 -> 3-> 4 **-> 10...**
* 10 -> 15 -> 20 -> 3 **-> 10...**



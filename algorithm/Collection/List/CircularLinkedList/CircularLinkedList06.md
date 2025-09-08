# 이중 원형 연결리스트 개선

* 이중 원형 연결리스트는 앞뒤 양방향 순회가 가능해서 원하는 노드까지 짧은 경로로 이동할 수 있다.
* 원형 구조 덕분에 끝에서 다시 처음으로 넘어가며 효율적인 최단 경로 탐색이 가능하다.

# 개선사항

* 탐색로직 리펙토링
* 테스트 케이스 추가

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

        var currentNode = GetNode(index);
        var beforeNode = currentNode.Prev;

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
        var currentNode = GetNode(index);
        var beforeNode = currentNode.Prev;

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

        return GetNode(index).Data;
    }

    public void Edit(Object data, int index)
    {
        // 예외 발생
        if (index < 0 || index >= Count)
        {
            throw new IndexOutOfRangeException();
        }

        GetNode(index).Data = data;
    }

    public Node GetNode(int index)
    {
        // 예외 발생
        if (index < 0 || index >= Count)
        {
            throw new IndexOutOfRangeException();
        }

        int temp = Count / 2;
        bool isFirstStart = temp >= index ? true : false;

        if (isFirstStart)
        {
            var currentNode = firstNode;
            for (int i = 0; i < Count; i++)
            {
                if (i == index)
                {
                    break;
                }
                currentNode = currentNode.Next;
            }
            return currentNode;
        }
        else
        {
            var currentNode = lastNode;
            for (int i = Count - 1; i >= 0; i--)
            {
                if (i == index)
                {
                    break;
                }
                currentNode = currentNode.Prev;
            }
            return currentNode;
        }
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






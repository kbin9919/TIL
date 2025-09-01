### 원형 연결리스트 수정(Node를 배열로 관리하던 구조에서 NextNode 중점으로 수정) 

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
    private Node firstNode;
    private Node boforeNode;
    
    public LinkedList()
    {
        boforeNode = new Node();
    }

    public void Add(Object data)
    {
        var node = new Node(data);

        if (firstNode == null)
        {
            firstNode = node;
        }

        boforeNode.Next = node;
        boforeNode = node;

        node.Next = firstNode;
    }

    public object Get(int index)
    {
        var currentNode = firstNode;

        for (int i = 0; i < index; i ++)
        {
            currentNode = currentNode.Next;
        }

        return currentNode.Data;
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

        for (int i = 0; i < 6; i++)
        {
            Console.WriteLine("현재 노드 데이터 : " + list.Get(i));
        }

    }
}
```

```C#
출력

현재 노드 데이터 : 1
현재 노드 데이터 : 2
현재 노드 데이터 : 3
현재 노드 데이터 : 4
현재 노드 데이터 : 5
다음 노드 데이터 : 1
```

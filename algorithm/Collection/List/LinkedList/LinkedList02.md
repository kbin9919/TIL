### 연결 리스트 구현하기

# 연결 리스트에서 데이터를 `add`할 때는 다음과 같은 형태로 저장된다.
* 연결 리스트는 \*\*노드(Node)\*\*들의 집합이고, 각 노드는 두 가지 정보를 가지고 있다.
  1. **데이터(Data)**: 저장하고자 하는 값 (예: 숫자, 문자열 등)
  2. **포인터(또는 참조, Reference)**: 다음 노드를 가리키는 정보

* 새로운 데이터를 추가(add)하면, 새로운 노드를 만들어서 그 노드에 데이터를 저장하고, 기존 노드들의 포인터를 적절히 조정해서 **노드들 간의 연결 구조**를 유지한다.
* 예를 들어 단일 연결 리스트에서 `add`를 끝에 하면, 새 노드를 생성해서 현재 마지막 노드의 포인터가 새 노드를 가리키도록 변경한다.
* 반대로 앞에 추가하면, 새 노드의 포인터가 기존 첫 번째 노드를 가리키고, 연결 리스트의 시작점(head)을 새 노드로 바꾼다.
* 그래서 연결 리스트는 배열처럼 연속된 메모리에 저장되지 않고, **노드들이 포인터로 연결된 구조**로 저장되는 게 핵심이다.


# 연결 리스트에서 \*\*가장 앞에 오는 노드(= 첫 번째 노드, head 노드)\*\*는 **반드시 알고 있어야 한다**

* 연결 리스트는 **각 노드가 다음 노드를 가리키는 방식으로만 연결**되어 있어서, 중간이나 끝에 있는 노드를 바로 접근할 수 없다.
* 그래서 어떤 작업이든(검색, 추가, 삭제 등)을 하려면 **무조건 처음부터 순차적으로 따라가야** 한다.
* 그런데 그 "처음"을 모르고 있으면 아무 작업도 할 수 없다. 결국 연결 리스트는 \*\*시작점(=head)\*\*만 알고 있어도 전체 구조를 따라가면서 조작할 수 있는 것이다.

즉, 연결 리스트에서 `head`는 **리스트 자체를 대표하는 핵심 포인터**라고 보면 된다. `head`를 잃어버리면 리스트 전체에 접근할 수 없다.

# 연결리스트 구현
```C#
using System;
using System.Runtime.ConstrainedExecution;

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

    public LinkedList() { }

    public void Add(Object data)
    {
        var node = new Node(data);

        if (firstNode == null)
        {
            firstNode = node;
            return;
        }

        Node LastNode = firstNode;

        while (LastNode.Next != null)
        {
            LastNode = LastNode.Next;
        }

        LastNode.Next = node;
    }

    public void Add(Object data, int index)
    {
        var node = new Node(data);

        var currentNode = firstNode;
        for (int i = 0; i < index; i++)
        {
            currentNode = currentNode.Next;
        }

        node.Next = currentNode.Next;   
        currentNode.Next = node;
    }
    public void Delete(int index)
    {
        var currentNode = firstNode;
        var beforeNode = firstNode;
        for (int i = 0; i < index; i++)
        {
            if(i == index - 1)
            {
                beforeNode = currentNode;
            }
            currentNode = currentNode.Next;
        }

        if (currentNode.Next != null)
        {
            beforeNode.Next = currentNode.Next;
        }
        currentNode = null;
    }

    public object Get(int index)
    {
        var currentNode = firstNode;

        for (int i = 0; i < index; i++)
        {
            currentNode = currentNode.Next;
        }

        return currentNode.Data;
    }

    public void Edit(Object data, int index)
    {
        var currentNode = firstNode;

        for (int i = 0; i < index; i++)
        {
            currentNode = currentNode.Next;
        }

        currentNode.Data = data;
    }

    public void Clear()
    {
        firstNode = null;
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
        list.Add(7, 2);
        list.Delete(1);

        for (int i = 0; i < 5; i++)
        {
            var data = list.Get(i);
            Console.WriteLine("현재 노드 데이터 : " + data);
        }

    }
}
```

```C#
출력

현재 노드 데이터 : 1
현재 노드 데이터 : 3
현재 노드 데이터 : 7
현재 노드 데이터 : 4
현재 노드 데이터 : 5
```
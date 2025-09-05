# 수정사항
1. 예외처리(인덱스가 리스트의 길이보다 클 때) 를 위한 index기반에서 Size기반으로 변경

# 개선사항
* Add(Object data, int index) 메서드만 i = 1로 시작하게 되어있음
* index == 0 일때 처리 로직 리펙토링

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
    public int Count = 0;

    public void Add(Object data)
    {
        Count++;
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
        // 예외 발생
        if (index < 0 || index >= Count)
        {
            throw new IndexOutOfRangeException();
        }
        var node = new Node(data);

        // 리펙토링
        if (index == 0)
        {
            var temp = new Node
            {
                Data = firstNode.Data,
                Next = firstNode.Next
            };

            node.Next = temp;
            firstNode = node;
            Count++;
            return;
        }

        var currentNode = firstNode;

        // 수정할 부분 *지금 메서드만 i = 1로 시작하게 되어있음
        for (int i = 1; i < Count; i++)
        {
            if (i == index)
            {
                break;
            }
            currentNode = currentNode.Next;
        }

        Count++;
        node.Next = currentNode.Next;
        currentNode.Next = node;
    }

    public void Delete(int index)
    {
        // 예외 발생
        if (index < 0 || index >= Count)
        {
            throw new IndexOutOfRangeException();
        }
        
        // 리펙토링
        if (index == 0)
        {
            Count--;
            firstNode = firstNode.Next;
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


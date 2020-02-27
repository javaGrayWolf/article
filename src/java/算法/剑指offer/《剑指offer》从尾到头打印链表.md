# 《剑指offer》系列--从尾到头打印链表

>*越是没有本领的就越加自命不凡。——邓拓*

### 题目

输入个链表的头结点，从尾到头反过来打印出每个结点的值。

### 解析

```java
public class Test04 {
    static class Node{
        int data;
        Node next;
    }
    public static void printReverseListNode(Node node){
        Stack<Node> stack = new Stack<>();
        while (node != null){
            stack.push(node);
            node = node.next;
        }
        while (!stack.isEmpty()){
            System.out.println(stack.pop().data);
        }
    }

    public static void main(String[] args) {
        Node node = new Node();
        node.data = 1;
        node.next = new Node();
        node.next.data = 2;
        node.next.next = new Node();
        node.next.next.data = 3;
        node.next.next.next = new Node();
        node.next.next.next.data = 4;
        printReverseListNode(node);
    }
}
```

输出结果：

```java
4
3
2
1
```




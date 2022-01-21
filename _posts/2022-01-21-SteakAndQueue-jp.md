---
layout: post
categories: "DataStructure"
title: "Steak And Queue"
author: "TY_K"
---

### Steak

선형 자료구조의 일종으로 Last In First Out (LIFO). 즉, 나중에 들어간 데이터가 먼저 나온다. 

이것은 Stack 의 가장 큰 특징이다. 

차곡차곡 쌓이는 구조로 먼저 Stack 에 들어가게 된 데이터는 맨 바닥에 깔리게 된다. 

그렇기 때문에 늦게 들어간 데이터들은 그 위에 쌓이게 되고 호출 시 가장 위에 있는 데이터가 호출되는 구조이다.

```java
import java.util.ArrayList;

public class stack<T> {
    private ArrayList<T> stack = new ArrayList<T>();

    public void push(T item) {
        stack.add(item);
    }

    public T pop() {
        if(stack.isEmpty()) {
            return null;
        }
        return stack.remove(stack.size() -1);
    }

    public boolean isEmpty() {
        return stack.isEmpty();
    }

    public static void main(String[] args) {
        stack<Integer> ms = new stack<Integer>();
        ms.push(1);
        ms.push(2);
        System.out.println(ms.pop());
        ms.push(3);
        System.out.println(ms.pop());
        System.out.println(ms.pop());
    }

}
```

### 출력

```
2
3
1
```

### Queue

선형 자료구조의 일종으로 First In First Out (FIFO). 즉, 먼저 들어간 데이터가 먼저 나온다. 

Stack 과는 반대로 먼저 들어간 데이터가 맨 앞에서 대기하고 있다가 먼저 나오게 되는 구조이다. 

참고로 Java Collection 에서 Queue 는 인터페이스이다. 

이를 구현하고 있는 Priority queue등을 사용할 수 있다.

```java
import java.util.ArrayList;

public class queue<T> {
    private ArrayList<T> queue = new ArrayList<T>();

    public void enqueue(T item) {
        queue.add(item);
    }

    public T dequeue() {
        if (queue.isEmpty()) {
            return null;
        }
        return queue.remove(0);
    }

    public boolean isEmpty() {
        return queue.isEmpty();
    }

    public static void main(String []args){
        queue<Integer> mq = new queue<Integer>();
        mq.enqueue(1);
        mq.enqueue(2);
        mq.enqueue(3);
        System.out.println(mq.dequeue());
        System.out.println(mq.dequeue());
        System.out.println(mq.dequeue());

    }
}
```

### 출력

```
2
3
1
```

기본적으로 java.util 패키지에서 Steak와 Queue가 구현되어 있다.

[참고링크][DataStructure]

[DataStructure]: https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/DataStructure#stack-and-queue "DataStructure"
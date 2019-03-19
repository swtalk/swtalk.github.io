---
layout: post
title:  "Collection.list.Stack 클래스"
date:   2019-03-17 17:04:00
author: viewrain
categories: java
---

```$xslt
이 포스팅은 자바의 신 1,2권의 내용으로 포스팅 되었습니다. 
```
* List 인터페이스를 구현한 또하나의 클래스인 Stack 클래스를 포스팅 하고자 한다.  
* 이 클래스는 별로 사용하진 않지만, LIFO 방식의 기능 구현에선 간혹 필요할 수 있다. 
    * 단 LIFO 기능은 이미 빠른 ArrayDeque 라는 클래스가 존재하지만, thread safe 하지 않다는 단점이 있다. 

* List 인터페이스를 구현한 클래스들은 매우 많지만, 그중 ArrayList, Vector, Stack , LinkedList 를 가장 많이 사용한다.

## Stack
* Stack 클래스는 List 컬렉션 클래스의 Vector 클래스를 상속받아, 전형적인 스택 메모리 구조의 클래스를 제공한다. 
* 스택 메모리 구조는 선형 메모리 공간에 데이터를 저장하면서 LIFO의 시멘틱을 따르는 자료구조 이다.
  * 생성자 : Stack() <- 아무 데이터도 없는 Stack 객체를 만듬 
![image](/assets/Postingimg/stack.png)
* 출처 : TCPSCHOOL.com 
    

* Stack 의 상속 관계
* Java.lang.Object
    * ㄴJava.util.AbstractCollection <E>
        * ㄴjava.util.AbstractList <E> 
            * ㄴjava.util.Vector <E>
              * ㄴjava.util.Stack <E>

## Stack 의 메소드
* empty() : 객체가 비어있는지 확인한다. 
* peek() : 객체의 가장 맨 위에 있는 데이터를 리턴
* pop() : 객체의 가장 위에 있는 데이터를 지우고 리턴 
* push(E item) : 매개변수로 넘어온 데이터를 가장 위에 저장
* search (Object o) : 매개변수로 넘어온 데이터의 위치를 리턴

## 구현
{% highlight java %}
package com.toast.first.viewrain;

import java.util.Stack;

public class checkStack {
    public void checkStack() {
        Stack<Integer> intStack = new Stack<>();
        for (int loop = 0; loop < 5; loop++) {
            intStack.push(loop);  # for문을 이용하여, 데이터를 저장함
            System.out.println(intStack.peek());  # 호출시 peek 메소드를 이용해 가장 위에 있는 데이터를 리턴
        }
        while (!intStack.empty()){  #empty 메소드를 사용하여, 비어있는지 확인 
            System.out.println(intStack.pop());  # 가장 위에 있는 데이터를 지우면서 리턴한다. 
        }
    }
    public static void main(String[] args) {
        checkStack stack = new checkStack();
        stack.checkStack();
    }
}
{% endhighlight %}

### 결과
```concordion
0
1
2
3
4
4
3
2
1
0
```
* List 컬렉션 클래스에 대해 , 지난번엔 ArrayList 를 포스팅했고 이번엔 Stack 을 포스팅 했다. 
  * 뭐 List 컬랙션 클래스에 속하는 종류야 많지만.. (ArrayList, LinkedList, Vector, Stack) 
* 정리하면, Stack 클래스는 List 컬렉션 클래스의 Vector 클래스를 상속받은 클래스 이고 LIFO 시멘틱을 따르는 자료구조이다. 
* Got it??


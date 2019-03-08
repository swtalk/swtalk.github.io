---
layout: post
title:  "Collection! ArrayList 클래스"
date:   2019-03-08 21:04:00
author: viewrain
categories: java
---

```$xslt
이 포스팅은 자바의 신 1,2권의 내용으로 포스팅 되었습니다. 
```
* 배열에 `순서`가 있다. 
* Collection 인터페이스를 확장함
    * 그래서 몇몇 추가된 메소드를 제외하곤 Collection 에 선언된 메소드와 큰 차이가 없음

* List 인터페이스를 구현한 클래스들은 매우 많지만, 그중 ArrayList, Vector, Stack , LinkedList 를 가장 많이 사용함

## ArrayList, Vector (설명안함) 
* 크기 확장이 가능한 배열, 배열처럼 사용하지만 대괄호를 사용하지 않고 메소드를 통해 객체를 넣고 빼고, 조회한다. 
* ArrayList 와 Vector 클래스의 경우 사용법이 거의 동일하고, 기능도 비슷해 ArrayList 를 선호 함
    * Vector 는 JDK1.0 / ArrayList 는 JDK1.2 에 추가 됨 
    * 여러 수정 요청이 동시에 있을 경우 ArrayList 는 Thread Safe 하지 않음 

* ArrayList 의 상속 관계
* Java.lang.Object
    * ㄴJava.util.AbstractCollection <E>
        * ㄴjava.util.AbstractList <E>
            * ㄴjava.util.ArrayList <E>


### Arraylist 를 이용하여 배열 만들기 
{% highlight java %}
package com.toast.first.viewrain;

import java.util.ArrayList;

public class ListSample {
    public static void main(String[] args) {
        ListSample sample = new ListSample();
        sample.checkArrayList2();
    }
    public void checkArrayList1(){
        ArrayList<String> list1 = new ArrayList<String>();
        list1.add("ArrayListSample");
    }
    public void checkArrayList2(){
        ArrayList<String> list2 = new ArrayList<String>(100);
        list2.add("a");
        list2.add("b");
        list2.add("c");
        list2.add(4, "D");
        System.out.println(list2);
    }
}
{% endhighlight %}

* 위 경우 Exception in thread "main" java.lang.IndexOutOfBoundsException: Index: 4, Size: 3 에러가 발생한다. 
* 3은 어디있니..? 즉 배열의 순서를 맞추지 않으면 안된다. 

* ArrayList 는 add, get, remove 만 기억하면 된다 (나머지는 API 문서를 통해 필요할때마다 보면 되므로,) 

### 생성한 배열을 for 문을 이용해 출력

{% highlight java %}
public void checkArrayList3(){
    ArrayList<Integer> list3 = new ArrayList<Integer>();
    list3.add(5);
    list3.add(2);
    list3.add(4);
    list3.add(4);
    list3.add(3);
    list3.add(2);
    for (int i=0; i<list3.size(); i++){
        System.out.print(list3.get(i) + " ");
    }
    Collections.sort(list3);
    System.out.println(list3);
}
{% endhighlight %}

```
결과 : 5 2 4 4 3 2 [2, 2, 3, 4, 4, 5]
```
* for 문을 이용해 list3 의 size 보다 작을때까지 계속해서 index 를 출력하게끔 하였다. 
* 또한 Collectiions 클래스의 sort 를 이용하여, 정렬하였다. 


### 좀더 세련되게 만들어보자 
* iterator 를 이용해, 코드를 바꿔본다. 
    * iterator 란 모든 컬렉션 클래스에 데이터를 읽을때 사용한다. 
    * 표준화가 안되면, 모든 컬렉션 클래스의 데이터를 읽는 메소드를 일일이 알아야 하는 단점이 있다. 


{% highlight java %}
public void checkArrayList4(){
    ArrayList<Integer> list4 = new ArrayList<Integer>();
    list4.add(5);
    list4.add(2);
    list4.add(4);
    list4.add(4);
    list4.add(3);
    list4.add(2);
    Iterator<Integer> itr = list4.iterator();
    while (itr.hasNext()){
        System.out.print(itr.next() + " ");
    }
    Collections.sort(list4);
    System.out.println(list4);
}
{% endhighlight %}
* Iterator 타입에 변수를 생성하고 (itr) 컬렉션 마다 iterator 메소드를 값으로 넣는다. 
* hasNext() 메소드란 Iterator 의 데이터가 있을 경우 True, 없으면 False 를 리턴한다. 
    * 즉 배열의 길이를 확인 할 때 length 나 size 가 아닌 hasNext 로 판단 할 수 있다. 
* hasNext() + next() 를 이용해 데이터를 반환하는 것이 안전하다. 

---
layout: post
title:  "Collection Framework"
date:   2019-03-19 22:04:00
author: viewrain
categories: java
---
```concordion
자바의 신, 자바의 정석 , 웹블로그를 토대로 공부한 내용을 포스팅 하였습니다.
```
## Java Collection Framework 

* 자바가 제공하는 잘 정의된 객체의 그룹을 다루기 위한 Framework 의 이름이며, `java.util` 패키지 안에 있다. 
  * collection framework 를 구성하는 2개의 그룹중 최상위 레벨은 `Interface` 이며 하위는 클래스 이다.
  * `데이터 그룹을 다루고 표현하기 위한 단일화된 구조` - Java API문서
  
* 아무리 뒤져봐도, 아래 이미지 만하게 정리한 사이트가 없어서 퍼왔다.   
![image](/assets/Postingimg/collection_framework.jpg)
* collection framework 은 그림에서 처럼 2개의 그룹으로 구분된다.
  * java.util.Collection
  * java.util.Map
![image](/assets/nomad.JPG)
## 핵심 인터페이스 (Data Structure 관점)
* List
  * 순서 있는 데이터 집합
  * 데이터 중복 허용
* Set
  * 순서 없는 데이터 집합
  * 데이터 중복 불허
* Map
  * key value 쌍으로 이루어진 데이터 집합
  * 순서 없고, key 중복은 불허하지만 value 중복은 허용함    
```concordion
Vector나 Hashtable과 같은 기존의 컬렉션 클래스들은 호환을 위해,
설계를 변경해서 남겨두었지만 사용안하는 것이 좋다.
대신 새로 추가된 ArrayList와 HashMap 사용 권장.
```

## Interface 그룹
* 아래의 것들이 java.util.Collection의 구현체가 아닌 인터페이스들이다.
  * java.util.List : List 자료 구조 (ordered, sequential)
  * java.util.Set : Set 자료 구조 (unique element)
  * java.util.SortedSet : 정렬된 set
  * java.util.NavigableSet
  * java.util.Queue : Queue 자료 구조 (한쪽에서 삽입, 반대에서 추출)
  * java.util.Deque : Deque 자료 구조 (FIFO와 FILO 모두 지원)

* 다음은 java.util.Map 의 인터페이스 그룹들이다. 
  * java.util.SortedMap: key가 ascending order로 정렬된 map
  * java.util.NavigableMap

## 정리하며
* 너무 방대해서, 언젠가 한번에 모아 봐야겠단 생각은 했었다. 
* 이미지를 좋은걸 찾고, http://hochulshin.com 블로그 내용도 너무 좋아 정리하기 편했다. 
  * 각각의 클래스는 별도로 포스팅 하고 있지만.. 그래도 한곳에 모아 놓으니 보기 편해서 너무너무 좋은듯 하다. 
* Got it..

### Reference
* https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html
* https://www.quora.com/What-are-the-applications-of-collection-framework-in-Java
* http://norows.com/java-collections.html
* https://www.javatpoint.com/collections-in-java
* http://hochulshin.com/java-collection-framework/

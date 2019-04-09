---
layout: post
title:  "StringTokenizer & Split"
date:   2019-04-09 11:33:00
author: viewrain
categories: java
---

```$xslt
이 포스팅은 자바의 신 1,2권의 내용으로 포스팅 되었습니다. 
```
# Why posting?
* 문자열 쪼개기.. 머릿속에 가장 먼저 들어오는건 다들 무엇인가?
* 학부때 항상 배우던, split과 StringTokenizer 에 대해 복습 차원에서 포스팅 한다.

# 개요
## java.util.StringTokenizer
* 생성자의 종류를 보면 알수 있듯, 내입맛대로 쪼갤수 있고, 혹은 지정된 구분자, 혹은 default 인 띄어쓰기를 이용할 수 있다.
  * **생성자**
    * StringTokenizer(String str)
    * StringTokenizer(String str, String delim)
    * StringTokenizer(String str, String delim, boolean returnDelims) # 쓸일이 있을지..

* 이러한 생성자로 만든 객체는 `hasMoreTokens()` 나 `hasMoreElementgs()` 메소드를 이용해 다음값으로 넘길 수 있다.
  * 좀더 심도있게 말하면, 다음 Token 값을 확인 하고, `nextToken()` 이나 `nextElement()` 메소드로 다음 Token 을 받을 수 있다.

## split()
* 책에서도 나와있듯, StringTokenizer 보단 split을 사용하길 권장한다. (실제 `java documents`에도..)
* 단 아주 큰 문자열을 다루고, 앞에 있는 일부 값만 처리해야 할 경우 split을 사용하면, `메모리 낭비가 심하므로`, 피해야 한다.


# 구현
### StringTokenizer
{% highlight java %}
import java.util.StringTokenizer;

public class Splits {
    public static void main(String[] args) {
       String tmp = "This is a GOD OF JAVA";
        Splits spl = new Splits();
        spl.sample(tmp);
        spl.sampleSplit(tmp);
    }
    private void sample(String tmp){
        StringTokenizer st = new StringTokenizer(tmp);
        while (st.hasMoreElements()){
            String tempData = st.nextToken();
            System.out.println("["+tempData+"]");
        }
        System.out.println();
    }

{% endhighlight %}

### split()
{% highlight java %}
private void sampleSplit(String tmp){
    String[] splitString = tmp.split(" ");
    for (String tempData : splitString){
        System.out.println("["+tempData+"]");
    }
}
{% endhighlight %}
* 좀더 간단해 보이고, 눈에 명확하게 들어온다.
  * String 배열 생성 후, tmp 문자열을 받아, split 메소드를 이용해 분리한다.
  * 메소드 호출시 구분자를 " " 띄어쓰기로 주었고, 해당 매개변수에 구분자를 넣을 수 있다.

## 결과
```concordion
[This]
[is]
[a]
[GOD]
[OF]
[JAVA]

[This]
[is]
[a]
[GOD]
[OF]
[JAVA]
```

## 정리
* 자주 사용하면서, 자주 보이는 클래스와 메소드이다.
  * 아주 기초적이지만, 항상보면 익숙하게 사용하므로, 몸에 아에 베어 버리도록 해야한다.

* Got it??

### Reference
* GODOFJAVA 2

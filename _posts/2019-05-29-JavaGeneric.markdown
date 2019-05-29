---
layout: post
title:  "Java Generic 기본개념부터 사용법까지.."
date:   2019-05-29 16:08:00
author: viewrain
categories: java
---

{% highlight java%}
{% endhighlight %}
```$xslt
이 포스팅은 자바의 신 1,2권의 내용으로 포스팅 되었습니다. 
```
## 개요
* 자바의 제네릭(Generic) 은 형 변환시에 발생 할 수 있는 문제들을 사전에 없애기 위해 만든것 (Java 5 부터 지원) 
    * 다양한 타입의 객체들을 다루는 Method 나 Collection 클래스에 컴파일 시 `타입체크`를 해주는 기능 
    * 클래스 내부에서 사용할 데이터 타입을 나중에 인스턴스화 할 때 확정짓는 것

## 구현방식
* public class 클래스명 <T> { ... }
* public interface 인터페이스명 <T> { ... }

* 자바 모든 클래스의 최상위 부모 클래스는 Object 클래스 이므로, 어떤 타입이든 기본 데이터 타입을 객체화 시킨 wrapperType 의 객체를 대입 받을 수 있다. 

## 구현
* ViewrainDefault.java
{% highlight java%}
public class ViewrainDefault {
    private Object object;
    public Object getObject() {
        return object;
    }
    
    public void setObject(Object object) {
        this.object = object;
    }
}
{% endhighlight %}
* Main.java
{% highlight java%}
public class Main {
    public static void main(String[] args) {
        ViewrainDefault vd = new ViewrainDefault();
        vd.setObject("제네릭");
        String str = vd.getObject();
        System.out.println(str);
    }
}
{% endhighlight %}
위 경우 Main.java 에서 컴파일 에러가 발생한다. 
* 즉 Object 를 구체적인 데이터 타입으로 명시화 해줘야 한다. 
    * String str = (String)vd.getObject();  같이 Casting 해주어야 에러가 발생하지 않음 

* ViewrainGeneric.java
{% highlight java%}
public class ViewrainGeneric <T> {
    private T t;
    public T getObject() {
        return t;
    }
    public void setObject(T t) {
        this.t = t;
    } 
}
{% endhighlight %}
* Main.java
{% highlight java%}
public class Main {
    public static void main (String[] args) {
        ViewrainGeneric<String> vg = new ViewrainGeneric<String>();
        vg.setObject("제네릭");
        String str = vg.getObject();
        System.out.println(str);
    }
}
{% endhighlight %}
강제 타입변환이 발생하지 않는다. 
* 즉 인스턴스화 시 ViewrainGeneric<String> 으로 String 타입을 지정해 주었다. 
    * 이렇게 함으로, 클래스 내부적으로 T 제네릭 타입은 String 으로 자동 재구성 된다. 


## 이유
Generic 이 없다면, 데이터 타입에 맞는 매번 새로운 클래스 또는 메소드를 작성해야 한다. 
* setObject 메소드를 부른다고 가정할때, 매번 데이터 타입에 맞는 메소드를 구현해야 한다. ㅠ.ㅠ

* 반복적인 코드 절약, 코드의 재사용성을 높여주는 이점을 취할 수 있어 사용이 용이하다. 

## 또하나의 예시
{% highlight java%}
import java.io.Serializable;

public class CastingDTO {
    private Object object;

    public void setObject(Object object){
        this.object = object;
    }

    public Object getObject(){
        return object;
    }
}
{% endhighlight %}
* 위의 CastingDTO 를 사용하는 GenericSample.java 를 생성
{% highlight java%}
public class GenericSample {
    public static void main(String[] ar){
        GenericSample ex = new GenericSample();
        ex.checkCastingDTO();
    }

    public void checkCastingDTO(){
        CastingDTO dto1 = new CastingDTO();
        dto1.setObject(new String());
        
        CastingDTO dto2 = new CastingDTO();
        dto2.setObject(new StringBuffer());
        
        CastingDTO dto3 = new CastingDTO();
        dto3.setObject(new StringBuilder());
        {... 생략 }
        String temp1 = (String)dto1.getObject();
        StringBuffer temp2 = (StringBuffer)dto2.getObject();
        StringBuilder temp3 = (StringBuilder)dto3.getObject();
    }

    public void checkDTO(CastingDTO dto){
        Object tempObject = dto.getObject();
        if(tempObject instanceof String){
            System.out.println("String dto");
        }else if(tempObject instanceof StringBuffer){
            System.out.println("StringBuffer dto");
        }else if(tempObject instanceof StringBuilder){
            System.out.println("StringBuilder dto");
        }
    }
}
{% endhighlight %}
* dto1,2,3 객체가 setObject() 를 이용하여 object 값을 설정하고 있음
* 그러나, temp1,2,3 에 다시 dto1,2,3 을 대입하고 있는데.. 이때는 Object 객체가 String, StringBuffer, StringBuilder 이기 때문에 Cating 을 해줘야 한다. 
    * 객체가 3개니깐 (요런식으로 캐스팅을) 해주겠지만.. 많으면 머리가 아플것 같다. 
* 또한 dto2 가 StringBuffer 인지 Builder 인지 헷갈릴 경우는 어떻게 할것인가..? 매번 instanceOf 를 사용할 수도 없고.. 이럴때 제네릭이 유용하다. 

* CastingGenericDTO.java
{% highlight java%}
import java.io.Serializable;

public class CastingGenericDTO<T> implements Serializable {
    private T object;

    public void setObject(T obj){
        this.object = obj;
    }

    public T getObject(){
        return object;
    }
}
{% endhighlight %}
* 클래스를 선언할 때 클래스명에 <T> 가 붙었음, 또한 타입명을 적는 곳에도 Object 대신 T 가 적여 있다. 


* GenericSample2.java
{% highlight java%}
public class GenericSample2 {
    public static void main(String[] ar){
        GenericSample2 ex = new GenericSample2();
        ex.checkGenericDTO();
    }

    public void checkGenericDTO(){
        CastingGenericDTO<String> dto1 = new CastingGenericDTO<String>();
        dto1.setObject(new String());
        CastingGenericDTO<StringBuffer> dto2 = new CastingGenericDTO<StringBuffer>();
        dto2.setObject(new StringBuffer());
        CastingGenericDTO<StringBuilder> dto3 = new CastingGenericDTO<StringBuilder>();
        dto3.setObject(new StringBuilder());

        String temp1 = dto1.getObject();
        StringBuffer temp2 = dto2.getObject();
        StringBuilder temp3 = dto3.getObject();
    }
}
{% endhighlight %}
* 이전 예제에 설명한 것처럼 객체를 인스턴스화 할때 이미 타입을 명시해 주고 있다. 


## 제네릭 타입의 이름
<T> 라고 적혀있는 부분은 사실 아무거나 써도 상관없다.. 단 개발자들의 컨벤션이 존재하기 마련이고.. 제네릭 또한 다음과 같이 사용한다. 

E : 요소 (Element)
K : 키 (Key) 
N : 숫자 (Number) 
T : 타입 (Type) 
V : 값 (Value) 
S,U,V : 두번째, 세번째, 네번째에 선언된 타입 


## 제네릭의 Wildcard 타입
* 메소드 선언시 제네릭 타입의 제한을 해소하기 위해 특정 타입 대신 <?> 를 사용
    * 해당 타입을 정확히 모르기 때문에 Object 로 받음

{% highlight java%}
public void wildcardMethod(WildcardGeneric<?> c) {
    Object value = c.getWildcard();
    System.out.println(value);
}
{% endhighlight %}

## 제한이 있는 Wildcard 타입

* 아무런 제약이 없는 <?> 는 어떤 타입도 올 수 있으므로, 타입에 제한을 걸어둠
  * <? extends TypeName> 과 같이 TypeName 클래스를 확장한 모든 클래스를 의미한다. 
{% highlight java%}
public void boundedWildcardMethod(WildcardGeneric <? extends Car> c) {
    Car value = c.getWildcard();
    System.out.println (value);
} 
{% endhighlight %}

## 제네릭한 메소드 선언
* wildcard 사용시에는 매개 변수로 넘어온 타입을 변경할 수 없다. 
    * 메소드를 제네릭하게 선언하면 제네릭 타입의 값을 변경 가능하다. 

{% highlight java%}
public class GenericWildcardSample {
    public static void main(String[] ar){
        GenericWildcardSample ex = new GenericWildcardSample();
        ex.callGenericMethod();
    }
    public <T> void genericMethod(WildCardGeneric<T> c, T addValue){
        c.setWildCard(addValue);
        T value = c.getWildCard();
        System.out.println(value);
        // Teemo
    }

    public void callGenericMethod(){
        WildCardGeneric<String> wildcard = new WildCardGeneric<String>();
        genericMethod(wildcard, "Teemo");
    }
}
{% endhighlight %}

* genericMethod() 의 선언부를 보면 <T> 를 써서 제네릭하게 메소드를 선언했다. 
    * 또한 매개변수를 보면 T 타입의 addValue 객체도 받고 있으며, 메소드 안에서 c의 값을 addvalue 로 정해주고 있다. 
    * 여기서 T 또한 범위를 설정 해 줄수 있다. 
        * public ❮T extends Car❯ void genericMethod(WildCardGeneric❮T❯ c, T addValue)
    * 한 메소드에서 두개 이상의 제네릭 타입을 매개변수로 받을 때는 , (쉼표) 로 구분해 주면 된다. 
        * public <S, T extends Car> void genericMethod(WildCard Generic❮?❯ c, T addValue1, S addValue2)



## Refference
* https://onsil-thegreenhouse.github.io/programming/java/2018/02/17/java_tutorial_1-21/
* https://limkydev.tistory.com/56


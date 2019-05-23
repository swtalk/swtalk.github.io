---
layout: post
title:  "Spring Framework Basic"
date:   2019-05-23 14:08:00
author: viewrain
categories: spring
---

## Spring 기초
### Spring 정의 
* 자바 엔터프라이즈 개발을 편하게 해주는 오픈소스 경량급 애플리케이션 프레임워크
    * 자바(JAVA) 플랫폼을 위한 오픈소스(Open Source) 애플리케이션 프레임워크(Framework)
    * 자바 개발을 위한 프레임워크로 종속 객체를 생성해주고,  조립해주는 도구
    * 자바로 된 프레임워크로 자바SE로 된 자바 객체(POJO)를 자바EE에 의존적이지 않게 연결해주는 역할.

* Spring은 자바 엔터프라이즈 애플리케이션 개발에 사용되는 프레임워크다. 애플리케이션 프레임워크는 애플리케이션 개발을 빠르고 효율적으로 할 수 있도록 애플리케이션의 바탕이 되는 틀과 공통 프로 그래밍 모델, 기술 API 등을 제공해준다.

* Spring은 Spring 컨테이너 또는 애플리케이션 컨텍스트라고 불리는 Spring 런타임 엔진을 제공한다. Spring 컨테이너는 설정정보를 참고로 해서 애플리케이션을 구성하는 오브젝트를 생성하고 관리한다.


### Spring 동작 방식
![image](/assets/Postingimg/springlogic.png)

### Spring 핵심 모델
* IoC/DI 
    * 오브젝트의 생명주기와 의존관계에 대한 프로그래밍 모델
* 서비스 추상화
    * `환경이나 서버, 특정 기술에 종속되지 않고` 이식성이 뛰어나며 유연한 애플리케이션을 만들 수 있다.
* AOP
    * 애플리케이션 코드에 산재해서 나타나는 부가적인 기능을 독립적으로 모듈화 하는 프로그래밍 모델

### 디자인 패턴
* 소프트웨어 설계 시 특정 상황에서 자주 만나는 문제를 해결하기 위해 사용할 수 있는 재사용 가능한 솔루션. 주로 객체지향 설계에 관한 것이고, 대부분 객체지향적 설계 원칙을 이용해 문제를 해결한다.
    * `디자인 패턴의 경우 다룰 내용이 많으므로, 별도로 다루도록 한다.` 

### Spring 특징
#### IOC
* IoC(Inversion of Control)를 풀이하면 제어권의 반전 즉 누군가가 제어를 대신해준다는 의미로 생각할수 있을것같다.
* Spring 을 쓰기전엔 개발자가 프로그램의 흐름 (어플리케이션 코드) 를 직접 제어했다. 하지만 Spring 에서는 Framework 가 주도하게 된다.
    * 객체의 생명 주기 자체를 컨테이너가 맡아서 한다.  제어권이 컨테이너로 넘어가게 되고, 제어의 흐름이 바뀌었다 하여, IoC (제어의 역전) 이라 부름 
    * 제어의 흐름이 Framework 로 변경됨에 따라 DI(의존성 주입) , AOP(관점지향프로그래밍) 등이 가능해짐 
        * 역으로 제어권이 없다면 `@Autowired` 를 통한 의존성 주입등이 불가능해진다. 
![image](/assets/Postingimg/springioc.png)
    * DI (Dependency Injection)
        * 객체간의 의존성을 자신이 아닌 외부에서 주입하는 개념
        
        
{% highlight java%}
package com.toast.cloud.iaas.gauge.DTO;


public class DIExample {
    public static void main(String[] args) {
        MessageBeanEN bean = new MessageBeanEN();
        bean.sayHello("Viewrain");
    }
}

class MessageBeanEN {
    public void sayHello(String name) {
        System.out.println("Hello " + name);
    }
}

class MessageBeanKR {
    public void sayHello(String name) {
        System.out.println("안녕" + name);
    }
}
{% endhighlight %}

```
만약 MessageBeanKR 의 sayHello 함수를 불러 한글로 출력하고 싶을 경우 DIExample 의 Main 메소드에서 객체를 만들 때 EN을 KR로 바꿔 주어야 한다. 
이런 경우 DIExample 이 MessageBeanEN(KR) 에 의존성을 가지고 있다고 한다. 
```
* 이러한 의존성을 해결하기 위해선, 객체의 인스턴스를 외부에서 생성받을 필요가 있음
* 파울러 저술에 의하면, 객체의 의존성 주입은 3가지 방식이 존재한다. 
    * 생성자를 이용한 의존성 주입
    * setter 를 이용한 의존성 주입 
    * 초기화 인터페이스를 이용한 의존성 주입 

* 초기화 인터페이스를 이용한 의존성 주입의 예시 
{% highlight java%}
package com.toast.cloud.iaas.gauge.DTO;

interface MessageBean{
    public void sayHello(String name);
}
public class DIExample {
    public static void main(String[] args) {
        MessageBean bean = new MessageBeanKR();
        bean.sayHello("Viewrain");
    }
}

class MessageBeanEN implements MessageBean {
    public void sayHello(String name) {
        System.out.println("Hello " + name);
    }
}

class MessageBeanKR implements MessageBean{
    public void sayHello(String name) {
        System.out.println("안녕 " + name);
    }
}
{% endhighlight %}


* 이렇게 하더라도, 약간의 코드변화는 일어나게 된다. 이게 싫으므로..
* xml 에 설정파일을 추가하여 MessageBean 에 주입할 객체 정보를 미리 담아 놓는다. 

{% highlight java%}
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:c="http://www.springframework.org/schema/c"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="messageBean" class="MessageBeanEN"/>
</beans>

{% endhighlight %}


이 경우 필요에 따라 설정파일만 변경해 주면 프로그램을 제어 할 수 있다. 

{% highlight java%}
package com.toast.cloud.iaas.gauge.DTO;

import org.apache.naming.factory.BeanFactory;
import org.springframework.beans.factory.xml.XmlBeanFactory;
import org.springframework.core.io.FileSystemResource;

interface MessageBean{
    public void sayHello(String name);
}
public class DIExample {
    public static void main(String[] args) {
        BeanFactory factory = new XmlBeanFactory(new FileSystemResource("resource/bean.xml"));
        MessageBean bean = ((XmlBeanFactory) factory).getBean("messageBean", MessageBean.class);
        bean.sayHello("viewrain");
    }

}
{% endhighlight %}


* DL (Dependency Lookup) 
  * 저장소에 저장되어 있는 빈(Bean)에 접근하기 위하여 개발자들이 컨테이너에서 제공하는 API를 이용하여 사용하고자 하는 빈(Bean)을 Lookup하는 것
    * 의존대상(사용할 객체)을 검색(lookup)을 통해 반환받는 방식
    * factory.getBean(id); 내가 찾고자 하는 대상을 검색해서 객체를 확보한다



## Reference
* https://ooz.co.kr/170
* https://jongmin92.github.io/2018/05/20/Spring/toby-8/

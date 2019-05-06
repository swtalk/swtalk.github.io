---
layout: post
title:  "Thread "
date:   2019-05-06 11:33:00
author: viewrain
categories: java
---

```$xslt
이 포스팅은 자바의 신 1,2권의 내용으로 포스팅 되었습니다. 
```

# 개요

## Process
* 프로그램을 `실행` 시켜서, 동작하게 만들면 이것을 `프로세스`라 한다.
* 해당 프로세스는 각각 하나의 처리 경로를 가지고 있고, 직렬적이다.
  * 어떠한 일을 수행하는 것에 있어, 개발자가 원하는 순서대로 일처리를 하지만, 순서에 상관없이 동시에 처리하고 싶을때 Thread(스레드) 를 이용한다.

## Thread ?
* 하나의 프로세스 내부에서 독립적으로 실행되는 하나의 작업 단위를 말하며, 세부적으로는 운영체제에 의해 관리되는 하나의 작업 혹은 태스크를 의미
  * JVM에 의해 하나의 프로세스가 발생하고 main( ) 안의 실행문 들이 하나의 스레드이다.
  * main( ) 이외의 또 다른 스레드를 만들려면 Thread 클래스를 상속하거나 Runnable 인터페이스를 구현한다.
  * 다중 스레드 작업 시에는 각 스레드 끼리 정보를 주고받을 수 있어 처리 과정의 오류를 줄일 수 있다.
  * 프로세스끼리는 정보를 주고받을 수 없다.

### Extends Thread
* Java Thread 를 구동하는 것중 대표적인건 Thread 클래스를 상속받는 것이다.

#### 구현
{% highlight java %}
/* 간단한 쓰레드 프로그램 */
import java.util.Random;
public class ThreadTest extends Thread {
	private int id = -1;
	public ThreadTest(int id){
		this.id = id;
	}
	public void run(){
		System.out.println( id + "번 쓰레드 동작 중..." );
		Random r = new Random(System.currentTimeMillis());
		try {
			long s = r.nextInt(3000); // 3초내로 끝내자.
			Thread.sleep(s); // 쓰레드를 잠시 멈춤
		} catch (InterruptedException e) {
			e.printStackTrace();
		}

		System.out.println( id + "번 쓰레드 동작 종료..." );
	}

	public static void main(String[] args) {

		System.out.println("Start main method.");

		for(int i = 0 ; i < 10 ; i++ ){
			ThreadTest test = new ThreadTest(i);
			test.start(); // 이 메소드를 실행하면 Thread 내의 run()을 수행한다.
		}

		System.out.println("End main method.");
	}
}
{% endhighlight %}

#### 결과
{% highlight java%}
Start main method.
0번 쓰레드 동작 중...
2번 쓰레드 동작 중...
1번 쓰레드 동작 중...
3번 쓰레드 동작 중...
4번 쓰레드 동작 중...
5번 쓰레드 동작 중...
6번 쓰레드 동작 중...
7번 쓰레드 동작 중...
End main method.
9번 쓰레드 동작 중...
8번 쓰레드 동작 중...
1번 쓰레드 동작 종료...
4번 쓰레드 동작 종료...
8번 쓰레드 동작 종료...
7번 쓰레드 동작 종료...
9번 쓰레드 동작 종료...
6번 쓰레드 동작 종료...
0번 쓰레드 동작 종료...
2번 쓰레드 동작 종료...
5번 쓰레드 동작 종료...
3번 쓰레드 동작 종료...
{% endhighlight %}

* 여기서 main() 메소드가 자신이 실행 시킨 쓰레드들이 종료가 되지 않았음에도 먼저 끝나버리는 것을 볼 수 있다.
* 조금만 코드를 바꿔서, index 변수를 프로그램 마지막에 출력해 보도록 하자. (10을 예상)

#### 구현

{%highlight java%}
/* 간단한 쓰레드 프로그램(변경 1) */
import java.util.Random;
public class ThreadTest extends Thread {
	// index 변수를 추가해서 스레드가 동작시에 해당 변수를 증가시키도록 할것이다.
	private static int index = 0;

	private int id = -1;
	public ThreadTest(int id){
		this.id = id;
	}
	public void run(){
		System.out.println( id + "번 쓰레드 동작 중..." );
		Random r = new Random(System.currentTimeMillis());
		try {
			long s = r.nextInt(3000); // 3초내로 끝내자.
			Thread.sleep(s); // 쓰레드를 잠시 멈춤
			index++; // index 변수를 증가
		} catch (InterruptedException e) {
			e.printStackTrace();
		}

		System.out.println( id + "번 쓰레드 동작 종료..." );
	}

	public static void main(String[] args) {

		System.out.println("Start main method.");

		for(int i = 0 ; i < 10 ; i++ ){
			ThreadTest test = new ThreadTest(i);
			test.start(); // 이 메소드를 실행하면 Thread 내의 run()을 수행한다.
		}

		System.out.println("current Index: "+ index); // index의 값을 반환
		System.out.println("End main method.");
	}
}
{% endhighlight java%}

#### 결과

{%highlight java%}
ain method.
0번 쓰레드 동작 중...
1번 쓰레드 동작 중...
2번 쓰레드 동작 중...
3번 쓰레드 동작 중...
4번 쓰레드 동작 중...
5번 쓰레드 동작 중...
6번 쓰레드 동작 중...
7번 쓰레드 동작 중...
current Index: 0
8번 쓰레드 동작 중...
9번 쓰레드 동작 중...
End main method.
5번 쓰레드 동작 종료...
6번 쓰레드 동작 종료...
1번 쓰레드 동작 종료...
0번 쓰레드 동작 종료...
3번 쓰레드 동작 종료...
2번 쓰레드 동작 종료...
4번 쓰레드 동작 종료...
8번 쓰레드 동작 종료...
9번 쓰레드 동작 종료...
7번 쓰레드 동작 종료...
{% endhighlight java%}

* 원하는대로 동작하지 않음을 확인 할 수 있다. 제멋대로 thread 가 종료되고 main 메소드도 thread 이므로, 뒤죽박죽이다.

#### 구현

{%highlight java%}
/* 간단한 쓰레드 프로그램(변경 2) */
import java.util.Random;
public class ThreadTest extends Thread {
	// index 변수를 추가해서 스레드가 동작시에 해당 변수를 증가시키도록 할겁니다.
	private static int index = 0;

	private int id = -1;
	public ThreadTest(int id){
		this.id = id;
	}
	public void run(){
		System.out.println( id + "번 쓰레드 동작 중..." );
		Random r = new Random(System.currentTimeMillis());
		try {
			long s = r.nextInt(3000); // 3초내로 끝내자.
			Thread.sleep(s); // 쓰레드를 잠시 멈춤
			index++; // index 변수를 증가
		} catch (InterruptedException e) {
			e.printStackTrace();
		}

		System.out.println( id + "번 쓰레드 동작 종료..." );
	}

	public static void main(String[] args) {

		System.out.println("Start main method.");

		for(int i = 0 ; i < 10 ; i++ ){
			ThreadTest test = new ThreadTest(i);
			test.start(); // 이 메소드를 실행하면 Thread 내의 run()을 수행한다.
		}

		try {
			Thread.sleep(5000); // 쓰레드가 종료할 때까지의 충분한 시간을 기다림
		} catch (InterruptedException e) {
			e.printStackTrace();
		}

		System.out.println("current Index: "+ index); // index의 값을 반환
		System.out.println("End main method.");
	}
}
{% endhighlight java%}
* 아까와 다르게 메인 메소드 내에서 sleep() 메소드를 추가하였다.
* 프로그램 내에서 쓰레드가 3초 내로 끝난다는 것을 알고 있기 때문에 일부러 메인 메소드에선 5초를 멈추게 했다.

#### 결과
```
Start main method.
0번 쓰레드 동작 중...
1번 쓰레드 동작 중...
2번 쓰레드 동작 중...
3번 쓰레드 동작 중...
4번 쓰레드 동작 중...
5번 쓰레드 동작 중...
6번 쓰레드 동작 중...
7번 쓰레드 동작 중...
8번 쓰레드 동작 중...
9번 쓰레드 동작 중...
9번 쓰레드 동작 종료...
8번 쓰레드 동작 종료...
6번 쓰레드 동작 종료...
7번 쓰레드 동작 종료...
1번 쓰레드 동작 종료...
2번 쓰레드 동작 종료...
3번 쓰레드 동작 종료...
0번 쓰레드 동작 종료...
5번 쓰레드 동작 종료...
4번 쓰레드 동작 종료...
current Index: 7
End main method.
```

* 하지만 이런코드는... 할많하않....

## Thread Join
* 메인 메소드가 자신이 실행시킨 쓰레드들이 전부 종료되기도 전에 먼저 종료가 되는 것을 해결하는 좋은 방법은 쓰레드 조인Thread Join을 이용하는 것이다.
* thread join 은 현재 스레드가 동작중이면 끝날때까지 기다리는 착한 메소드 이다.

{%highlight java%}
/* 간단한 쓰레드 프로그램(변경 3) */
import java.util.ArrayList;
import java.util.Random;
public class ThreadTest extends Thread {
	private static int index = 0;

	private int id = -1;
	public ThreadTest(int id){
		this.id = id;
	}
	public void run(){
		System.out.println( id + "번 쓰레드 동작 중..." );
		Random r = new Random(System.currentTimeMillis());
		try {
			long s = r.nextInt(3000); // 3초내로 끝내자.
			Thread.sleep(s); // 쓰레드를 잠시 멈춤
			index++; // index 변수를 증가시킵니다.
		} catch (InterruptedException e) {
			e.printStackTrace();
		}

		System.out.println( id + "번 쓰레드 동작 종료..." );
	}

	public static void main(String[] args) {

		System.out.println("Start main method.");

		ArrayList<Thread> threadList = new ArrayList<Thread>();

		for(int i = 0 ; i < 10 ; i++ ){
			ThreadTest test = new ThreadTest(i);

			test.start(); // 이 메소드를 실행하면 Thread 내의 run()을 수행한다.
			threadList.add(test); // 생성한 쓰레드를 리스트에 삽입
		}

		for(int i = 0 ; i < threadList.size(); i++){
			try {
				threadList.get(i).join(); // 쓰레드의 처리가 끝날때까지 기다린다.
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		}

		System.out.println("current Index: "+ index); // index의 값을 반환
		System.out.println("End main method.");
	}
}
{% endhighlight java%}

* 추가가 된 것은 threadList에 쓰레드를 삽입하고 이후 threadList를 순회하며 각 쓰레드에서 join() 메소드를 실행시켜주는 것이다.

#### 결과
```
Start main method.
0번 쓰레드 동작 중...
1번 쓰레드 동작 중...
2번 쓰레드 동작 중...
3번 쓰레드 동작 중...
4번 쓰레드 동작 중...
5번 쓰레드 동작 중...
6번 쓰레드 동작 중...
7번 쓰레드 동작 중...
8번 쓰레드 동작 중...
9번 쓰레드 동작 중...
4번 쓰레드 동작 종료...
1번 쓰레드 동작 종료...
6번 쓰레드 동작 종료...
5번 쓰레드 동작 종료...
0번 쓰레드 동작 종료...
2번 쓰레드 동작 종료...
3번 쓰레드 동작 종료...
8번 쓰레드 동작 종료...
7번 쓰레드 동작 종료...
9번 쓰레드 동작 종료...
current Index: 9
End main method.
```

## Thread Synchronized
* 스레드 safety 하다, 이런이야기를 들어봤을 것이다. 즉 동작중인 프로세스에 또 치고 들어올 경우를 대비해 Syncronized 로 감싸야 한다.

{%highlight java%}
/* 간단한 쓰레드 프로그램(변경 4) */
import java.util.ArrayList;
import java.util.Random;
public class ThreadTest extends Thread {
	private static int index = 0;

	private int id = -1;
	public ThreadTest(int id){
		this.id = id;
	}
	public void run(){
		System.out.println( id + "번 쓰레드 동작 중..." );
		Random r = new Random(System.currentTimeMillis());
		try {
			long s = r.nextInt(3000); // 3초내로 끝내자.
			Thread.sleep(s); // 쓰레드를 잠시 멈춤
			setIndex();
		} catch (InterruptedException e) {
			e.printStackTrace();
		}

		System.out.println( id + "번 쓰레드 동작 종료..." );
	}

	public synchronized static void setIndex(){
		index++; // index 변수를 증가시킵니다.
	}

	public static void main(String[] args) {

		System.out.println("Start main method.");

		ArrayList<Thread> threadList = new ArrayList<Thread>();

		for(int i = 0 ; i < 10 ; i++ ){
			ThreadTest test = new ThreadTest(i);

			test.start(); // 이 메소드를 실행하면 Thread 내의 run()을 수행한다.
			threadList.add(test); // 생성한 쓰레드를 리스트에 삽입
		}

		for(int i = 0 ; i < threadList.size(); i++){
			try {
				threadList.get(i).join(); // 쓰레드의 처리가 끝날때까지 기다립니다.
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		}

		System.out.println("current Index: "+ index); // index의 값을 반환
		System.out.println("End main method.");
	}
}
{% endhighlight java%}

* Syncronized 를 사용하면, setIndex() 메소드에 스레드 들이 동시에 접근 할 수 없다.
```
Start main method.
0번 쓰레드 동작 중...
1번 쓰레드 동작 중...
2번 쓰레드 동작 중...
3번 쓰레드 동작 중...
4번 쓰레드 동작 중...
5번 쓰레드 동작 중...
6번 쓰레드 동작 중...
7번 쓰레드 동작 중...
8번 쓰레드 동작 중...
9번 쓰레드 동작 중...
7번 쓰레드 동작 종료...
9번 쓰레드 동작 종료...
4번 쓰레드 동작 종료...
5번 쓰레드 동작 종료...
3번 쓰레드 동작 종료...
2번 쓰레드 동작 종료...
8번 쓰레드 동작 종료...
6번 쓰레드 동작 종료...
1번 쓰레드 동작 종료...
0번 쓰레드 동작 종료...
current Index: 10
End main method.
```

## Interface Runnuble
* Thread 를 이용하기 위해선, Thread 클래스를 상속받는 것과 Runnable 인터페이스를 구현하는 두가지 방법이 있다.
* 보통 쓰레드 객체를 만들 때 위의 예처럼 Thread를 상속하여 만들기도 하지만 보통 Runnable 인터페이스를 구현하도록 하는 방법을 많이 사용한다.

{%highlight java%}
import java.util.ArrayList;

public class Test implements Runnable {
    int seq;
    public Test(int seq) {
        this.seq = seq;
    }
    public void run() {
        System.out.println(this.seq+" thread start.");
        try {
            Thread.sleep(1000);
        }catch(Exception e) {
        }
        System.out.println(this.seq+" thread end.");
    }

    public static void main(String[] args) {
        ArrayList<Thread> threads = new ArrayList<Thread>();
        for(int i=0; i<10; i++) {
            Thread t = new Thread(new Test(i));
            t.start();
            threads.add(t);
        }

        for(int i=0; i<threads.size(); i++) {
            Thread t = threads.get(i);
            try {
                t.join();
            }catch(Exception e) {
            }
        }
        System.out.println("main end.");
    }
}
{%endhighlight java%}

* Thread를 extends 하던 것에서 Runnable을 implements 하도록 변경되었다. (Runnable 인터페이스는 run 메소드를 구현하도록 강제한다.) 그리고 Thread 를 생성하는 부분을 다음과 같이 변경했다.

## Thread VS Runnuble
* 가장 큰 것은 Thread를 상속 받게 되면 상속 받은 클래스는 다른 클래스를 상속 받을 수가 없지만 Runnable은 인터페이스이기 때문에 구현만 하면 되고 다른 필요한 클래스를 상속 받을 수 있다는 것이다.



## 정리
* 언뜻 봐서는 Runnable 인터페이스를 구현하는 것이 어떠한 상황에서든 합당해 보이나 프로그래밍은 언제나 그렇듯이 일방적인 해답만 있는건 아니니 사용 프로그램의 케이스를 따져서 Thread를 상속 받을지 Runnable 인터페이스를 구현할지 결정해야 한다.
* Got it??

### Reference
* GODOFJAVA 2
* https://jdm.kr/blog/72
* https://wikidocs.net/230

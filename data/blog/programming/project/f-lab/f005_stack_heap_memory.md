---
title: '[F-Lab] Stack / Heap Memory'
date: '2021-05-08 00:00:05'
tags: ['F-Lab', 'Stack', 'Heap', 'Memory']
draft: false
summary: 'Stack / Heap Memory 에 대해 학습한 내용을 정리 했습니다.'
---

> ✍️ 기본 자료형과 참조 자료형을 공부하고 나니 Stack과 Heap 메모리에 대해서도 궁금해졌다. 무엇보다 첫 멘토링 시간에 이 둘에 대한 설명을 제대로 하지 못해 복습을 하긴 했어야 했다.  
> 또, 실무에서 이에 대한 개념이 제대로 잡히지 않아 여러 번 실수를 경험한 적이 있다. 앞으로 이런 실수를 다시 되풀이하지 않기 위해 대략적으로만 파악하고 있던 메모리 구조를 알아보자!

---

# **Stack Memory**

- 기본 자료형으로 선언된 변수는 값과 함께 할당된다.
- 참조 자료형은 할당된 Heap Memory의 참조값과 할당된다.
- 각 스레드는 각자의 Stack Memory 영역을 가진다.
- 지역 변수, 매개 변수의 생명주기는 자신이 속한 곳의 스코프가 실행되면 사라진다.

```java
public class StackMemoryTest {
    public static int changeStayus(int param) {
        int tmp    = param * 10;
        int result = tmp / 2;
        return result;
    }

    public static void main(String[] args) {
        int value = 10;
        value = changeStayus(value);
        System.out.println(value);
    }
}
```

위 코드를 살펴보자.  
main() 메소드에 선언된 value 변수는 10 이라는 값이 Stack Memory에 할당된다. 그 이후에 changeStatus() 메소드를 호풀하면서 인자값으로 value를 넘겨주게 되는데, 참조 자료형처럼 값을 참조하지 않고 changeStatus()의 매개 변수 param에 해당 값이 복사되어 Stack Memory에 할당된다. (그림으로 그리고 싶은데 팔을 다쳐서 그릴 수가 없다.)  
이제 changeStatus() 메소드로 이동하여 Stack Memory에 어떤 값이 저장되는지 알아보자. changeStatus() 메소드 내에서 선언된 tmp 변수가 Stack Memory에 할당되고, result 변수가 선언되며 Sack Memory에 할당된다. 이제 result 변수가 main() 메소드의 value 변수에 리턴되면서 changeStatus() 메소드에서 선언된 매개 변수 param과 지역 변수 tmp, result은 해당 메소드의 마지막 스코프가 실행되면서 Stack Memory에서 pop되어 사라진다.  
main() 메소드의 value 변수의 값은 changeStatus() 메소드가 종료되면서 리턴한 값으로 초기화되고 해당 변수는 System.out.println()에 의해 콘솔창에 출력되고 main() 메소드의 마지막 스코프가 실행되면서 Stack Memory에 마지막으로 할당된 value 변수도 pop되어 사라진다.

# **Heap Memory**

- 생명주기가 긴 데이터가 저장된다.
- 모든 참조 자료형은 해당 영역에 할당되며, 이에 해당하는 주소값이 레퍼런스 변수와 함께 Stack Memory에 저장된다.
- 스레드의 개수와는 관계없이 하나의 Heap Memory만 존재한다.

```java
public class HeapMemoryTest {
    public static void main(String[] args) {
				int value = 10;
        String hello = "hello";
        HeapMemoryTest heapMemoryTest = new HeapMemoryTest();
    }
}
```

위의 코드를 살펴보면, value 변수는 Stack Memory에 값과 변수가 할당되었고, hello 변수와 heapMemoryTest 변수는 Heap Memory(hello 변수는 Constant Pool) 할당되어 해당 주소값과 변수가 Staack Memory에 할당된 것을 지난 시간 공부한 것을 통해서 알 수 있다.  
이 때, 참조 자료형들은 Pass By Reference 라는 특성이 있는데, 이는 기본 자료형과 다르다. 기본 자료형은 특정 변수가 어떤 메소드의 인자로 전달될 때 값이 복사되어 호출된 메소드에서 인자로 받은 매개 변수에 어떤 짓을 해도 원래 있던 변수는 값이 변하지 않는다.  
하지만 참조 자료형의 경우 해당 참조 변수를 어떤 메소드에 인자로 전달하여 해당 메소드 내에서 인자로 전달받은 매개 변수의 값을 수정한다면, 원래 있던 참조 변수의 값도 변화한다. 이는 기본 자료형처럼 값을 복사하는 것이 아니라, Heap Memory에 저장되어 있는 값을 함께 참조하기 때문이다.  
그렇기 때문에 기존의 참조 변수가 어떠한 메소드에 인자로 전달되고, 그 메소드에서 값이 변경되면 기존의 참조 변수의 값은 아무도 참조하지 않은 상태로 Heap Memory에 남게 된다. 이러한 쓰레기 값을 Garbage Collector가 알아서 정리를 해준다.

### **String, Wrapper Class**

다만 모든 참조 자료형이 인자로 전달될 때 모두 같은 값을 참조하는 것은 아니다. String과 기본 자료형의 Wrapper 클래스들은 불변하는 값을 가지는 규칙 때문에 immutable 한 특징을 가진다. 때문에 해당 변수들을 어떠한 메소드에 인자로 전달되어 메소드 내에서 값이 변경되어도 원래 있던 변수의 값은 변경되지 않는다.

```java
public class HeapMemoryTest {
    public static void main(String[] args) {
        String str = "hello";
        Integer integer = 2021;
        System.out.println("str = " + str);
        System.out.println("integer = " + integer);

        changeStatus(str, integer);

        System.out.println("str = " + str);
        System.out.println("integer = " + integer);
    }

    public static void changeStatus(String strParam, Integer intParam) {
        String tmpStr = strParam + " world!";
        Integer tmpInt = intParam + intParam;
        System.out.println("tmpStr = " + tmpStr);
        System.out.println("tmpInt = " + tmpInt);
    }
}
/*
	str = hello
	integer = 2021

	tmpStr = hello world!
	tmpInt = 4084441

	str = hello
	integer = 2021
*/
```

위 코드를 보면 알 수 있듯, String과 Integer 클래스는 참조 자료형이지만 마치 기본 자료형처럼 작동한다. 이는 모든 Wrapper 클래스에 final 접근 제어자가 선언되었기 때 문인데, JDK 5 버전부터 AutoBoxing/UnBoxing을 지원하면서 Wrapper 클래스의 인스턴스가 불변하지 않는다는 원칙에 어긋나는 것과 메모리와 CPU 낭비 등 다양한 문제를 해결하기 위함이다.

---

### **참고 사이트**

[참고 아티클 - 1](https://stackoverflow.com/questions/28237542/why-wrapper-classes-in-java-are-final)

[참고 아티클 - 2](https://yaboong.github.io/java/2018/05/26/java-memory-management/)

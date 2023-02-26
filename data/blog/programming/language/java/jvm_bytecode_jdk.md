---
title: 'JVM / Bytecode / JDK / JRE / JIT Compiler'
date: '2020-12-14'
tags: ['JVM', 'Bytecode', 'JDK', 'JRE', 'JIT Compiler']
draft: false
summary: 'JVM / Bytecode / JDK / JRE / JIT Compiler 의 개념에 대해 이해한 내용을 정리 했습니다.'
---

# **들어가기에 앞서**

> Java를 국비를 통해 공부하면서 해당 언어에 대한 기본기를 더 탄탄히 하고 싶다는 목표가 있었다.  
> 하늘도 알았는지 2020년이 끝나기 전에 백기선님께서 운영하는 Online Java Study에 참여할 수 있게 되었다.그래서 오랜 기간 까먹고 지내던 Java 기본기를 더 탄탄히 하고, 알지 못했던 부분을 채우는 시간이 될 수 있도록 꾸준히 노력해야겠다.

# **Java Compile Structure**

> Java는 **객체지향 언어**로서,  
> **어떠한 운영체제에서도 동일하게 프로그램이 실행**될 수 있도록 하는 특징을 가진다.

![Java Compile Structure](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FNFcHM%2FbtqP11iJXvY%2FbWW7uXKM26Lqer3NZzSURk%2Fimg.png)

1.  **_Java File Build_**
    1.  **.java → Java Compiler(javac) → .class**
        - Java Compiler(javac)가 .java 확장자 파일을 JVM(Java Virture Machine)이 이해할 수 있는 Bytecode로 번역하여 확장자가 .class 확장자 파일을 생성한다.
2.  **JVM**
    1.  **Class Loader**
    2.  **Runtime Data Areas**
    3.  **Execution Engine**
        - 번역된 .class 확장자 파일을 JVM의 구성요소 중 하나인 Class Loader를 통해 Bytecode를 Runtime Data Areas에 로드한다.  
          Runtime Data Areas에서 운영체제 위에서 실행될 수 있는 메모리 영역을 할당 받으면 Bytecode를 Execution Engine으로 이동하여 Interpreter 혹은 JIT Compiler를 통해 기계어로 번역한다.

---

# **Bytecode**

> Java Compiler를 이용해 .java를 **JVM이 이해할 수 있도록 번역한 코드이며, .class 확장자를 가진다.**

각각의 Bytecode는 1byte로 구성되지만 몇 개의 파라미터가 사용되는 경우가 있어 총 몇 바이트로 구성되는 경우가 있다.  
또한, Bytecode를 조작해 많은 것들을 활용할 수 있다.

1.  프로그램 분석
    - 코드에서 버그 찾는 툴
    - 코드 복잡도 계산
2.  클래스 파일 생성
    - 프록시
    - 특정 API 호출 접근 제한
    - 스칼라 같은 언어의 컴파일러
3.  프로파일러
4.  최적화

### **일반적으로 작성된 .java 파일**

```java
package Week_01;

public class Day_01 {
    public static void main(String[] args) {
        System.out.println("Hello Study!");
    }
}
```

### **IntelliJ에서 Bytecode 확인하기**

- IntelliJ → Command + A → Show Bytecode

```java
// class version 55.0 (55)
// access flags 0x21
public class Week_01/Day_01 {

  // compiled from: Day_01.java

  // access flags 0x1
  public <init>()V
   L0
    LINENUMBER 3 L0
    ALOAD 0
    INVOKESPECIAL java/lang/Object.<init> ()V
    RETURN
   L1
    LOCALVARIABLE this LWeek_01/Day_01; L0 L1 0
    MAXSTACK = 1
    MAXLOCALS = 1

  // access flags 0x9
  public static main([Ljava/lang/String;)V
   L0
    LINENUMBER 5 L0
    GETSTATIC java/lang/System.out : Ljava/io/PrintStream;
    LDC "Hello Study!"
    INVOKEVIRTUAL java/io/PrintStream.println (Ljava/lang/String;)V
   L1
    LINENUMBER 6 L1
    RETURN
   L2
    LOCALVARIABLE args [Ljava/lang/String; L0 L2 0
    MAXSTACK = 2
    MAXLOCALS = 1
}
```

### **javap를 이용해 .class 파일 확인하기**

- Terminal → javap -v -l -p 경로와 파일 이름.class

```java
public class Week_01.Day_01
  minor version: 0
  major version: 55
  flags: (0x0021) ACC_PUBLIC, ACC_SUPER
  this_class: #5                          // Week_01/Day_01
  super_class: #6                         // java/lang/Object
  interfaces: 0, fields: 0, methods: 2, attributes: 1
Constant pool:
   #1 = Methodref          #6.#15         // java/lang/Object."<init>":()V
   #2 = Fieldref           #16.#17        // java/lang/System.out:Ljava/io/PrintStream;
   #3 = String             #18            // Hello Study!
   #4 = Methodref          #19.#20        // java/io/PrintStream.println:(Ljava/lang/String;)V
   #5 = Class              #21            // Week_01/Day_01
   #6 = Class              #22            // java/lang/Object
   #7 = Utf8               <init>
   #8 = Utf8               ()V
   #9 = Utf8               Code
  #10 = Utf8               LineNumberTable
  #11 = Utf8               main
  #12 = Utf8               ([Ljava/lang/String;)V
  #13 = Utf8               SourceFile
  #14 = Utf8               Day_01.java
  #15 = NameAndType        #7:#8          // "<init>":()V
  #16 = Class              #23            // java/lang/System
  #17 = NameAndType        #24:#25        // out:Ljava/io/PrintStream;
  #18 = Utf8               Hello Study!
  #19 = Class              #26            // java/io/PrintStream
  #20 = NameAndType        #27:#28        // println:(Ljava/lang/String;)V
  #21 = Utf8               Week_01/Day_01
  #22 = Utf8               java/lang/Object
  #23 = Utf8               java/lang/System
  #24 = Utf8               out
  #25 = Utf8               Ljava/io/PrintStream;
  #26 = Utf8               java/io/PrintStream
  #27 = Utf8               println
  #28 = Utf8               (Ljava/lang/String;)V
{
  public Week_01.Day_01();
    descriptor: ()V
    flags: (0x0001) ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: return
      LineNumberTable:
        line 3: 0

  public static void main(java.lang.String[]);
    descriptor: ([Ljava/lang/String;)V
    flags: (0x0009) ACC_PUBLIC, ACC_STATIC
    Code:
      stack=2, locals=1, args_size=1
         0: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
         3: ldc           #3                  // String Hello Study!
         5: invokevirtual #4                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
         8: return
      LineNumberTable:
        line 5: 0
        line 6: 8
}
```

---

# **JVM**

> **Java Virtual Machine, .class 파일을 컴파일해 기계어로 번역한다.**

![JVM](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Ft6Hto%2FbtqP4mts8CN%2FVeqm01K6CxEcSkPJhmzDm1%2Fimg.png)

JVM은 다른 프로그램을 실행시키는 것이 목적인 프로그램이다.  
이에 두 가지 기본 기능이 존재하는데, Java 프로그램이 **어느 기기나 운영체제 상에서도 실행될 수 있게** 해준다. 또한, **프로그램 메모리를 관리하고 최적화**한다.(Garbage Collection)

### **JVM Components**

이렇게 JVM이 OS에 종속적이지 않으면서 메모리를 최적화하기 위해 세 가지의 구성요소가 함께 일한다.

1.  **Class Loader**
2.  Class Loader는 Bytecode로 구성된 .class 파일을 Runtime Data Areas에서 메모리를 할당 받기 위해 읽어 들인다.
3.  **Runtime Data Areas**
    - 각 스레드마다 할당 받는 영역
      - PC Register
      - JVM Stack
      - Native Method Stack
    - 모든 스레드가 공유하는 영역
      - Heap
      - Method Area
      - Runtime Constant Pool
4.  Runtime Data Areas(런타임 데이터 영역)은 JVM이 운영체제에 실행되면서 할당받는 메모리 영역이다. 해당 영역에는 6개로 나누어 메모리를 할당 받는다.
5.  **Execution Engine**  
    Class Loader를 통해 JVM 내의 Runtime Data Areas에 배치된 Bytecode는 Execution Engine에 의해 실행되며, Execution Engine은 Bytecode를 명령어 단위로 읽어서 실행한다.  
    이는 **Interpreter** 형식 혹은 **JIT Compiler**를 통해 실행하며, **Garbage** **Collection**을 통해 메모리 최적화를 진행한다.

- **Interpreter**
  - 메모리에 할당된 코드를 한 행씩 기계어로 번역한다.  
    실행 속도가 느리지만 코드 번역시 즉시 실행이 가능하며 테스트에 용이하다.
- **JIT Compiler**
  - Bytecode를 기계어로 번역하는 것은 Interpreter와 비슷하지만 캐싱 해놓은 같은 함수가 여러번 불려도 같은 기계어를 새롭게 생성하지 않고 기존의 기계어를 불러온다.
- **Garbage Collection**
  - Java에서는 프로그램 코드를 명시적으로 지정하여 해체하지 않는데, 이 때 Garbage Collection이 더 이상 필요하지 않은 객체를 지우는 역할을 한다. 이때, GC는 Heap 영역의 메모리만 다루며 Young / Old 영역으로 나누어 관리한다.

### **JIT Compiler**

- Bytecode를 기계어로 번역할 때 JIT Compiler를 사용한다.  
  JVM이 Interpreter를 이용해 Btyecode에서 기계어로 번역할 때, 반복되는 내용을 컴파일해서 사용한다. 이때, 최초 실행 시 많은 메모리와 시간이 소요되긴 하지만 최초 실행하면 재사용이 가능하다.  
  또한, 컴파일 없이 Interpreter만 하는 경우보다 반복적인 작업을 할 때 높은 효율을 낼 수 있다.

---

# **Runtime**

지금까지 Eclipse나 IntelliJ로 손쉽게 컴파일하면서 정작 원리에 대해서는 잊고 있었다. 이번 시간을 통해 직접 컴파일 해보자!

나는 미리 작성한 .java 파일을 이용해 컴파일 해볼 계획이다.터미널을 열고, 컴파일하고자 하는 .java 파일이 있는 경로로 이동한다.  
그 이후에 해당 경로에 파일이 존재하는지 확인한다.

```
 cd 경로/파일이름.java
 ls
```

![예시](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FE4LpL%2FbtqP6okbsaM%2F51z2wO34aTvUGA4Em6tIQK%2Fimg.png)

파일을 확인했다면 Java Compiler를 이용해 .java 파일을 Bytecode의 .class 파일로 컴파일한다.  
그 후, .class 파일이 생겼는지 확인한다.

```
 javac 파일이름.java
 ls
```

![예시](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FMnnTI%2FbtqQgzkoWly%2F12TjBf0T6KcoxpUjk03XTK%2Fimg.png)

.class 파일을 JVM으로 컴파일하기 위해서는 src 폴더로 이동해야 한다.  
`cd ..` 명령어를 입력하면 상위 경로로 이동하게 되며, src 폴더에서 패키지/파일 이름을 이용해 .class 파일을 컴파일한다.

```
 cd ..
 java 패키지/파일이름
```

![예시](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcQ8T7U%2FbtqP0tzNOnc%2FhJW5TFOiykSywJ63tjfum1%2Fimg.png)

---

# **JDK / JRE**

> Java Development Kit / Java Runtime Enviroment  
> **_JDK = JRE + @_**

## **JDK**

> 마치 외장하드나 USB 등 읽기/쓰기가 가능한 장치와도 같다.  
> 즉, JRE를 포함한 Compiler, Debuger 등 다양한 개발도구를 포함하는 프로그램이다.

## **JRE**

> 무료 PDF Reader처럼 읽기만 가능하다.  
> JVM, Java API, 기본 제공 라이브러리 등 Java로 만들어진 프로그램을 실행하기 위한 구성요소를 담고 있다.

---

# **Reference**

### **Java Compile Structure, JVM Components**

- [Naver D2](https://d2.naver.com/helloworld/1230)

### **Runtime**

- 백기선님 기술 블로그

[macOS 터미널 자바 컴파일 실행 방법](https://whitepaek.tistory.com/9)

- 컴파일에서 실행까지

[Back to the Essence](https://homoefficio.github.io/2019/01/31/Back-to-the-Essence-Java-%EC%BB%B4%ED%8C%8C%EC%9D%BC%EC%97%90%EC%84%9C-%EC%8B%A4%ED%96%89%EA%B9%8C%EC%A7%80-2/)

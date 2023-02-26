---
title: 'Primitive Type / Reference Type / Literal / Array'
date: '2020-12-15'
tags: ['Primitive Type', 'Reference Type', 'Literal', 'Array', 'Variable']
draft: false
summary: 'Primitive Type / Reference Type / Literal / Array 의 개념에 대해 이해한 내용을 정리 했습니다.'
---

# **Variable**

변수란 값을 저장할 수 있는 메모리의 특정 번지에 붙이는 이름이다.

Java의 변수는 **다양한 타입의 값을 저장할 수가 없다.**  
만약 int 타입으로 선언한 변수에 boolean 타입의 데이터를 저장할 수 없다.  
그래서 **정수 타입(`int,` `long`)에는 정수형 데이터**, **실수 타입(`float`, `double`)에는 실수형 데이터**만 들어갈 수 있다. 배열도 마찬가지이다.

## **Declaring Variables / Initializing Variables**

### **변수 선언**

1.  데이터 타입(기본형 / 참조형) 선언
2.  변수명 선언

    ```java
    // 하나의 변수만 선언하는 경우
    int num1;
    boolean check1;

    // 하나 이상의 변수를 선언하는 경우
    int num2, num3, num4;
    boolean check2, check3, check4;
    ```

- 변수를 선언하기 위해서는 변수명 선언 조건과 예약어에 대한 규칙을 파악하고 선언해야 한다.

| **작성 규칙**                                | **예시**                                                    |
| -------------------------------------------- | ----------------------------------------------------------- |
| 변수명 첫 번째 문자는 숫자로 시작할 수 없다. | 가능 : index, $hello, my_name 불가능 : 1_index, 10num, 5str |
| 영자는 대소문자를 구분한다.                  | 변수 str와 Str는 다른 변수이다.                             |
| Java 예약어는 사용할 수 없다.                | 예시 : private, print, public, int 등                       |
| 변수명의 길이는 제한은 없다.                 |                                                             |

### **변수 초기화**

변수 초기화는 변수를 선언할 때 실제 데이터를 정의하는 것을 의미한다.

초기화 하는 방법은 **대입 연산자(=)** 를 사용하여 선언한 변수에 실제 데이터를 저장하는 형식으로 구현한다.

```java
int num1 = 10;
int num2 = 20;

int sum = num1 + num2;
Integer num3 = new Integer();

System.out.println(sum); // 30
```

만약 초기화 되지 않고 선언만 된 변수를 연산을 진행하면 초기화되지 않은 변수가 있다고 예외를 발생시킨다.

```java
int num1 = 10;
int num2;

int sum = num1 + num2;
System.out.println(sum);

// 예외 발생
java: variable num2 might not have been initialized
```

## **Variable Scope / Lifetime**

> 변수는 선언되는 **위치, 접근 제한자에 따라 엑세스할 수 있는 범위**가 달라진다.  
> Java에서는 **모든 식별자가 정적으로 범위화되며 함수 호출 스택과는 독립적**이다.

### **변수 스코프**

1.  **멤버 변수 (Class Level Scope)**

    ```java
     public static class MemberVar {
         private int num1; // Seetter / Getter 메소드를 이용해서 초기화하고, 재사용
         public boolean check; // 해당 클래스 내에 어떤 메소드에서도 호출
         final private String talk = "Hello World!"; // 상수의 의미

         // method1 변수에서 접근제한자가 static으로 선언된 check를
         // true값으로 초기화한 후, 해당 값을 반환한다.
         boolean method1() {check = true; return check;}
     }
    ```

2.  클래스 내에 선언되는 변수로 접근 제한자를 통해 다양한 클래스, 메소드에서 초기화 가능하며 Setter, Getter 메소드를 이용해 초기화 가능하다.
3.  **지역 변수 (Method Level Scope)**

    ```java
     public static class MethodVar {
         int num1;

         public int method1() {
             int num1 = 1;
             int num2 = 2;
             int result = num1 + num2;
             return result;
         }

         public int method2() {
             num1 = 20;
     //  num1을 같은 이름의 새로운 변수로 선언하면 새로 선언된 변수의 데이터가 반환된다.
     //  int num2 = 10 + num1;
             return num2;
         }
     }
    ```

4.  메소드 내에서 선언된 변수로 해당 메소드 내에 동일한 이름의 멤버 변수가 사용되지 않는다면 같은 이름의 지역 변수를 선언할 수 있다. 하지만 본질적으로 멤버 변수와 지역 변수의 데이터가 저장된 메모리 주소는 같지 않다.

### **변수 라이프타임**

- 변수의 라이프타임은 해당 변수가 언제 초기화되는지 혹은 초기화, 선언된 위치가 어디에 존재하는지에 따라 달라진다.
- 변수의 라이프타임은 **Garbage Collection**과 연관이 깊다.
- **멤버 변수(인스턴스 변수**)의 경우,  
  클래스를 생성자를 통해 정의 했을 경우 프로그램을 종료할 때 까지 메모리 상에 존재한다.
- **지역 변수(로컬 변수)**의 경우,  
  정의된 클래스에 의해 메소드가 호출되어 실행된 후 종료되면 메모리 상에서 사라진다.

## **Type Inference / var Type**

> 타입 추론이란 코드 작성 당시, **데이터 타입이 정해지지 않았지만 컴파일러가 그 타입을 유추**하는 것이다.

타입 추론은 Java 10에서 추가된 이니셜라이저를 사용하는 로컬 변수의 유형 추론이다.

Java 9가지는 로컬 변수의 유형을 명시적으로 언급해야 했고, 로컬 변수를 초기화하는 데 사용되는 이니셜라이저와 호환되는지 확인해야 했다.

Java 10에서 타입 추론이 추가되면서 변수의 데이터 유형을 명시하지 않아도 된다. 하지만 변수를 인스턴스로 정의하고 컴파일러에게 어떠한 데이터 타입으로 인식하게 할 것인지 명시해주어야 한다.  
_~(마치 Javascript의 `var` 같다.)~_

**또한, 이러한 타입 추론은 이니셜라이저가 있는 로컬 변수에서만 사용할 수 있으며 멤버 변수(인스턴스 변수)나 메소드 파라미터, 리턴 유형 등에서 사용할 수 없다.**

```java
public class TypeInference_varType {
    public static void main(String[] args) {
        // 일반적으로 데이터 타입을 선언한 변수
        String str1 = "Good Bye, Java 9";

        // 데이터 타입을 선언하지 않은 var 타입 변수
        var str2 = "Hello, Java 10";
        System.out.println("str1은 문자열 타입 데이터인가? = " + str1 instanceof String);
        System.out.println("str1 = " + str1);
        System.out.println("str2은 문자열 타입 데이터인가? = " + str2 instanceof String);
        System.out.println("str2 = " + str2);

        // 일반적은 HashMap 선언
        Map<Integer, String> map1 = new HashMap<>();

                // var를 이용한 HashMap 선언
        var map2 = new HashMap<Integer, String>();
        map1.put(1, "신짱구");
        map2.put(1, "신짱아");

        System.out.println("map1 = " + map1.get(1));
        System.out.println("map2 = " + map2.get(1));
    }
}
```

## **Data Type Conversion**

### **Data Type Casting**

- 강제로 타입을 변환하는 것을 의미한다.
- 강제 타입 변환은 캐스팅 연산자 괄호()를 사용한다.

```java
int a = 10;
int b = 3;
// 이 때, a / b 연산의 값이 double 데이터 타입으로 변환되는 것은 아니다.
// a 변수 앞에 캐스팅 연산자 괄호가 있으니 a 변수를 double 타입의 데이터로 변환한다.
double c = (double) a / b;
```

### **Data Type Promotion**

- 자동으로 타입 변환이 일어나는 것을 의미한다.
- 자동 타입 변환은 값의 허용 범위가 작은 타입이 허용 범위가 큰 타입으로 저장될 때 발생한다.

```java
byte byteValue = 10;
int intValue = byte; // byte 타입 데이터가 자동으로 int로 변환

// float 타입 데이터가 long 타입 데이터와 연산하면서 long 타입으로 변환
double doubleValue = 1.4f + 0.2;
```

---

# **Primitive / Reference Type**

## **Primitive Type**

- 기본형 데이터 타입으로 총 8개 존재한다.
  - 정수형 데이터 타입 → byte, short, char, int, long
  - 실수형 데이터 타입 → float, double
  - 논리형 데이터 타입 → boolean
- 기본값을 가지는 데이터 타입으로 null이 존재하지 않는다.
- Generic을 이용할 때 필요한 Wrapper Class가 존재한다.
- 기본형 데이터 타입은 스레드의 Stack 메모리에 저장한다.

| **이름**   | **메모리 사용 크기** | **저장되는 값의 허용 범위**                             |
| ---------- | -------------------- | ------------------------------------------------------- |
| **byte**   | 1byte / 8bit         | \-128 ~ 127                                             |
| **short**  | 2byte / 16bit        | \-32,768 ~ 32,767                                       |
| **char**   | 2byte / 16bit        | 0 ~ 65535                                               |
| **int**    | 4byte / 32bit        | \-2,147,483,648 ~ 2,147,483,647                         |
| **long**   | 8byte / 64bit        | \-9,223,372,036,854,775,808 ~ 9,223,372,036,854,775,807 |
| **float**  | 4byte / 32bit        | (1.4 X 10의 -45제곱) ~ (3.4 X 10의 38제곱)              |
| **double** | 8byte / 64bit        | (4.9 X 10의 -324제곱) ~ (1.8 X 10의 308제곱)            |

## **Reference Type**

- 참조형 데이터 타입으로 기본형 데이터 타입을 제외한 모든 타입을 뜻한다.
- 최상위 클래스인 `java.lang.Object`를 상속 받는다.
- 참조형 데이터 타입(객체)는 Heap 메모리에 저장되며 HotSpot을 기준으로 아무런 데이터가 없는 객체라도 8byte를 차지한다.
- new, Reflection 등으로 객체를 생성한다.
- String은 char 타입의 Array 형태로, Array는 Reference Type이기 때문에 String도 참조형 데이터 타입이다.

---

# **Literal**

> Literal은 데이터 그 자체를 의미한다.

```java
int a = 1;
```

- a는 변수명, 1은 리터럴이다.  
  즉, 1과 같이 변하지 않는 데이터(boolean, char, double, long, int..)를 리터럴이라고 한다.
- 더 쉽게 설명하자면 변수나 상수는 메모리에 할당된 공간을 의미하는 반면, 리터럴은 공간에 저장된 데이터 자체를 의미한다.

## **정수 리터럴**

1.  2진수
    - 0b 또는 0B로 시작한다.
    - 0과 1로 구성된다.
2.  8진수
    - 0으로 시작하고 0 ~ 7 숫자로 구성된다.
3.  16진수
    - 0x 또는 0X로 시작한다.
    - 0 ~ 9, A ~ F 또는 a ~ f로 구성된다.

```java
// 2진수
int lit2 = 0b11010;
System.out.println(lit2);

// 8진수
int lit8 = 013;
System.out.println(lit8);

// 16진수
int lit16 = 0xB3;
System.out.println(lit16);
```

---

# **Array**

## **1차원 배열**

```java
// 1차원 배열
// 배열 길이만 선언 -> 데이터는 선언하지 않음
int[] arr1_1 = new int[5];

// 길이만 선언된 배열에 데이터 삽입
for (int i = 0; i < arr1_1.length; i++) {
    arr1_1[i] = i;
}

// 각 인덱스에 들어갈 데이터 초기화와 동시에 배열 선언
int[] arr1_2 = {0, 1, 2, 3, 4};
```

## **2차원 배열**

```java
// 각 인덱스에 들어갈 데이터 초기화와 동시에 배열 선언
int[] arr1_2 = {0, 1, 2, 3, 4};

// 2차원 배열
// 행과 열의 길이만 선언 -> 데이터는 선언하지 않음
int[][] arr2_1 = new int[5][5];

// 행과 열의 틀만 선언된 배열에 데이터 삽입
for (int i = 0; i < arr2_1.length; i++) {
    for (int j = 0; j < arr2_1[i].length; j++) {
        arr2_1[i][j] = j;
    }
}

// 각 행의 열에 데이터 초기화와 동시에 배열 선언
int[][] arr2_2 = {{0, 1, 2, 3, 4}, {5, 6, 7, 8, 9}, {10, 11, 12, 13, 14}, {15, 16, 17, 18, 19}, {20, 21, 22, 23, 24}};
```

---

# **References**

## **Variable / Primitive / Reference / Literal**

- 혼자 공부하는 자바

[혼자 공부하는 자바](https://www.hanbit.co.kr/store/books/look.php?p_code=B5635758676)

## **Type Inference**

- 타입 추론과 var 타입

[Java 10 LocalVariable Type-Inference](https://www.baeldung.com/java-10-local-variable-type-inference)

---
title: '[F-Lab] Reference Type'
date: '2021-05-08 00:00:04'
tags: ['F-Lab', 'Reference Type']
draft: false
summary: 'Reference Type 에 대해 학습한 내용을 정리 했습니다.'
---

> ✍️ 드디어 첫 멘토링을 시작했다! 여러 대화가 오갔지만, 가장 자신 없었던 참조 타입과 해당 타입이 메모리 상에서 어떻게 운용되는지 기록해보려고 한다. 왜냐면 멘토님께서 해당 질문을 주셨는데 대답을 잘못했기 때문이다. 그래서 다시 되짚어보는 시간을 가지려고 한다.

---

# **참조 자료형**

참조 자료형은 앞서 공부한 기본 자료형을 제외한 모든 자료형을 뜻한다. 다만 String 클래스의 경우 new 연산자를 사용하지 않고 Literal 형태로 변수값을 선언 할 수 있고, + 연산이 가능한 유일한 참조 자료형이다.

모든 참조 자료형은 **java.lang.Object 를 상속받는다.** 이러한 참조 자료형에는

1.  Annotation(@주석)
2.  Arrays(배열)
3.  Class
4.  Enum
5.  Interface

가 존재한다.

참조 자료형과 기본 자료형의 차이를 살펴보자.

[Java 8 Pocket Guide](https://www.oreilly.com/library/view/java-8-pocket/9781491901083/ch04.html)

[Reference VS Primitive](https://www.notion.so/21238a1c9c854d5fba720c5e5f37480f)

| Reference Type                                                                                                                                                                                                                                          | Primitive Type                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Unlimited number of reference types, as they are defined by the user. (기본 자료형을 제외한 모든 사용자 정의 자료형)                                                                                                                                    | Consists of boolean and numeric types: char, byte, short, int, long, float, and double.                                                                                                                                                                                                                                                                                                                                                                        |
| Memory location stores a reference to the data. (Stack 메모리에 해당 값을 저장하는 것이 아닌 Heap 메모리에 할당된 값의 주소를 참조하여 저장한다.)                                                                                                       | Memory location stores actual data held by the primitive type. (Stack 메모리에 기본 자료형의 값이 그대로 저장된다.)                                                                                                                                                                                                                                                                                                                                            |
| When a reference type is assigned to another reference type, both will point to the same object. (한 참조 객체가 다른 참조 객체에 할당되면, 둘 다 동일한 참조 객체를 바라본다.)                                                                         | When a value of a primitive is assigned to another variable of the same type, a copy is made. (한 기본 자료형이 다른 기본 자료형을 할당하면 같은 기본 자료형을 바라보는 것이 아니라 새로운 복사본이 메모리에 저장된다.)                                                                                                                                                                                                                                        |
| When an object is passed into a method, the called method can change the contents of the object passed to it but not the address of the object. (객체가 메소드에 전달되면 전달된 객체의 데이터는 메소드에서 변경 할 수 있지만, 주소는 변경 할 수 없다.) | When a primitive is passed into a method, only a copy of the primitive is passed. The called method does not have access to the original primitive value and therefore cannot change it. The called method can change the copied value. (기본 자료형이 메소드에 전달되면 기존의 기본 자료형을 호출한 메소드가 찾을 수 없기 때문에 값을 변경 할 수 없지만, 전달된 기본 자료형은 기존의 기본 자료형의 복사본이기 때문에 호출된 메소드에서 값을 변경 할 수 있다.) |

## **.hashCode()**

> Returns a hash code value for the object. This method is supported for the benefit of hash tables such as those provided by HashMap.

- Java 8 Reference Docs

해당 메소드는 java.lang.Object에 속하는 메소드로, 모든 참조 자료형은 Object를 상속 받으니 해당 메소드를 Override 할 수 있다. 또한, 해당 메소드 호출 시 객체의 해시코드를 int 타입으로 반환하는데 이 값은 메소드를 호출한 객체가 할당된 메모리 주소값을 해시화해서 반환한 값이다.

```java
public class HashCodeTest {

    public String desc;

    public HashCodeTest(String desc) {
        this.desc = desc;
    }

    public static void main(String[] args) {
        HashCodeTest test0 = new HashCodeTest("Constant Pool");
        HashCodeTest test1 = new HashCodeTest(new String("Heap Memory"));
        HashCodeTest test2 = test1;

        System.out.println("test0.hashCode() = " + test0.hashCode());
        System.out.println("test1.hashCode() = " + test1.hashCode());
        System.out.println("test2.hashCode() = " + test2.hashCode());
    }
}
/*
	test0.hashCode() = 1878246837
	test1.hashCode() = 929338653
	test2.hashCode() = 929338653
*/
```

코드를 통해 각 객체의 해시코드 값을 확인해보자. test0과 test1은 같은 HashCodeTest 클래스이지만 Heap Meory에는 각자 할당 받는다. test2의 경우 test1 객체를 할당 받아 초기화 되었으니 test1과 같은 객체를 참조하고 있다.

하지만, 이 .hashCode() 메소드는 String 클래스에 한해서 다르게 재정의 되어 있다.

```java
/**
     * Returns a hash code for this string. The hash code for a
     * {@code String} object is computed as
     * <blockquote><pre>
     * s[0]*31^(n-1) + s[1]*31^(n-2) + ... + s[n-1]
     * </pre></blockquote>
     * using {@code int} arithmetic, where {@code s[i]} is the
     * <i>i</i>th character of the string, {@code n} is the length of
     * the string, and {@code ^} indicates exponentiation.
     * (The hash value of the empty string is zero.)
     *
     * @return  a hash code value for this object.
     */
    public int hashCode() {
        int h = hash;
        if (h == 0 && value.length > 0) {
            char val[] = value;

            for (int i = 0; i < value.length; i++) {
                h = 31 * h + val[i];
            }
            hash = h;
        }
        return h;
    }
```

String 클래스에 재정의된 .hashCode() 코드 블럭이다. 살펴보면 문자열 한 글자 씩 가져와 각 정수와 아스키 코드를 더한 최종값을 반환한다. 때문에 String 클래스의 경우 중복되는 해시코드가 존재 할 수 있다. 해당 경우는 문자열이 같을 때 존재한다.

```java
public class StringHashCodeTest {
    public static void main(String[] args) {
        String test0 = "abc";
        String test1 = "abc";
        String test2 = "가나다";

        System.out.println("test0.hashCode() = " + test0.hashCode());
        System.out.println("test1.hashCode() = " + test1.hashCode());
        System.out.println("test2.hashCode() = " + test2.hashCode());

        String test3 = new String("def");
        String test4 = new String("def");

        System.out.println("test3.hashCode() = " + test3.hashCode());
        System.out.println("test4.hashCode() = " + test4.hashCode());
    }
}
/*
	test0.hashCode() = 96354
	test1.hashCode() = 96354
	test2.hashCode() = 43761996
	test3.hashCode() = 99333
	test4.hashCode() = 99333
*/
```

위의 코드를 보면 알 수 있듯, test3과 test4는 Heap Memory에 각각 다르게 할당되었는데 hashCode의 값이 같은 것을 확인 할 수 있다. 이는 두 객체의 값(문자열)이 같기 때문이다.

## **.equals()**

해당 메소드 또한 java.lang.Object에 속한 메소드로, 모든 참조 차료형은 해당 메소드를 상속받아 재정의 가능하다. 때문에 각 참조 자료형에서 어떻게 정의하느냐에 따라 기준이 달라진다.

.equals() 메소드와 == 연산자는 서로 비슷하지만 다르다. == 연산자는 기본 자료형 간 연산을 수행 할 때는 값을 비교하지만, 참조 자료형 간 연산을 수행 할 때는 참조 자료형의 주소값을 비교한다. String 클래스에서 .equals() 메소드는 한 글자 씩 나누어 비교한 후 문자열 값이 같은지 아닌지 논리값을 반환한다. 이를 어떤 클래스에서는 이름이 같으면 같은 객체로 볼 수 있게끔 정의 내릴 수도 있다.

## **String**

> The String class represents character strings. All string literals in Java programs, such as "abc", are implemented as instances of this class. Strings are constant; their values cannot be changed after they are created. String buffers support mutable strings. Because String objects are immutable they can be shared.

앞서 설명한 것 처럼 String 클래스는 두 가지 방식으로 선언 할 수 있다.

- new 연산자를 이용한 선언 → Heap Memory 영역에 할당

```java
String str0 = new String("abcd");
String str1 = new String("abcd");
```

- Literal을 이용한 선언 → Constant Pool(상수 풀) 영역에 할당

```java
String str2 = "abcd";
String str3 = "abcd";
```

이 두 가지 방식을 통해 중복되는 문자열 4개를 변수로 선언했다. 하지만 메모리 상에 4개의 문자열이 모두 할당될까? HashCode를 통해 알아보자.

```java
public static void main(String[] args) {
        String str0 = new String("abcd");
        String str1 = new String("abcd");
        String str2 = "abcd";
        String str3 = "abcd";

        System.out.println("str0.hashCode() = " + System.identityHashCode(str0));
        System.out.println("str1.hashCode() = " + System.identityHashCode(str1));
        System.out.println("str2.hashCode() = " + System.identityHashCode(str2));
        System.out.println("str3.hashCode() = " + System.identityHashCode(str3));
    }

/*
	str0 = 1163157884
	str1 = 1956725890
	str2 = 356573597
	str3 = 356573597
*/
```

위 코드를 보면 메모리 상에 3개의 데이터만 할당된 것을 알 수 있다. 그 이유는 Literal로 선언된 String 변수는 Constant Pool에 할당되며, String 클래스는 내부적으로 intern() 메소드를 실행해서 가장 먼저 Constant Pool에 해당 String 값을 찾아본 후 해당하는 값이 없다면 Constant Pool에 객체를 생성하고, 그 주소값을 반환한다.

이 때, str0 변수와 str1 변수가 값은 같은데 왜 각기 다른 객체 주소를 할당 받았을까? 그 이유는 두 변수가 new 생성자를 통해 선언된 객체이기 때문에 Constant Pool이 아닌 Heap Memory 에 각각 할당 되었기 때문이다.

### **.intern()**

해당 메소드는 앞서 언급한 것 처럼 Constant Pool에 해당하는 문자열을 찾는다. 해당 메소드를 사용하는 코드로 확인해보자.

```java
public void stringInternMethod() {
        String strA = new String("stringA");
        String strAa = new String("stringA");

        String strB = "stringA";
        String strBa = "stringA";
        String strBb = strB.intern();

        String strC = new String("stringB");
        String strCa = strC.intern();
        String strCb = strC.intern();

        System.out.println("strA = " + System.identityHashCode(strA));
        System.out.println("strAa = " + System.identityHashCode(strAa));

        System.out.println("strB = " + System.identityHashCode(strB));
        System.out.println("strBa = " + System.identityHashCode(strBa));
        System.out.println("strBb = " + System.identityHashCode(strBb));

        System.out.println("strC = " + System.identityHashCode(strC));
        System.out.println("strCa = " + System.identityHashCode(strCa));
        System.out.println("strCb = " + System.identityHashCode(strCb));
    }
/*
	strA = 1163157884
	strAa = 1956725890

	strB = 356573597
	strBa = 356573597
	strBb = 356573597

	strC = 1735600054
	strCa = 21685669
	strCb = 21685669
*/
```

위의 코드를 보면 strA 와 strAa 는 같은 값이지만 다른 주소를 출력하는 걸 확인 할 수 있는데, 이는 두 변수가 new 생성자로 선언되어 Heap Memory에 각각 할당되었기 때문이다.

반면, strB 관련 변수들을 살펴보면 strB 변수가 Literal 로 선언되어 해당 값을 Constant Pool에 할당한다. 그 후 strBa 병수가 Literal로 선언될 때 String 클래스 내부적으로 intern()이 호출되어 Constant Pool 에서 해당 값을 먼저 찾는다. 이 때 먼저 할당된 strB의 값이 존재하기 때문에 strBa는 해당 주소를 참조한다. strBb 변수 또한 intern() 메소드를 통해 해당 값에 대한 주소를 참조한다.

strC 관련 변수들을 살펴보면 strC는 new 생성자로 선언되었기 때문에 Heap Memory에 할당되어 Constant Pool에 존재하지 않는다. 때문에 strCa 변수는 intern() 메소드가 Constant Pool에서 해당 값을 찾다가 존재하지 않는 것을 확인하고 Constant Pool에 해당 값을 새로 할당하고 값의 주소를 strCa 변수가 참조한다. strCb 변수의 경우, strCa 변수가 선언되면서 Constant Pool에 만들어 놓은 값을 찾고 해당 값의 주소를 참조한다.

과거 Java 7 이전에는 Perm 영역에서 Constant Pool이 관리되었는데, 메모리 확장 불가로 인한 OOM 발생으로 Java 7 이후로 Heap Memory에서 Constant Pool을 관리하도록 변경되었다. 즉, 문자열이 GC의 영역에 포함되었다는 의미이다.

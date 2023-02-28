---
title: '[F-Lab] psvm'
date: '2021-05-06 00:00:00'
tags: ['F-Lab', 'private', 'static', 'viod', 'main']
draft: false
summary: 'Java 의 private static void main 에 대해 학습한 내용을 정리 했습니다.'
---

# 처음부터 찬찬히

F-Lab 멘토링 시작 전, 자바의 신 예습 시간을 가지기로 했다. 오늘은 그 대망의 첫 날이다. 처음 혼공자로 Java 독학 했을 때의 마음가짐으로 산뜻하게 시작해보자!

- Java Version : Java 11 OpenJDK
- OS : Mac OS Big Sur 11.1
- Utils : iTerm, oh-my-zsh, vi, Sublime Text 등

---

# public static void main(String\[\] args) {}

나는 지금까지 개발을 하면서 MSG 같은 IDE의 맛에 푹 빠져 가장 기초적인 Java가 동작하는 원리, 심지어 콘솔에서 어떻게 Java 파일을 컴파일 해야 하는지 잊고 살았다. 이번 기회를 통해 IDE를 사용하지 않고 Java 파일을 만들고, 명령어로 직접 해당 파일을 실행해보고자 한다.

책에서는 Windows를 기준으로 설명하고 있는데, 나는 Mac OS를 사용하고 있어 명령어에 대해 혼란이 조금 있었다. 터미널에서 직접 Java 파일을 만들고 싶었는데 copy con 명령어를 자꾸 찾을 수 없다고 뜨길래 왜 그럴까 검색을 해봤더니, touch 파일명.확장자 를 사용하면 됐었고, vi 를 이용해서도 파일을 생성 할 수 있었다. 이 정도로 모르다니 정말 심각한 수준이라고 할 수 있다.

나는 vi 에디터를 이용해서 Java 파일을 만들었는데, 회사에서 종종 vi 에디터를 사용하는 경우가 있어 조금은 익숙했다. 하지만 정말 간만에 만들어보는 **main()** 메소드라 그런지 접근제어자 철자도 틀리고, 파라미터에 String 배열이 들어가는 것도 까먹는 불상사가 발생했다.

일단, Java 언어에서 가장 작은 단위는 Class 라는 것을 알고, 메소드는 Class 내에 선언되어야 한다는 것을 안다는 가정 하에 문득 이런 의문이 들었다.

**특정 Java 파일을 실행하기 위해서 해당 파일에 main()가 필수적으로 들어가야 하는데, 왜 그런걸까?**

- **접근 제어자**
  - **public**
    - **main()** 메소드는 다른 클래스에 존재하는 메소드 등을 호출해야 하는데, 만약 **main()** 메소드가 public 제어자가 아닌 private 혹은 final 등으로 선언되어 있다면 접근이 불가능하기 때문에 **main()** 함수는 public 접근 제어자를 선언한다.
  - **static**
    - static 제어자는 프로그램이 실행될 때, 따로 인스턴스화 하지 않아도 (새롭게 메모리에 할당하지 않아도) method 영역 메모리에 호출된다. 이렇게 static 제어자가 선언된 클래스, 메소드 등은 프로그램이 종료될 때까지 메모리에 남아 있게 된다. **즉, GC에 의해 메모리에서 사라지지 않게 된다는 의미이다.**
    - public 부분에서 설명된 것 처럼, **main()** 메소드는 다른 클래스의 메소드를 호출해야 하기 때문에 중간에 메모리에서 사라지면 안된다. 그러한 이유 때문에 static 제어자를 사용한다.
- **리턴 타입**
  - **void**
    - 만약 **main()** 메소드의 리턴 타입이 존재한다면 프로그램이 종료될 때 특정 데이터를 반환하겠다는 의미인데, 이 것은 **main()** 메소드의 의미에 부합하지 않다. 즉, 프로그램이 실행되는 시점에만 메세지를 주고 받기 때문에 프로그램이 종료된 이후 데이터를 반환 받는 것은 의미가 없다.
- **파라미터**
  - **String\[\] args**
    - public 과 static 제어자 선언 이유와 연결되는 의미로, 외부에서 값을 받아오기 위한 파라미터이다. 이 때, 외부에서 받아오는 값을 옵션이라고 하는데, 해당 옵션들을 String 배열에 넣어 프로그램을 실행한다.

[정적 메소드, 너 써도 될까?](https://woowacourse.github.io/javable/post/2020-07-16-static-method/)

![예시](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc5WlEv%2Fbtq4jDHhnhV%2F6M9q0zJqQXJk2iLhv2t8WK%2Fimg.png)

![결과](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcTe2Sz%2Fbtq4kUayods%2FzryyD5mAqWK4IzDWapU71k%2Fimg.png)

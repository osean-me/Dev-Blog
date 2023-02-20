---
title: '스택 수열'
date: '2020-10-16'
tags: ['백준 1874', '알고리즘', '코딩 테스트', 'Stack']
draft: false
summary: '백준 1874 문제를 풀고 이해한 내용을 정리 했습니다.'
---

# 진짜 모르겠다. 너무 어렵다.

## 어떻게든 풀어 보겠다고 안간힘을 쓴 흔적.

- 말아먹을 내 코드

```java
package 심성헌.알고리즘_2주차;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.List;

public class 스택수열_1874 {
  static public BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
  static public int[] input(int n) throws NumberFormatException, IOException {
    int[] input = new int[n];
    for (int i = 0; i < n; i++) {
      input[i] = Integer.parseInt(br.readLine());
    }
    return input;
  }

  static public void stack(int[] input) {
    int subPlus = 0;
    List<Integer> list = new ArrayList<Integer>();

    for (int i = 1; i <= input.length; i++) {
      if (list.isEmpty() || !list.get(list.size() - 1).equals(input[subPlus])) {
         list.add(i); System.out.println("+");
      } else {
        list.remove(list.size() - 1);
        subPlus++;
        System.out.println("-");
      }

      for (int k = 0; k < list.size(); k++) {
        System.out.print(list.get(k) + " ");
      }

      System.out.println(" / list.size() = " + list.size());
    }
  }

  public static void main(String[] args) throws NumberFormatException, IOException {
    int[] input = input(Integer.parseInt(br.readLine()));
    stack(input);
  }
}
```

또 다시 다른 사람 코드를 참고했다. 미쳤다고 생각한다. 정말로.  
처음에는 stack에 대해서 이해는 했지만 문제 자체를 어떻게 풀어야할 지 몰라서 앞이 캄캄했다. 그래도 문제가 요구하는 것 그대로 그렸더니 어느정도 감은 잡혔다. 하지만 여기서 큰 문제가 발생했다. 나는 이 문제를 ArrayList 클래스를 이용해서 어떻게든 풀어내려고 했지만 난 할 수 없었다.(😇)

1.  **Stack 클래스의 존재를 몰랐던 점.**
2.  **다중 for문을 사용하고 싶지 않은 이상한 고집.**
3.  **등등...**

정말 1, 2번의 비중이 나를 화나게 하는 큰 요인이라고 생각한다.

- **_어떻게든 풀어보려고 노력한 나의 흔적들.._**

![풀이](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcbLyOZ%2FbtqK7zwjs6d%2FvQnBtMH9gzg3ESzU6aWkVk%2Fimg.jpg)

코드와 필기를 보면 대충 느낌이 오겠지만,  
Stack 클래스 존재 유무도 몰랐던 바보 심성헌은 `push, pop`을 `ArrayList.remove()와 for문`을 통해서 해결하려고 했다.(🤮)

## Stack 클래스

문제를 풀 수 없다는 열등감에 지고 말았다. 그래서 구글에 검색했다.  
이 때 Stack 클래스를 알게 되었고, 해당 클래스에는 `push(), pop(). peek()` 메소드가 존재하고 많은 사람들이 위의 메소드를 사용한다.

- **Stack.push(n) → Stack에 n의 데이터를 삽입**
- **Stack.pop() → Stack의 마지막 데이터를 삭제**
- **Stack.peek() → Stack의 마지막 데이터 조회**

다른 사람들의 코드를 조금 참고했지만, 어쨌든 내 손으로 풀었다고 생각하고 싶다.(착각의 늪)

- **_Stack 클래스를 이용한 코드_**

```java
package 심성헌.알고리즘_2주차;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Stack;

public class 스택수열_1874 {
   public static void main(String[] args) throws NumberFormatException, IOException {

     BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
     StringBuffer sb = new StringBuffer();
     Stack<Integer> stack = new Stack<Integer>();

     int n = Integer.parseInt(br.readLine());
     int[] arr = new int[n]; int m = 0;

     for (int i = 0; i < n; i++) {
      arr[i] = Integer.parseInt(br.readLine());
      stack.push(i + 1); sb.append("+ \n");

      while (!stack.isEmpty() && stack.peek() == arr[m]) {
        stack.pop();
        sb.append("- \n");
        m++;
      }
    }

    if (!stack.isEmpty()) {
      System.out.println("NO");
    } else {
      System.out.println(sb.toString());
    }
  }
}
```

위에 끄적거린 필기와 나의 고집을 이겨내려는 마음으로 코드를 짰다.  
먼저 데이터 입력은 BufferReader 클래스를 사용했고, + / - 를 출력하기 위한 문자열 클래스는 StringBuffer 클래스를 사용했는데 StringBuilder 클래스를 사용하는 사람도 많았다.

비록 다중 반복문을 통해 해결했지만, 다중 반복문을 사용하지 않으면 해결할 수 없는 문제라고 생각했다. 처음에는 1 ~ n까지 데이터를 입력 받는 것도 또 다른 for문을 선언해서 입력했다. 하지만 BufferReader 클래스를 사용하면서 중복되는 반복문은 줄일 수 있었다.

for문을 시작하면서 수열로 등록할 변수에 데이터를 입력하고, stack에 `i + 1`를 삽입했다. 그렇게 되면 `i` 가 커지면서 stack에 쌓이게 될 것이다.

i = 0 / stack.get(0) = i + 1 = 1  
i = 1 / stack.get(1) = i + 1 = 2  
i = 2 / stack.get(2) = i + 1 = 3

여기서 중요한 점은 수열에 입력한 데이터와 stack의 마지막 데이터가 같으면 stack의 마지막 데이터는 pop이 되어야 한다. 근데 이러한 경우가 **한 번만 발생하는 것이 아니라 연속으로 발생할 수 있다는 경우도 생각해야 한다!**

위와 같이 연속으로 pop()을 실행해야 하는 경우도 고려해야 하기 때문에, 논리 연산을 통한 반복을 수행하는 while문을 사용해서 문제를 해결하려고 했다.

> pop()를 실행하기 위한 조건
>
> 1.현재(i) 입력한 수열의 값과 stack.peek()의 값이 같다면!  
> 2.stack이 비어 있지 않다면!

처음에는 stack이 비어 있는 경우를 while문의 조건으로 넣지 않았는데, 그렇게되면 stack.peek()과 수열의 값을 비교할 수 없어서 에러가 발생한다.  
(stack이 비어 있기 때문에 peek()을 실행할 수 없다.)

또 한 가지 더 중요한 것은 수열의 인덱스 번호가 `i`가 되면 안된다. 수열의 경우는 pop()이 실행되는 경우(즉, while문이 실행 되면서 조건이 `true`인 경우)에만 인덱스 번호가 1씩 증가해야 한다.  
그렇기 때문에 나는 수열을 위한 인덱스 번호를 별도로 선언했다.

(`int m = 0;`)

이렇게 풀면 문제 조건에 맞게 정상적으로 해결이 된다.  
이제 마지막으로 stack이 비어 있지 않은 경우에 "NO"라는 문자열을 출력해야 하는데, 여기서 해당 문자열을 출력하는 조건이 stack이 비어 있지 않은 경우란 pop()을 수행하기 위한 조건을 연산할 때 수열에 입력된 값이 -1 씩 감소하지 않을 때를 의미한다.  
for문의 `i`는 1씩 증가하며, stack에서도 1씩 증가한 `i`를 push한다. 그런데 전에 수행된 반복문에서 pop된 데이터보다 큰 데이터를 다시 pop 조건에서 검사할 수 없다.(맞게 말을 했는지 모르겠다.)

왜냐하면 위의 경우에 검사하지 못한 데이터가 stack에 남아 있기 때문에 **입력한 수열 데이터만큼 수행하지 못했다는 의미가 되기 때문이다.**

이 부분을 조건문으로 해결하면 해당 문제는 **맞았습니다!** 를 볼 수 있을 것이다.

시간이 너무 늦었다. 이제 잘거다. 안녕!

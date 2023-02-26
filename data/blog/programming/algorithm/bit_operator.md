---
title: '비트 연산 및 X 와 K'
date: '2020-11-25'
tags: ['백준 1322', 알고리즘', '코딩 테스트', '비트 연산']
draft: false
summary: '백준 1322 문제를 풀고 대해 이해한 내용을 정리 했습니다.'
---

# CT 부셔보기

이번 문제도 SSAFY를 준비하며 추천 받은 문제여서 풀게 되었다.  
해당 문제는 비트 연산을 응용해 해결해야 하는 문제인데, 지금까지 비트 연산이라는 말을 들어만 봤지 정확히 무엇인지 공부하지 못했다.

비트 연산은 조건문을 사용하면서 비교 연산자로 사용하던 `&` 나 `|` 등을 활용해 비트(bit, 2진수로 이루어진 데이터 단위)를 연산하는 것을 의미한다.

먼저 비트 연산에 대해 공부하고 백준 문제로 넘어가도록 하자!

# 비트 연산

[참고 아티클](http://www.tcpschool.com/c/c_operator_bitwise)

처음 독학을 하며 비교 연산자에 대해 공부할 때 단순히 조건을 검사하는 용도로만 생각했다. 하지만 비교 연산자는 생전 듣지도 보지도 못한 명제 논리에 의한 논리적 추론과 진리표에 기인해 만들어진 것이었다.

~**_(중등 수학부터 수학을 포기한 유명한 수포자다.)_**~

[논리적 추론, 논리연산자,진리표](https://luv-n-interest.tistory.com/345)

그렇기 때문에 이러한 논리 연산자를 이용해 비트 연산을 수행할 수 있는데, 정확한 의미를 파악하지 못했지만 그래도 용도에 대해서는 파악하고 있어서 이해하는데 큰 어려움은 없었다.

N과 M의 이진수가 있다고 가정 했을 때, 두 이진수의 `i` 번째의 값이 서로 `1`일 경우에 `&` 연산자는 `1`을 반환하고, 둘 중 하나만 `1`일 경우에 `|` 연산자도 `1`을 반환한다.

즉, `&` 연산자의 경우 논리곱으로서 N과 M의 i 번째 값이 `참`이어야 결과도 `참`이다.  
반면 `|` 연산자의 경우 논리합으로서 N과 M의 i 번째 값 모두가 `거짓`이어야 결과도 `거짓`이 된다.

![AND 연산](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbb0tWv%2FbtqOdbzjGJK%2FYhLhiiNPqWuGoOaKxX8nok%2Fimg.png)

![OR 연산](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbWohhc%2FbtqN8YgDBvq%2FcW3ljzcpw4WUKJYBobUHqK%2Fimg.png)

이처럼 `^` 연산자의 경우 i 번째 값이 다를 경우 `1` 을 반환하고, `~` 연산자의 경우 N의 i 번째 값이 `1`일 때는 `0`을 `0`일 때는 `1`을 반환한다.이를 이해하는데 큰 어려움은 없었지만

![XOR 연산](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcmg9lI%2FbtqOeBK3W7U%2FLeTziAtRnv1Shh8R5HR6Zk%2Fimg.png)

![NOT 연산](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbv77N3%2FbtqN8f31c0Z%2FSQw3jKZLKQlRfKCtVMAPw0%2Fimg.png)

비트 연산을 이해하는데 큰 어려움은 없었지만 논리곱, 논리합 등 논리에 대한 영역이 이해가 되지 않았다. 앞서 말한 것 처럼 나에게 수학은 남의 나라 이야기이기 때문에 익숙하지 않았다.

[SW Expert Academy](https://swexpertacademy.com/main/learn/course/subjectList.do?courseId=AVuPCwCKAAPw5UW6)

[삼성 SW Expert Academy](https://swexpertacademy.com/main/learn/course/subjectList.do?courseId=AVuPCwCKAAPw5UW6)에서 논리적 사고, Computational Thinking에 대한 교육 영상을 참고해 논리적 추론에 대해 조금이나마 공부할 수 있었다.

# X와 K

[1322번: X와 K](https://www.acmicpc.net/problem/1322)

해당 문제는 자연수 X와 K가 주어지며, `X + Y` 와 `X | Y` 가 같을 수 있는 K 번째 자연수를 출력해야 하는 문제이다.

먼저 해당 문제를 해결하기 위해서는 Java에서 제공하는 `Math.pow()` 메소드를 알아두면 편하다.

- **Math.pow()**

## X, Y를 이진수로 변환 및 비트 연산

> K = 2 (2 번째 Y 값을 구했을 때)  
> X = 5 → 101  
> Y = 2 → 10  
> 5 + 2 == 101 | 10 → 7 == 111
>
> K = 20 (20 번째 Y 값을 구했을 때)  
> X = 5 → 101  
> Y = 80 → 110000  
> 5 + 80 == 101 | 1010000 → 85 == 1010101

- `X + Y == X | Y` 를 연산 하려면 `X`를 이진수로 변환 했을 때, 0인 인덱스가 1인 `Y`가 위 조건에 해당한다.
  - `X`를 이진수로 변환 했을 때, `X`의 i 인덱스 값($2^n$)가 `K`보다 작은 경우만큼 `X`의 이진수 값에서 0인 부분을 찾아야 한다.
  - 이는 `Y`가 `X`보다 큰 경우도 포함한다.
- `X`에서 0인 자리가 1인 자연수($2^n$)를 찾고, 해당 자연수와 `X`를 비트 연산을 했을 때 `X`와 값이 다르다면 자연수를 리스트 혹은 배열에 저장한다.
  - 위의 리스트 혹은 배열의 길이를 `m` 이라고 가정하고 $2^m$의 값이 `K`보다 작을 때 만큼 반복하여 `X`의 0인 자릿수가 1인 자연수 모두를 리스트 혹은 저장한다.
- `m` 만큼 다시 반복하여 리스트 혹은 배열에서 가장 위에 존재하는 `Y`(저장된 자연수)를 꺼내어 결과값 0과 계속 더한다.
  - 또한, `K`에 $2^i$를 빼서 `X + Y == X | Y` 조건에 부합하지 않는 `Y` 값은 제외한다.
  - 이 때, `i`는 반복하며 감소하는 인스턴스 변수 `i`이다.

![풀이](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbKR697%2FbtqN8YOvmsP%2FlfCFWKkxpvqOe4OUt4vgs0%2Fimg.png)

## 코드

[참고 아티클](https://algwang.tistory.com/58)

```java
package 심성헌.알고리즘_5주차.백준;

import java.io.*;
import java.util.*;

public class X와K_1322 {
    static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    static BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

    public static void main(String[] args) throws Exception {
        String[] input = br.readLine().split(" ");
        long x = Long.parseLong(input[0]);
        long k = Long.parseLong(input[1]);
        long save = 1;
        long result = 0;
        ArrayList<Long> arr = new ArrayList<Long>();

        while ((long) Math.pow(2, arr.size()) <= k) {
            System.out.print(arr.size() + " ");
            System.out.println(save);
            if ((x | save) != x) {
                arr.add(save);
            }
            save *= 2;
        }

        for (int i = 0; i < arr.size(); i++) {
            System.out.print(arr.get(i) + " ");
        }
        System.out.println();
        for (int i = arr.size() - 1; i >= 0; i--) {
            if (k == 0) {
                break;
            } else {
                if ((long) Math.pow(2, i) <= k) {
                    result += arr.get(i);
                    System.out.println("result = " + result);
                    k -= (long) Math.pow(2, i);
                }
            }
        }
        bw.write(result + "");
        bw.flush();
        bw.close();
    }
}
```

이번 문제는 위 블로거 분의 코드를 참고했다.  
손으로 직접 푼다면 노가다로 해결할 수 있을 것 같은데, 이걸 코드로 풀어 나가는 것이 정말 어려웠다.  
가장 이해하고 접근하기 어려웠던 부분은 X + Y 와 X | Y 가 같다는 걸 어떻게 수학적으로 증명해야 할지 감이 잡히지 않아서 이해하는데 굉장히 오랜 시간이 소요됐다.**_~(참고로 해당 문제는 백준 골드4 레벨이다.)~_**  
그래도 이번 문제를 통해 비트 연산은 무엇인지, Math.pow() 메소드가 어떤 용도인지, 왜 이렇게 코드가 구성 되었는지 깊게 이해할 수 있었던 시간이었다.

아직 가야 할 길이 멀지만 하나씩 배워서 내 것이 되는 이 과정이 참 재미있다.  
재미있는 만큼 잘 할 수 있도록 더 노력 해야겠다!

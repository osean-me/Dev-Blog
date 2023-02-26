---
title: '삼진법 뒤집기'
date: '2020-12-11'
tags: ['프로그래머스', '알고리즘', '코딩 테스트', '삼진법']
draft: false
summary: '프로그래머스의 삼진법 뒤집기를 풀고 이해한 내용을 정리 했습니다.'
---

# 문제

[코딩테스트 연습 - 3진법 뒤집기](https://programmers.co.kr/learn/courses/30/lessons/68935)

### **문제 설명**

자연수 n이 매개변수로 주어집니다. n을 3진법 상에서 앞뒤로 뒤집은 후, 이를 다시 10진법으로 표현한 수를 return 하도록 solution 함수를 완성해주세요.

---

### 제한사항

- n은 1 이상 100,000,000 이하인 자연수입니다.

---

### 입출력 예

| **n** | **result** |
| ----- | ---------- |
| 45    | 7          |
| 125   | 229        |

---

### 입출력 예 설명

입출력 예 #1

- 답을 도출하는 과정은 다음과 같습니다.

| **n(10진법)** | **n(3진법)** | **앞뒤 반전(3진법)** | **10진법** |
| ------------- | ------------ | -------------------- | ---------- |
| 45            | 1200         | 21                   | 7          |

- 따라서 7을 return 해야 합니다.

입출력 예 #2

- 답을 도출하는 과정은 다음과 같습니다.

| **n(10진법)** | **n(3진법)** | **앞뒤 반전(3진법)** | **10진법** |
| ------------- | ------------ | -------------------- | ---------- |
| 125           | 11122        | 22111                | 229        |

- 따라서 229를 return 해야 합니다.

# 풀이

## 설명

- 진법 계산을 아예 모르던 내가 SSAFY의 CT를 공부했던 덕인지 해당 문제는 조금 수월하게 풀 수 있었다.
- 예전에 알고리즘 공부하면서 `Math.pow()` 메소드를 공부하게 됐고, 그 덕분에 제곱 계산도 수월하게 해결 할 수 있었다.
- 변수 `tmp`가 0이 되기 전까지 반복하여 `n`의 3진수를 구한다.
  - 이 때, 나는 `List`가 쓰기 싫어서 `StringBuffer`를 이용했다.
- `StringBuffer`에 3진수의 각 자리의 값이 역순으로 쌓이기 때문에 추가적으로 변환한 3진수를 뒤집는 과정은 생략했다.
- `score` 변수를 선언하고, 해당 변수에 `StringBuffer`의 `subString`와 `Math.pow()`를 사용해 값을 더하면 10진법의 값이 된다.

## 코드

```java
package 심성헌.알고리즘_6주차;

public class 삼진법뒤집기_프로그래머스 {
    public static void main(String[] args) {
        int n = 125;
        int result = solution(n);

        System.out.println(result);
    }

    public static int solution(int n) {
        int tmp = n;
        StringBuffer sb = new StringBuffer();
        while (tmp > 0) {
            int remain = tmp % 3;
            sb.append(remain);
            tmp /= 3;
        }
        int score = 0;
        int x = sb.length() - 1;
        for (int i = 0; i < sb.length(); i++) {
            score += Integer.parseInt(sb.toString().substring(i, i + 1)) * Math.pow(3, x);
            x--;
        }
        return score;
    }
}
```

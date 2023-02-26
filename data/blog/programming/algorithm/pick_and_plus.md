---
title: '두 개 뽑아서 더하기 / 완전 탐색 / HashSet'
date: '2020-12-11'
tags: ['프로그래머스', '알고리즘', '코딩 테스트']
draft: false
summary: '프로그래머스 문제를 풀고 이에 필요한 완전 탐색, HashSet 에 대해 이해한 내용을 정리 했습니다.'
---

# 들어가기 전에

프로그래머스 기존의 아이디를 탈퇴하고 다시 시작하는 마음으로 새로 만들었다.

그래서 알고리즘 레벨 1부터 차근히 풀어보고 기록을 남기려고 한다!

# 문제

[코딩테스트 연습 - 두 개 뽑아서 더하기](https://programmers.co.kr/learn/courses/30/lessons/68644)

### **문제 설명**

정수 배열 numbers가 주어집니다. numbers에서 서로 다른 인덱스에 있는 두 개의 수를 뽑아 더해서 만들 수 있는 모든 수를 배열에 오름차순으로 담아 return 하도록 solution 함수를 완성해주세요.

---

### 제한사항

- numbers의 길이는 2 이상 100 이하입니다.
  - numbers의 모든 수는 0 이상 100 이하입니다.

---

### 입출력 예

| numbers       | return          |
| ------------- | --------------- |
| \[2,1,3,4,1\] | \[2,3,4,5,6,7\] |
| \[5,0,2,7\]   | \[2,5,7,9,12\]  |

---

### 입출력 예 설명

입출력 예 #1

- 2 = 1 + 1 입니다. (1이 numbers에 두 개 있습니다.)
- 3 = 2 + 1 입니다.
- 4 = 1 + 3 입니다.
- 5 = 1 + 4 = 2 + 3 입니다.
- 6 = 2 + 4 입니다.
- 7 = 3 + 4 입니다.
- 따라서 `[2,3,4,5,6,7]` 을 return 해야 합니다.

입출력 예 #2

- 2 = 0 + 2 입니다.
- 5 = 5 + 0 입니다.
- 7 = 0 + 7 = 5 + 2 입니다.
- 9 = 2 + 7 입니다.
- 12 = 5 + 7 입니다.
- 따라서 `[2,5,7,9,12]` 를 return 해야 합니다.

# 풀이

## 설명

1. 해당 문제는 입출력 예시처럼 배열에 존재하는 모든 요소를 탐색하여 더한 값을 출력 해야 한다.
2. 단, 더한 값이 중복으로 존재하는 경우 하나만 배열에 저장해야 한다.

1번의 조건은 이중 반복문을 통해 쉽게 구현할 수 있었다.  
하지만 중복을 제거하는 방법에서 내가 직접 구현하는 것 보다 자료구조를 사용하는 것이 훨씬 효율적이라고 판단했다. 그래서 **`Set`** 자료구조를 사용했다.

`Set` 자료구조의 대표적인 구현 클래스로 `HashSet`가 존재한다.  
먼저 `HashSet`은 `Hash` 형태를 기반으로 하기 때문에 정렬에 대해 완전하지 못하다.  
즉, 저장된 데이터의 순서가 뒤죽박죽이라는 뜻이다!  
단, 중복된 데이터는 저장하지 않는 `Set` 자료구조의 특성을 가지고 있다.

### Set 자료구조 톺아보기

[\[Java\] 자바 HashSet 사용법 & 예제 총정리](https://coding-factory.tistory.com/554)

이러한 이유로 나는 이중 반복문으로 배열의 요소들을 각각 더한 후, 해당 값을 `HashSet`에 저장했고, 저장된 데이터를 `Iterator`로 변환하여 `Arrays.sort()` 메소드를 이용해 더한 값을 오름차순으로 정렬하는 아이디어로 코드를 구성했다.

## 코드

```java
package 심성헌.알고리즘_6주차;

import java.util.HashSet;
import java.util.Iterator;
import java.util.Set;

public class 두개뽑아서더하기_프로그래머스 {

    public static void main(String[] args) {
        int[] arr = { 5, 0, 2, 7 };
        int[] answer = solution(arr);

        for (int i = 0; i < answer.length; i++) {
            System.out.print(answer[i] + " ");
        }
    }

    public static int[] solution(int[] numbers) {
        Set<Integer> set = new HashSet<Integer>();
        for (int i = 0; i < numbers.length - 1; i++) {
            int x = numbers[i];
            for (int k = i + 1; k < numbers.length; k++) {
                int y = numbers[k];
                set.add(x + y);
            }
        }

        Iterator<Integer> iter = set.iterator();

        int[] answer = new int[set.size()];
        int cnt = 0;
        while (iter.hasNext()) {
            answer[cnt] = iter.next();
            cnt++;
        }

        for (int i = 0; i < answer.length - 1; i++) {
            int tmp = answer[i];
            for(int k = i + 1; k < answer.length; k++) {
                if(tmp > answer[k]) {
                    answer[i] = answer[k];
                    answer[k] = tmp;
                    tmp = answer[i];
                }
            }
        }

        return answer;
    }
}
```

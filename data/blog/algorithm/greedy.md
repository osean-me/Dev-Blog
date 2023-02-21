---
title: '탐욕 알고리즘'
date: '2020-10-18'
tags: ['백준 5585', '알고리즘', '코딩 테스트', '탐욕 알고리즘', 'Greedy']
draft: false
summary: '백준 5585 문제를 풀고 이를 풀기 위해 학습한 탐욕 알고리즘에 대해 이해한 내용을 정리 했습니다.'
---

# 그리디 알고리즘

그리디 알고리즘은 최적해를 구하는 데에 주로 사용하며, 동적 프로그래밍과 함께 사용하여 효율을 올린다고 알려져있다.  
또한, 그리디 알고리즘은 여러 경우 중 하나를 결정해야 할 때 그 순간의 최적의 선택을 컴퓨터에게 하도록 만드는 것을 의미하는데, 그리디 알고리즘이 최적해를 보장 해주지 않지만 결과까지 도달하는 시간이 굉장히 단축된다는 장점이 있다.

위의 트리 구조에서 가장 최적의 해는 7 → 100 → 107의 경로가 가장 최적의 해로 도달할 수 있는 경우이다.

![트리](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FRTwSL%2FbtqK8k7soUj%2FwwTHrxY6DodKefvOY8ia01%2Fimg.png)

하지만 그리디 알고리즘은 7 보다 큰 13을, 5 보다 큰 11을 선택한다. 즉, 현재 위치에서 가장 최선의 선택을 하기 때문에 최적해를 보장 해주지 않는 다는 뜻이다.

가장 간단한 예시는 거슬러 줘야 할 **최소한의 동전 개수**가 몇 개인지 파악할 때 그리디 알고리즘이 사용된다.

# 거스름 돈

해당 문제는 그리디 알고리즘을 잘 보여주는 문제 중 하나이다. 비교적 쉽게 풀 수 있다. 트리 구조에서 하나의 횡단에 500, 100, 50, 10, 5, 1 의 값을 가진 노드가 있다고 생각하고 최선의 선택을 통한 최적해(해당 문제에서는 최소한의 동전 개수)를 구하고자 한다면 아래의 그림처럼 될 것이다.

![설명](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fegcm12%2FbtqK84XAMbU%2FKjo2OkqqGutkqqnVjxuCn1%2Fimg.jpg)

위의 그림을 통해 대략적인 이해가 되었다. 그렇다면 이제 코드로 구현해 보자!

- **_그리디 알고리즘 구조대로 풀거라고는 상상도 못한 코드_**

```java
package 심성헌.알고리즘_3주차;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class 거스름돈_5585 {

  public static void main(String[] args) throws NumberFormatException, IOException {
    BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    int n = 1000 - Integer.parseInt(br.readLine());
    int[] coin = { 500, 100, 50, 10, 5, 1 };
    int count = 0;
    int result = 0;

    for (int i = 0; i < coin.length; i++) {
      if (n >= coin[i]) {
        count = n / coin[i];
        result += count;
        n = n - (coin[i] * count);
      }
    }

    System.out.println(result);
  }
}
```

1차원 배열을 이용해서 동전 종류를 저장했다. 즉, 각 횡단에 존재하는 노드의 종류라고 생각하면 좋을 듯 하다!  
이제 1차원 배열의 길이만큼 반복해서 거슬러 줘야 할 최소한의 동전 개수를 파악하면 된다. 나는 아래의 식을 이용해서 문제를 풀었다.

> 나머지 금액 = 현재 금액 - (현재 동전 종류 \* 동전 개수)

여기서 동전 개

수를 파악할 저장소만 선언해주면 쉽게 문제를 풀 수 있다!

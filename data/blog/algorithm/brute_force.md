---
title: '완전 탐색 알고리즘'
date: '2020-10-18'
tags: ['백준 7568', '알고리즘', '코딩 테스트', '완전 탐색 알고리즘', 'Brute Force']
draft: false
summary: '백준 7568 문제를 풀고 이를 풀기 위해 학습한 완전 탐색에 대해 이해한 내용을 정리 했습니다.'
---

# 완전 탐색

코딩 테스트를 볼 때 가장 기초적인 그리고 가장 출제 빈도가 높은 알고리즘이 그리디와 완전 탐색 알고리즘이라고 한다. 그래서 이번 주차 문제도 그리디, 완전 탐색 그리고 재귀 알고리즘에 대한 문제를 공지했다. 그렇다면 도대체 완전 탐색 알고리즘은 뭘까?

[참고 아티클](https://velog.io/@sungjun-jin/%EC%99%84%EC%A0%84%ED%83%90%EC%83%89)

~(위 블로그에서 아주 자세히 설명하고 있다.)~

위 글에서 완전 탐색 기법의 종류로

- **Brute Force : for문과 if문을 이용하여 처음부터 끝까지 탐색하는 방법**
- **비트 마스크 : 이진수 표현을 자료구조로 쓰는 기법 (AND, OR, XOR, SHIFT, NOT)**
- **재귀 함수**
- **순열 : 서로 다른 n개의 원소에서 r개의 중복을 허용하지 않고 순서대로 늘어 놓은 수**
- **BFS(너비우선탐색), DFS(깊이우선탐색)**

라고 한다. 이번에 출제된 **`덩치`** 문제는 Brute Force(브루트 포스) 알고리즘이라고 하는데, for문과 if문을 이용하여 처음부터 끝까지 탐색한다고 한다.  
브루트 포스 알고리즘 유형은 풀이가 쉬운 만큼 많은 문제를 가지고 있는데 대표적으로

- **비교해야 할 개수가 많으면 많아질 수록 런타임이 오래 걸린다.**
- **같은 이유로 메모리 용량을 많이 잡아 먹는다.**

해당 알고리즘을 사용했을 때, 1부터 1000억까지 모든 경우의 수를 비교한다고 한다면 경우의 수는

> **100,000,000,000 \* 100,000,000,000 = 생각하기도 싫음**

생각하기도 싫을 만큼의 경우의 수를 비교해야 한다. 그렇다면 위에서 말한 문제점이 발생할 것이다. 여기서 웃긴 점은 마치 버블 정렬과 조금 비슷한 느낌을 받았다. 버블 정렬도 자신의 위치에서 뒤에 존재하는 모든 수와 비교를 하는데 완전 탐색을 조금 더 발전한 것이 버블 정렬 아닐까 생각해본다! (아닐거다🤥)

# 덩치 문제

해당 문제는 경우의 수를 비교하는 것보다 입력 받은 값(String)을 int로 각각 변환 후 저장하는 것이 더 어려웠다.

1.  **StringTokenizer 클래스는 delim(구분자)을 기준으로 문자열을 잘라 Token으로 저장한다.**
2.  **이 Token은 한 번 호출하고 나면 삭제된다.**
3.  **마치 Set 자료 구조 같구나.**

[참고 아티클](https://steemit.com/kr-dev/@gyeryak/easyalgo-2-bruteforce)

나는 입력 받은 값을 int\[\]\[\] 2차원 배열로 저장하고 싶었고, 이를 구현하기 위해서 StringTokenizer 객체도 n개 만큼의 1차원 배열로 만들어야 했다.  
그 후에 StringTokenizer 각 인덱스마다 어떤 구분자로 Token을 해당 인덱스 안에 Token으로 저장할 것인지 생성자를 선언 해줘야 했다.

- **_입력 받은 문자열을 int\[\]\[\] 2차원 배열로 저장하는 구조_**

![설명1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F1zieI%2FbtqLbmKrvqQ%2F3tlCZSDKLpvqwrtxqQPW2k%2Fimg.jpg)

그림을 어떻게 그려야 할지 몰라서 손코딩처럼 적어 보았다!  
StringTokenizer 클래스에 대한 자세한 그림은 아래 링크에서 확인할 수 있다.

[참고 아티클](https://codingdog.tistory.com/entry/java-stringtokenizer-%EA%B5%AC%EB%B6%84%EC%9E%90%EB%A5%BC-%EA%B8%B0%EC%A4%80%EC%9C%BC%EB%A1%9C-%EB%AC%B8%EC%9E%90%EC%97%B4%EC%9D%84-%EC%9E%90%EB%A5%B8%EB%8B%A4)

이제 코드를 확인해 볼 차례다. 입력 받은 값을 int 자료형으로 바꿨다면, 이제 모든 데이터를 비교해야 한다.  
즉, 총 5명의 사람이 있다고 가정 했을 때, 자신을 제외한 모든 사람들과 비교해야 한다. 이 때의 경우의 수는

> **(5-1) \* 5 = 20  
> 경우의 수 = 20개**

그림을 너무 못 그렸지만 자신을 제외한 사람들을 비교했을 때 그려지는 모양은 위와 같다. 이 것을 코드로 확인 해보자!

![설명2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbbBSQF%2FbtqK83RSJBM%2FqJJkU9LiVOMZHkAA6rgmb0%2Fimg.jpg)

- **_위풍당당 완전 탐색 코드_**

```java
package 심성헌.알고리즘_3주차;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class 덩치_7568 {
  public static void main(String[] args) throws NumberFormatException, IOException {
     BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
     StringBuffer sb = new StringBuffer();
     int n = Integer.parseInt(br.readLine());
     int[][] people = new int[n][2];
     StringTokenizer[] st = new StringTokenizer[n];

     for (int i = 0; i < n; i++) {
      int m = 0; st[i] = new StringTokenizer(br.readLine(), " ");

      while (st[i].hasMoreTokens()) {
        people[i][m] = Integer.parseInt(st[i].nextToken());
        m++;
      }
    }

    for (int i = 0; i < people.length; i++) {
      int rank = 1;

      for (int k = 0; k < people.length; k++) {
        boolean check = (people[i][0] < people[k][0]) && (people[i][1] < people[k][1]) && (i != k);

        if (check) {
          rank++;
          }
        }

      sb.append(rank + " ");
    }

    System.out.println(sb.toString());
  }
}
```

위의 코드를 확인했을 때, 다중 for문을 사용한 것을 확인할 수 있다.

자신을 뜻하는 `i`, `i`와 비교할 다른 사람을 뜻하는 `k`.  
즉, `i`를 기준으로 `k` 를 비교하면 된다. 이 때, `i` 보다 몸무게, 키가 크다면 `count` 값이 + 1 증가한다.

이제 `i` 에 할당된 count를 StringBuffer 객체에 저장하여 이를 출력하면 문제 해결 완료!

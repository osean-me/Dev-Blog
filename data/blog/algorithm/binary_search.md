---
title: '순차 탐색 및 이진 탐색'
date: '2020-11-07'
tags: ['알고리즘', '코딩 테스트', '순차 탐색', '이진 탐색']
draft: false
summary: '순차 탐색과 이진 탐색에 대해 이해한 내용을 정리 했습니다.'
---

# 순차 탐색 / 이진 탐색

## 순차 탐색

- 배열 안에 있는 특정 데이터를 찾기 위해 앞으로 데이터를 하나씩 차례대로 확인하는 방법이다.
- 정렬되지 않은 배열에서 특정 데이터를 찾고자 할 때 사용한다.
- 데이터가 아무리 많아도 시간이 충분하다면 항상 원하는 데이터를 찾을 수 있다는 장점이 있다.
- 제일 처음 인덱스부터 순차적으로 탐색하기 때문에 시간 복잡도는 `**O(N)**` 이다. (N = 배열의 길이)
- ~(뭔가 버블 정렬과 비슷한 알고리즘 같다.)~

## 이진 탐색

- 배열이 정렬되어 있다는 전제 하에 사용할 수 있는 알고리즘이다.
- 시작점과 끝점을 이용해 중간점을 찾고, 찾고자 하는 데이터와 중간점의 데이터가 맞을 때 까지 반복한다.
- 즉, 재귀 함수 또는 반복문을 통해 구현할 수 있다.
- 탐색 범위를 절반씩 줄여 나가기 때문에 빠른 시간 내에 탐색을 마무리 할 수 있다.
- 단, 배열 내 중복된 데이터가 있을 경우 최선의 알고리즘은 아니다.

![풀이](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Flh7dV%2FbtqMOxQ2fwJ%2F7B7U3Lsbn3sin0ASKhCRLk%2Fimg.jpg)

## Lower / Upper Bound

- 이진 탐색의 경우 중복 데이터가 존재하지 않을 경우 최선이지만, 중복 데이터가 존재할 경우 최선이 될 수 없다. 이를 보완하기 위해 나온 알고리즘이 Lower / Upper Bound 알고리즘이다.

### Lower Bound

- 찾고자 하는 데이터와 같거나 가장 가까운 크기의 데이터의 인덱스를 반환한다.
- 즉, Lower Bound를 통해 반환된 인덱스 번호 이후에 데이터는 몇 개인지는 모르지만 중복된 데이터가 존재한다는 의미이다. (이를 보완하기 위해서 Upper Bound 알고리즘을 활용한다.)
- 그러한 이유로 반환된 인덱스 번호 이전의 인덱스에는 절대 해당 데이터는 배열 내에서 존재하지 않는다.
- 만약 찾고자 하는 데이터가 배열 내에 존재하지 않을 경우 찾고자 하는 데이터와 가장 근접한 데이터의 인덱스 번호를 반환한다.

### Upper Bound

- 찾고자 하는 데이터가 배열 내에서 몇 개가 존재하는지 모르는 Lower Bound 알고리즘을 보완하기 위해서 만들어진 알고리즘이다.
- Upper Bound 알고리즘은 찾고자 데이터의 가장 가까운 초과된 데이터가 존재하는 인덱스 번호를 반환한다.

### 참고 블로그

[참고 아티클](https://jackpot53.tistory.com/33)

# 이진 탐색을 응용해서 문제 해결하기

## 부품 찾기 - 이코테

```java
package 심성헌.알고리즘_5주차.이코테;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class 부품찾기_이코테 {

    static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    static int n, m;
    static int[] arr1, arr2;

    public static void main(String[] args) throws Exception {
        // N = 물건의 개수 / M = 내가 찾는 물건의 개수
        // N 입력
        n = Integer.parseInt(br.readLine());
        arr1 = new int[n];
        StringTokenizer st = new StringTokenizer(br.readLine());

        int idx = 0;
        while (st.hasMoreTokens()) {
            arr1[idx] = Integer.parseInt(st.nextToken());
            idx++;
        }

        // M 입력
        m = Integer.parseInt(br.readLine());
        arr2 = new int[m];
        st = new StringTokenizer(br.readLine());
        idx = 0;
        while (st.hasMoreTokens()) {
            arr2[idx] = Integer.parseInt(st.nextToken());
            System.out.print(find(arr1, 0, n - 1, arr2[idx]) + " ");
            idx++;
        }
    }

    public static String find(int[] arr, int start, int end, int target) {
        if (start > end) return "No";
        int mid = (start + end) / 2;
        if (arr[mid] == target) return "Yes";
        else if (arr[mid] > target) return find(arr, start, mid - 1, target);
        else return find(arr, mid + 1, end, target);
    }
}
```

이번 문제는 비교적 쉽게(?) 풀 수 있었던 것 같다.  
다만 주어진 테스트 케이스에서 마지막 출력값이 `Yes` 로 출력되어야 하는데 자꾸 `No` 로 출력되어서 왜 이런지 이유를 찾는데 시간이 꽤나 소요됐다.

`find` 메소드는 String 자료형을 반환하는데, `else if` 구문에서 재귀로 `find` 메소드를 호출하게 해놓고서는 이걸 `return` 해주지 않는 정말 사소한 문제때문에 시간이 오래 소요됐다. 맞다. 나 바보다.

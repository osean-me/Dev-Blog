---
title: '숫자 카드 2'
date: '2020-11-07'
tags: ['백준 10816', '알고리즘', '코딩 테스트', '이진 탐색']
draft: false
summary: '백준 10816 문제를 풀고 이진 탐색에 대해 이해한 내용을 정리 했습니다.'
---

## 숫자 카드 2 - 백준 10816

- 나를 미치게 만든 문제
- 수많은 런타임 에러, 시간 초과, 메모리 초과를 겪으면서 미쳐버리는 줄 알았다.

### 이진 탐색으로 풀어보기

```java
package 심성헌.알고리즘_5주차.백준;

import java.io.*;
import java.util.*;

public class 숫자카드2_10816_binary {

    static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    static BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
    static StringBuffer sb = new StringBuffer();
    static int n, m, count;
    static int[] arr1, arr2;
    static boolean[] check;

    public static void main(String[] args) throws Exception {
        n = Integer.parseInt(br.readLine());
        arr1 = new int[n];
        check = new boolean[n];

        StringTokenizer st = new StringTokenizer(br.readLine());
        int cnt = 0;
        while (st.hasMoreTokens()) {
            arr1[cnt] = Integer.parseInt(st.nextToken());
            cnt++;
        }

        Arrays.sort(arr1);

        m = Integer.parseInt(br.readLine());
        arr2 = new int[m];

        st = new StringTokenizer(br.readLine());
        cnt = 0;
        while (st.hasMoreTokens()) {
            arr2[cnt] = Integer.parseInt(st.nextToken());
            count = 0;
            card(arr1, 0, n - 1, arr2[cnt]);
            sb.append(count + " ");
            cnt++;
        }
        bw.write(sb.toString());
        bw.flush();
        bw.close();
    }

    public static void card(int[] arr, int start, int end, int target) {
        if (start > end || start < 0 || end >= n)
            return;
        int mid = (start + end) / 2;
        int mid1 = mid + 1 >= n ? n - 1 : mid + 1;
        int mid2 = mid - 1 < 0 ? 0 : mid - 1;
        if (arr[mid] == target) {
            if (!check[mid]) {
                count++;
                check[mid] = true;
                if (arr[mid2] != target || arr[mid1] == target) {
                    card(arr, mid1, end, target);
                } else if (arr[mid2] == target || arr[mid1] != target) {
                    card(arr, start, mid2, target);
                }
            }
        } else if (arr[mid] > target) {
            card(arr, start, mid2, target);
        } else {
            card(arr, mid1, end, target);
        }
    }
}
```

위의 부품 찾기 문제를 해결했기 때문에 정답률이 30% 대이긴 하지만 이번 문제는 어느정도 쉽게 해결할 수 있을 것 같았다.  
그래서 이진 탐색을 이용해 문제를 풀었고, 내가 구현하고자 했던 로직은 다음과 같다.

- `arr1` 배열을 이진 탐색으로 `target`과 같은 데이터를 찾았을 경우
  1.  boolean 배열인 `check` 의 해당 인덱스 값을 `true` 로 변경해 재방문시 일어날 수 있는 중복 카운트의 위험성을 제거한다.
  2.  `cnt++` 를 실행한다.
  3.  해당 인덱스의 전, 후 인덱스 데이터와 `target` 이 같을 경우
      - 전의 인덱스와 같을 경우 → `end = mid - 1`
      - 후의 인덱스와 같을 경우 → `start = mid + 1`
- `target` 이 `arr1[mid]`보다 작을 경우
  1.  `end = mid - 1`
- `target`이 `arr1[mid]`보다 클 경우
  1.  `start = mid + 1`

이러한 로직으로 구현했고, 주어진 테스트 케이스로는 이상없이 출력되었다.  
하지만 백준에서 채점했을 때 **시간 초과** 혹은 **메모리 초과**가 발생했다. 당시에는 발생하는 이유를 알 수 없었고, 그냥 내 코드가 잘못되었다고만 생각했다.

그래서 알아본 결과, **Lower / Upper Bound 알고리즘**을 응용하면 문제를 해결할 수 있을거란 답변을 보고 이를 적용해봤다.

### Lower / Upper Bound를 이용해 풀어보기

```java
import java.io.*;
import java.util.*;

public class Main {

    static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    static BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
    static StringBuffer sb = new StringBuffer();
    static int n, m;
    static int[] arr1 = new int[20000001];

    public static void main(String[] args) throws Exception {
        n = Integer.parseInt(br.readLine());

        StringTokenizer st = new StringTokenizer(br.readLine());
        int cnt = 0;
        while (st.hasMoreTokens()) {
            arr1[cnt] = Integer.parseInt(st.nextToken());
            cnt++;
        }

        Arrays.sort(arr1);

        m = Integer.parseInt(br.readLine());

        st = new StringTokenizer(br.readLine());
        cnt = 0;
        while (st.hasMoreTokens()) {
            int result = check(Integer.parseInt(st.nextToken()));
            sb.append(result + " ");
        }
        bw.write(sb.toString());
        bw.flush();
        bw.close();
    }

    public static int check(int target) {
        int lower = lowerbound(arr1, target);
        int upper = upperbound(arr1, target, lower);

        return upper - lower;
    }

    public static int lowerbound(int[] arr, int target) {
        int lower = 0;
        int upper = arr1.length;
        while (lower < upper) {
            int mid = lower + (upper - lower) / 2;
            if (target <= arr1[mid]) {
                upper = mid;
            } else {
                lower = mid + 1;
            }
        }
        return lower;
    }

    public static int upperbound(int[] arr, int target, int lowerIdx) {
        int lower = lowerIdx;
        int upper = arr1.length;
        while (lower < upper) {
            int mid = lower + (upper - lower) / 2;
            if (target >= arr[mid]) {
                lower = mid + 1;
            } else {
                upper = mid;
            }
        }
        return lower;
    }
}
```

_~(어떻게 로직이 진행되는지 확인해보려고 System.out.println() 이 많다..^^; 디버킹도 할 줄 알아야 하는데..)~_

처음에는 `check` 라는 메소드없이 `main` 메소드에서 `arr2` 배열을 입력받으면서 `lower`, `upper` 함수를 실행하고 반환되는 int 값을 계산한 것을 출력했다. 근데 런타임 에러가 발생했다. 이 때도 도대체 왜 런타임 에러가 나는지 알 수 없었다.

그래서 다른 사람들의 코드를 조금(?) 참고해서 알아낸 결과, 내 코드에서 출력을 위한 `lower`, `upper` 메소드를 호출하는 과정에서 런타임 에러가 발생하는 것 같았다.

이를 해결하기 위해 `check` 메소드를 만들었고, 혹시나 하는 마음에 `upper` 메소드에는 `lower` 메소드에서 반환한 값을 넣어줘 최대한 적은 횟수로 탐색할 수 있도록 구현해보았다.

```java
import java.io.*;
import java.util.*;

public class Main {

    static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    static BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
    static StringBuffer sb = new StringBuffer();
    static int n, m;
    static int[] arr1 = new int[20000001];

    public static void main(String[] args) throws Exception {
        n = Integer.parseInt(br.readLine());

        StringTokenizer st = new StringTokenizer(br.readLine());
        int cnt = 0;
        while (st.hasMoreTokens()) {
            arr1[cnt] = Integer.parseInt(st.nextToken());
            cnt++;
        }

        Arrays.sort(arr1);

        m = Integer.parseInt(br.readLine());

        st = new StringTokenizer(br.readLine());
        cnt = 0;
        while (st.hasMoreTokens()) {
            int result = check(Integer.parseInt(st.nextToken()));
            sb.append(result + " ");
        }
        bw.write(sb.toString());
        bw.flush();
    }

    public static int check(int target) {
        int lower = lowerbound(arr1, target);
        int upper = upperbound(arr1, target, lower);

        return upper - lower;
    }

    public static int lowerbound(int[] arr, int target) {
        int lower = 0;
        int upper = arr1.length;
        while (lower < upper) {
            int mid = (upper + lower) / 2;
            if (target <= arr1[mid]) {
                upper = mid;
            } else {
                lower = mid + 1;
            }
        }
        return lower;
    }

    public static int upperbound(int[] arr, int target, int lowerIdx) {
        int lower = lowerIdx;
        int upper = arr1.length;
        while (lower < upper) {
            int mid = (upper + lower) / 2;
            if (target >= arr[mid]) {
                lower = mid + 1;
            } else {
                upper = mid;
            }
        }
        return lower;
    }
}
```

이렇게 `check` 메소드를 만드니까 24% 정도 들어갔을 때 틀렸습니다를 받을 수 있었다.  
그렇다면 애초에 내가 짠 코드는 틀린 코드라는 말이다.🤗  
그래서 어디가 틀렸을까 확인하는 중에 도무지 알 수 없어서 다른 사람들의 코드를 조금 참고 했는데, lower / upper 메소드가 거의 이진 탐색과 비슷한 코드를 발견했다.

[참고 아티클](https://herong.tistory.com/entry/BOJ-10816-%EC%88%AB%EC%9E%90-%EC%B9%B4%EB%93%9C-2-Java)

이 분의 경우

1.  `mid`값을 비트 연산(>>)을 통해 추출한다.
2.  `target`값이 `arr[mid]`와 같을 때는 `lower`값을 `mid + 1`로 설정한다.
3.  `target`값이 `arr[mid]`보다 작을 경우 `upper`값을 `mid`로 설정한다.
4.  `target`값이 `arr[mid]`보다 클 경우 `lower`값을 `mid + 1`로 설정한다.

나와 굉장히 비슷하다. 다만 반환하는 값이 다른데, 위의 링크에서는 `lower` 메소드의 경우 +1 연산을 추가적으로 진행하며 `check` 메소드에서도 +1 연산을 진행한다.  
나는 아직 의도를 파악하지 못했기 때문에 원래 구현 했던 방식대로 +1 연산을 제외하고 했더니 더 빠른 결과를 보여줬다.(웃기는 짬뽕)

![결과](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FGwzG2%2FbtqMPhtCShi%2FIkOZLHATIecz2U5BIkXK3k%2Fimg.png)

아마도 1번의 경우 때문에 +1 연산을 추가적으로 해준 것 같다. 어쨌든 위 블로그의 코드는 이진 탐색의 응용이라고 볼 수 있다.

다만 나는 재귀 함수를 통해 구현 했고, 블로거 분께서는 while문으로 구현 했다의 차이인 것 같은데, 재귀로 인한 stackoverflow가 발생해서 이전의 시간 초과, 메모리 초과, 런타임 에러가 발생한 것이 아닐까 싶다.

## 최종 코드

```java
import java.io.*;
import java.util.*;

public class Main {

    static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    static BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
    static StringBuffer sb = new StringBuffer();
    static int n, m;
    static int[] arr1;

    public static void main(String[] args) throws IOException {
        n = Integer.parseInt(br.readLine());

        StringTokenizer st = new StringTokenizer(br.readLine());
        int cnt = 0;
        arr1 = new int[n];
        while (st.hasMoreTokens()) {
            arr1[cnt] = Integer.parseInt(st.nextToken());
            cnt++;
        }

        Arrays.sort(arr1);

        m = Integer.parseInt(br.readLine());

        st = new StringTokenizer(br.readLine());
        cnt = 0;
        while (st.hasMoreTokens()) {
            int target = Integer.parseInt(st.nextToken());
            int result = check(target);
            sb.append(result + " ");
        }
        bw.write(sb.toString());
        bw.flush();
        bw.close();
    }

    public static int check(int target) {
        int lower = lowerbound(target);
        int upper = upperbound(target);

        return upper - lower;
    }

    public static int lowerbound(int target) {
        int lower = 0;
        int upper = n;
        while (lower < upper) {
            int mid = (upper + lower) / 2;
            if (target == arr1[mid]) {
                upper = mid;
            } else if (arr1[mid] > target) {
                upper = mid;
            } else {
                lower = mid + 1;
            }
        }
        return lower;
    }

    public static int upperbound(int target) {
        int lower = 0;
        int upper = n;
        while (lower < upper) {
            int mid = (upper + lower) / 2;
            if (target == arr1[mid]) {
                lower = mid + 1;
            } else if (target < arr1[mid]) {
                upper = mid;
            } else {
                lower = mid + 1;
            }
        }
        return upper;
    }
}
```

즉, 지금까지 발생한 문제를 정리해보자면

1.  **시간 초과**
    - 말 그대로 시간 초과를 발생한 것 같다. 처음 제출한 코드는 재귀 함수로 구현 했는데, 이 때 `target`과 `arr[mid]`값이 같을 경우 `arr[mid]`의 앞, 뒤 인덱스도 다시 탐색하도록 코드를 구현했다.
    - 이러한 이유로 불필요한 재귀 함수가 호출 되면서 정해진 시간보다 초과해서 이런 문제가 발생한 것 같다.
2.  **메모리 초과**
    - 위의 문제와 비슷한 문제인 것 같다.
    - 이 경우도 재귀 함수로 구현 했는데 `arr[mid]`의 앞, 뒤 인덱스를 다시 탐색할 때 새로운 조건을 추가하게 되면서 메모리가 더 많이 소요되는 문제를 발생했던 것 같다.
3.  **런타임 에러**
    - 아직도 정확한 이유를 모르겠다.
    - 예상으로는 인덱스 배열의 크기를 넘거나 적은 값을 탐색 했기 때문에 발생한 것 같다.
4.  **틀렸습니다.**
    - `lower` / `upper` 메소드에서 `target`과 `arr[mid]`를 비교하는 조건문을 `≤` 혹은 `else` 를 이용해 두 가지로만 구현 했는데 이 경우에 정확한 방향을 잡지 못해 옳지 못한 답을 중간(24%)에 출력한 것 같다.

너무 답답했지만 그래도 이해하고 나니 즐거웠다. 또 보자! (**계수 정렬**을 통해 한번 풀어 봐야겠다.)

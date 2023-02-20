---
title: '최소 / 최댓값 구하기'
date: '2020-10-16'
tags: ['백준 10818', '알고리즘', '코딩 테스트', '최소값', '최댓값']
draft: false
summary: '백준 10818 문제를 풀고 이해한 내용을 정리 했습니다.'
---

# 최소, 최댓값 구하기

면접 준비를 하면서 선택 정렬과 버블 정렬이 문제로 나온다고 들어서 선택 정렬에 대해서 먼저 공부를 했다. 예전에 풀었던 문제지만 다시 들여다보지 않아서 다 까먹었다. 어쨌든, 나는 이번 문제를 선택 정렬 알고리즘을 활용해서 문제를 풀었다. 근데 결과는 런타임 에러였다.

- 런타임 에러 코드

```java
import java.io.*;
public class Main {
  public static void main(String[] args) throws Exception {
    BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

    int n = Integer.parseInt(br.readLine());
    int[] list = new int[n];

    for (int i = 0; i < n; i++) {
      list[i] = Integer.parseInt(br.readLine());
    }

    int min = list[0];
    int max = list[0];

    for (int i = 0; i < n; i++) {
      for (int k = i + 1; k < n; k++) {
        boolean minCheck = min > list[k];
        boolean maxCheck = max < list[k];

        if (minCheck) min = list[k];
        if (maxCheck) max = list[k];
      }
    }

    System.out.println(min + " " + max);
  }
}
```

분명 IDE에서는 주어진 예제의 입력 데이터를 넣었을 때 동일한 결과가 출력되는데, 왜 제출하면 에러가 발생해서 채점조차 안되는지 알 수가 없었다.  
똑같은 코드를 형식만 바꿔서 10번 정도 다시 넣어 봐도 결과는 똑같았다.

## BufferReader

이번 코드는 Scanner클래스를 사용하지 않고 BufferReader클래스를 사용했다.  
곰곰이 생각했을 때, 내가 유추할 수 있는 런타임 에러의 이유는 바로 이거다.

**입력 예제를 붙여 넣었을 때 에러가 발생하는 이유는 채점 시, 삽입하는 데이터가 내 코드에 맞지 않기 때문이다!**

코드에서 BufferReader의 메소드 중 readLine() 메소드를 사용하려 입력 예제를 받았는데, 해당 메소드는 한 줄로 입력한 데이터를 문자열로 받고, 이를 Integer로 변환하는 과정에서 에러가 발생하는 것 같다. 즉, 띄어쓰기만큼 문자열을 잘라내고, 이를 기본 자료형으로 바꿔야 했다.

Scanner를 사용했더라면, next() 메소드를 사용해 띄어쓰기를 기준으로 문자열을 나누면 됐는데, BufferReader는 그게 안되는 것 같았다.  
(아마 더 현명한 방법이 있을지도..)  
하지만 나는 Scanner를 사용하고 싶지 않았는데, 무엇보다 BufferReader를 사용했을 때 로직을 해결하는 런타임 시간이 굉장히 줄었기 때문이다.(다른 사람들 코드를 보니 그러했다.)  
그러한 이유로 BufferReader를 잘 활용할 수 있는 방법이 뭐가 있을까 검색했다.

## StringTokenizer

구글 검색이나 맞은 사람들 코드를 확인해보니 StringTokenizer라는 클래스를 사용했다. 이 클래스는 문자열을 특정 구분자를 지정하고, 이를 기준으로 문자열을 잘라내고, 생성자를 선언한 변수에 저장한다. 해당 클래스를 사용하면

1.  **_최소, 최댓값을 검사할 정수를 입력 받는다._**
2.  **_입력 받은 정수를 일일이 검사한다._**

이 과정이 줄어들어 입력 받는 동시에 최소, 최댓값 검사가 가능하다.  
또한, 해당 클래스는 Set 자료구조를 응용해서 작동하는 것 같았다.  
왜냐하면 처음 DAO, DTO에 대한 개념을 배울 때 ResultSet이라는 클래스를 이용해 데이터베이스에서 데이터를 받아오고, next() 메소드를 이용해 받아 올 Rownum 개수만큼 while문을 돌리는 것이 떠올랐기 때문이다.

![풀이](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FntkHQ%2FbtqK7zpx7qo%2Fwsxw4tvUkLmatT3goF6KJk%2Fimg.jpg)

- BufferReader + StringTokenizer를 사용한 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main { public static void main(String[] args) throws Exception {
  BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
  Integer.parseInt(br.readLine());
  // 배열의 길이
  StringTokenizer st = new StringTokenizer(br.readLine(), " ");
  int min = 1000001; int max = -1000001;
  while (st.hasMoreTokens()) {
    int value = Integer.parseInt(st.nextToken());
    if (max < value) max = value;
    if (min > value) min = value;
  }
    System.out.println(min + " " + max);
  }
}
```

위의 코드를 보면 알 수 있듯, 입력 받은 문자열을 반복문 안에서 int로 재정의했다. 또한, max와 min변수를 만들어, 논리값이 true인 경우에 각 변수의 데이터가 현재의 value값이 되는 구조로 만들었다.  
(사실 StringTokenizer를 사용하려고 하다 보니 다른 사람이 짠 코드를 볼 수 밖에 없었다..쓰렉성헌..🗑)

## 마무리

아주 예전에 이 문제를 푼 적이 있는데, 그 때는 알고리즘이 뭔지 문제는 어떻게 풀어야 할지도 몰라서 이해하지도 못한 남의 코드 갖다 붙여 넣어서 맞은 기록이 있다. 이번에는 그러고 싶지 않았지만 의도치 않게(?) 이런 상황이 된 점 조금 속상하다. 그래도 그 때와는 달리 코드 자체가 이해가 되고, 따로 검색하지 않아도 내가 모르는 클래스를 왜 이렇게 사용했는지 읽히고 있다.  
어쨌든, 예전에 제출했던 코드와 지금 제출한 코드와 비교를 하면

- 과거
  1.  ListArray 클래스 안에 최소,최댓값을 구해 주는 메소드가 있고, 해당 메소드를 사용.
  2.  Scanner 클래스를 사용함.
  3.  그래서 런타임이 오래 걸림.
- 현재
  - List의 하위 클래스는 사용하지 않음.
  - Scanner대신 BufferReader + StringTokenizer 사용.
  - 런타임이 확실히 줄어듬.

이런 결과를 확인할 수 있다.  
최대한 선택 정렬 알고리즘을 활용해서 풀었다면 좋았을 것 같은데, 망할 Scanner때문에 다른 사람 코드는 조금 훔쳐봤다. 앞으로는 그러지 말아야지..

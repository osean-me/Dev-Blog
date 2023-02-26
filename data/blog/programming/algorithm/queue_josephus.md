---
title: 'Queue 및 요세푸스 순열'
date: '2020-11-25'
tags: ['백준 1158', '알고리즘', '코딩 테스트', '요세푸스 순열', 'Queue']
draft: false
summary: 'Queue 와 요세푸스 순열에 대해 이해한 내용을 정리 했습니다.'
---

# Queue

![Queue](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbsHcnM%2FbtqN8eKJQ2N%2Flzd6YtOk0K9iCFIcGAXCyK%2Fimg.png)

Queue는 자료구조 중 하나로, **FIFO(First In First Out, 선입선출) 구조**를 가지는 자료구조이다.  
먼저 들어온 데이터가 먼저 나가는 구조를 뜻하며, 데이터가 입력된 시간 순서대로 처리해야 할 필요가 있는 상황에 사용한다. 대표적인 예로 **너비 우선 탐색(BFS)**에서 주로 사용한다.

# 요세푸스 순열

[1158번: 요세푸스 문제](https://www.acmicpc.net/problem/1158)

SSAFY를 준비하며 오픈카톡방에서 이런 저런 정보를 공유하던 중 CT를 대비하기 위해 **요세푸스 순열**을 공부하면 좋을 것 같다는 이야기를 들어 접하게 되었다.

해당 문제는 N명의 인원수(배열의 길이)와 순서대로 M 번째에 제거할 정수(출력되는 정수)가 주어진다.

나는 해당 문제를 **Queue** 자료 구조를 이용해서 해결했다.  
문제는 생각보다 간단히 해결했고, 정확히 이해하기 위해서 그림과 테이블을 만들어 하나씩 생각해 보았다.

![설명](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbMauCf%2FbtqOdaNTwYN%2FA9KdKtGEBVUzBYtkcP1ebK%2Fimg.jpg)

## 코드로 톺아보기

많은 사람들이 ArrayList 클래스를 이용해 더 간단하고 빠르게 문제를 해결할 수 있도록 했는데, 나는 아직 자료구조에 대한 이해가 많이 떨어져서 Queue를 이용해 해결하고자 했다.

위의 그림과 같이 0 번째 인덱스를 제외하고, `cnt` 변수를 활용해 `cnt` 변수의 값을 증가 시켜 해당 변수값이 M 번째라면 `queue` 변수의 가장 앞에 있는 데이터를 `poll()`하고, 다시 해당 데이터를 `offer()`하는 형식으로 구현하였다.

```java
package 심성헌.알고리즘_5주차.백준;

import java.io.*;
import java.util.*;

public class 요세푸스문제_1158 {

    static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    static BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
    static StringBuffer sb = new StringBuffer();
    static Queue<Integer> queue = new LinkedList<Integer>();
    static int n, m;

    public static void main(String[] args) throws Exception {
        StringTokenizer st = new StringTokenizer(br.readLine());
        n = Integer.parseInt(st.nextToken());
        m = Integer.parseInt(st.nextToken());
        int arr[] = new int[n + 1];

        for (int i = 1; i < arr.length; i++) {
            arr[i] = i;
            queue.offer(arr[i]);
        }
        String result = turn(arr, m);
        bw.write(result.toString());
        bw.flush();
        bw.close();
    }

    public static String turn(int[] arr, int turn) {
        int cnt = 1;
        sb.append("<");
        while (!queue.isEmpty()) {
            if (cnt == turn) {
                cnt = 1;
                int save = queue.poll();
                sb.append(save);
                if (!queue.isEmpty()) {
                    sb.append(", ");
                }
            } else {
                queue.offer(queue.poll());
                cnt++;
            }
        }
        sb.append(">");
        return sb.toString();
    }
}
```

# 주저리

많은 알고리즘을 접해본 것은 아니지만 지금까지 풀어 본 문제 중에서 내 수준과 딱 맞는 문제라고 생각한다.  
아직 여러모로 부족하기 때문에 내가 직접 짠 코드가 아닌 다른 사람의 코드를 참고하며 문제를 해결 했는데, 이번 문제는 오롯이 나 혼자 해결해서 조금 뿌듯하다.

하지만 아직 겪고 깨야 할 벽들이 많이 있으니 더 탄탄히 공부해서 실력을 쌓아 올려야겠다!

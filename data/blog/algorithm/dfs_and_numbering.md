---
title: 'DFS, 단지 번호 붙이기'
date: '2020-11-02'
tags: ['백준 2667', '알고리즘', '코딩 테스트', 'DFS', '너비 우선 탐색']
draft: false
summary: '백준 2667 문제를 풀고 이해한 내용을 정리 했습니다.'
---

# 그래프, DFS, BFS 복습하기

[백준 2667](https://www.notion.so/4-1-Graph-DFS-BFS-3b07d7be3bb74815860f7888261fe893)

# 얼음과 미로 그 사이 어딘가

처음 문제를 접했을 때, `인접 행렬`로 만들어진 테이블을 보고

> **"BFS로 풀어야 하나?"**

라고 생각했다. 그래서 구조는 기억이 나지 않지만 BFS(너비 우선 탐색)이 재귀나 Stack 자료 구조를 사용하지 않고, Queue 자료 구조를 이용하는 것을 바탕으로 공부 했던 내용을 톺아보며 코드를 작성했다.

## 도전! BFS!

```java
package 심성헌.알고리즘_5주차;

import java.io.*;
import java.util.*;

public class 단지번호붙이기_BFS {

    static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    static int n;
    static int length = 0;
    static int cnt = 0;
    static int[][] arr;

    public static void main(String[] args) throws Exception {
        n = Integer.parseInt(br.readLine());
        arr = new int[n][n];
        StringTokenizer[] st = new StringTokenizer[n];
        for (int i = 0; i < n; i++) {
            st[i] = new StringTokenizer(br.readLine());
            char[] tmp = st[i].nextToken().toCharArray();
            for (int k = 0; k < tmp.length; k++) {
                arr[i][k] = tmp[k] - '0';
            }
        }
        int[] xy = { 0, 0 };
        bfs(xy);
        for (int i = 0; i < arr.length; i++) {
            System.out.print("arr[" + i + "] = ");
            for (int k = 0; k < arr[i].length; k++) {
                System.out.print(arr[i][k]);
            }
            System.out.println();
        }
    }

    public static void bfs(int[] xy) {

        Queue<int[]> queue = new LinkedList<int[]>();
        queue.offer(xy);

        while (!queue.isEmpty()) {
            xy = queue.poll();
            int[][] nxy = { { -1, 1, 0, 0 }, { 0, 0, -1, 1 } };

            for (int i = 0; i < 4; i++) {
                nxy[0][i] = xy[0] + nxy[0][i];
                nxy[1][i] = xy[1] + nxy[1][i];
                int[] tmp = { nxy[0][i], nxy[1][i] };

                if (nxy[0][i] < 0 || nxy[1][i] < 0 || nxy[0][i] >= n || nxy[1][i] >= n)
                    continue;

                System.out.println("arr[" + nxy[0][i] + "][" + nxy[1][i] + "] = " + arr[nxy[0][i]][nxy[1][i]]);

                if (arr[nxy[0][i]][nxy[1][i]] == 1) {
                    arr[nxy[0][i]][nxy[1][i]] = 5;
                    queue.offer(tmp);
                    cnt++;
                }
            }
        }
    }
}
```

결과를 말하자면 실패다. 먼저 결과 출력을 확인 해보자.

![결과 콘솔](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbQNkAJ%2FbtqMqCyjgOq%2FKcZz6YliujeydEEqjBRn5K%2Fimg.png)

실패한 이유를 다음과 같이 파악할 수 있다.

1.  **Queue에 쌓인 노드의 좌표 중 현재 좌표를 기준으로 기준의 상하좌우 노드를 탐색하고, 탐색한 좌표의 노드가 조건에 부합하지 않는다면 Queue에 쌓이지 않는다.**
2.  **즉, Queue의 마지막 좌표의 상하좌우를 탐색 했을 때 조건에 부합하지 않는다면 Queue에는 아무 것도 쌓이지 않기 때문에 반복문은 종료된다.**

이러한 이유로 위의 콘솔에 출력된 인접 행렬 테이블을 확인해보면 첫 번째 단지만 탐색하고 나머지는 탐색하지 못한 것을 확인할 수 있다.  
첫 번째 단지의 마지막 노드 위치를 기준(`arr[2][2], x = 2, y = 2`)으로 상하좌우를 탐색 했을 때 조건에 부합하는 노드가 없으므로 Queue에 쌓이지 않고 반복문이 끝나면서 해당 프로그램을 종료된다.

그렇다면 해당 문제는 너비를 탐색하는 것이 아니라 깊이를 우선적으로 탐색해야 원하는 결과를 출력할 수 있다.

즉, 모든 노드를 탐색하지만 조건에 부합하는 노드를 발견하면 해당 노드에 인접한 노드를 먼저 탐색해야 문제를 해결할 수 있다.  
그리고 탐색을 진행하면서 처음 조건에 부합한 좌표의 논리가 `true` 인 경우에 해당 노드에 연결된 간선의 개수를 List 자료형에 담는다.

그러면 문제 해결!

이제 DFS 알고리즘으로 해결한 코드를 확인 해보자.

## 가라! DFS몬!

`[system]성공적으로 문제를 해결했다!`

```java
package 심성헌.알고리즘_5주차;

import java.io.*;
import java.util.*;

public class 단지번호붙이기_2667 {

    static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    static int n, x, y;
    static int length = 0;
    static int cnt = 0;
    static int[][] arr;

    public static void main(String[] args) throws Exception {
        n = Integer.parseInt(br.readLine());
        arr = new int[n][n];
        StringTokenizer[] st = new StringTokenizer[n];
        ArrayList<Integer> list = new ArrayList<Integer>();
        for (int i = 0; i < n; i++) {
            st[i] = new StringTokenizer(br.readLine());
            char[] tmp = st[i].nextToken().toCharArray();
            for (int k = 0; k < tmp.length; k++) {
                arr[i][k] = tmp[k] - '0';
            }
        }
        for (int i = 0; i < arr.length; i++) {
            boolean check = false;
            for (int k = 0; k < arr[i].length; k++) {
                check = dfs(i, k);
                System.out.println("---------------------------");
                if (check) {
                    list.add(cnt);
                    length++;
                    cnt = 0;
                }
            }
        }
        System.out.println(length);
        Collections.sort(list);
        for (int i : list) {
            System.out.println(i);
        }
    }

    public static boolean dfs(int x, int y) {
        if (x < 0 || y < 0 || x >= n || y >= n)
            return false;

        System.out.println("arr[" + x + "][" + y + "] = " + arr[x][y]);
        if (arr[x][y] == 1) {
            arr[x][y] = 5;
            dfs(x - 1, y);
            dfs(x + 1, y);
            dfs(x, y - 1);
            dfs(x, y + 1);
            cnt++;
            return true;
        }
        return false;
    }
}
```

먼저 결과 출력을 확인 해보자.

![결과 콘솔2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FxV9pP%2FbtqMqeqT5JD%2FXnNRpldNBFAuAYotMpDkSk%2Fimg.jpg)

위 스크린샷은 점선을 기준으로 현재의 노드가 조건에 부합할 때 재귀가 발생하는 노드를 출력한 결과다. 즉, Stack 자료 구조를 재귀적으로 구현 했다고 볼 수 있다.

(사실 DFS를 Stack 자료 구조와 재귀 함수를 이용해 구현할 수 있다는 이야기를 듣고 둘의 차이가 무엇인지 전혀 이해하지 못했는데, 이번에 비로소 이해할 수 있었다.)

`main` 메소드에서 인접 행렬의 길이만큼 반복문을 구성하고 그 만큼 `dfs` 메소드를 좌표에 맞게 호출한다. 즉, 모든 노드를 탐색한다는 의미이다.  
그렇다면 위에서 Stack 자료 구조를 재귀적으로 구현 했다고 했는데 이 말의 의미는 뭘까?

Stack 자료 구조는 기본적으로 **FILO(First In Last Out, 선입후출) 구**조로 구성되어 있다.  
만약 위의 문제를 Stack 자료 구조로 해결했을 때, 특정 조건에 부합한다면 **Stack 자료형에 노드의 좌표를 push 했을 것이다.**  
그 후, Stack에 쌓인 만큼 반복하여 한 번 반복을 실행할 때 **pop을 통해 꺼낸 노드 좌표의 상하좌우를 다시 탐색하고, 이 때 조건에 부합한 노드 좌표를 다시 Stack에 push 한다.**

즉, 현재 노드의 상하좌우를 탐색하기 위한 조건은 그 다음 노드를 먼저 탐색한다는 의미이다. 하노이의 탑을 재귀 함수로 풀었던 기억을 되살려 보자.

### 재귀 함수 복습하기

분명 Stack 자료 구조를 이용하여 해결하려고 했는데 코드 구조와 방향성이 재귀 함수를 이용했을 때와 거의 비슷하다는 것을 확인할 수 있다. 그렇다면 JAVA를 이용했을 때 굳이 Stack 클래스를 호출하고, 반복문 코드를 넣어서 코드를 늘리는 것보다 재귀 함수를 이용해서 문제를 풀면 훨씬 쿨한 코드가 되지 않을까?

그래서 재귀적으로 문제를 해결했다.

이 때, 단지의 개수를 먼저 출력하고, 각 단지에 존재하는 아파트의 개수를 오름차순으로 출력해야 하는데 재귀 함수로 코드를 싸는 것보다 값을 출력하기 위한 규칙성을 찾는 것이 더 어려웠다.

해결 방법은 이렇다.

1.  **단지 내 아파트 개수 파악하기**
    - 단지 내에 존재하는 아파트의 개수는 재귀 함수 내에서 조건에 부합할 때마다 `cnt++` 를 호출한다.
    - 단지가 끝난 후에 `cnt` 변수를 초기화 하지 않으면 전체 아파트 개수를 파악하는 꼴이 된다. 그렇기 때문에 아래 조건의 연장선으로 현재 노드가 호출한 재귀 함수의 논리값이 `true` 일 경우 `cnt` 값을 초기화 해준다.
2.  **몇 개의 단지가 존재하는지 파악하기**
    - 현재 노드가 아파트라면 해당 아파트의 단지만큼 재귀 함수를 호출할 것이며, 이는 현재 노드가 호출한 재귀 함수의 논리값은 `true`라는 의미이다.
    - 그렇다면 `true`가 나올 때 단지의 개수를 **+1** 한다.

이렇게 하면 위의 출력문을 통해 파악한 단지 내 아파트의 개수, 단지의 개수를 파악할 수 있다.

문제에서는 단지 내 아파트의 개수를 오름차순으로 정렬하라고 했는데, 이걸 또 익숙한 버블 정렬이나 선택 정렬로 구현하기 귀찮아서 Collentions 객체의 sort() 함수를 이용해 오름차순으로 정렬하고 이를 출력했다.

이번 문제를 풀면서 조금 이해되던 그래프에 대한 개념을 조금 더 이해할 수 있게 되었다.  
BFS, DFS 두 경우 모두 코드로 작성했고, 이렇게 글을 썼기 때문에 앞으로 나아갈 수 있었던 것 같다!  
아직 그래프를 완전히 이해하기에는 까마득하지만 느려도 맞는 방향을 향해 걸어가자!

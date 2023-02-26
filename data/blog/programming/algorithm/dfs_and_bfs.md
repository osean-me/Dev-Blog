---
title: 'DFS(깊이 우선 탐색) / BFS(너비 우선 탐색)'
date: '2020-10-31'
tags: ['Graph', '알고리즘', '코딩 테스트', 'DFS', '깊이 우선 탐색', 'BFS', '너비 우선 탐색']
draft: false
summary: '깊이 우선 탐색과 너비 우선 탐색에 대해 이해한 내용을 정리 했습니다.'
---

# 탐색 알고리즘

이번 주는 탐색 알고리즘의 대표적인 두 가지 `DFS`와 `BFS`에 공부할 예정이다.  
먼저 탐색 알고리즘은 트리 구조로 만들어진 그래프의 노드 그리고 노드와 노드 사이에 연결된 간선의 상태를 파악하기 위해 사용된다.

먼저, 그래프를 데이터로 구현하기 위해서는 먼저 **테이블**에 대한 이해가 있어야 하며, 이는 `인접 행렬` 또는 `인접 리스트`를 이용해서 구현할 수 있다.

# Graph

[참고 아티클](https://gmlwjd9405.github.io/2018/08/13/data-structure-graph.html)

## 인접 행렬

인접 행렬이란 **그래프의 연결 관계**를 **2차원 배열**로 표현하는 방식을 말한다.  
즉, 노드의 개수는 이미 정해져 있으니 각 **_노드(x축 index 번호)_**와 **_노드(y축 index 번호)_** 사이의 관계를 데이터로 저장해 테이블로 구현한다는 의미이다. 그렇기 때문에 인접 행렬은 각각의 노드 간 관계를(논리 혹은 0,1) 파악하면 되기 때문에 **O(1)**의 **시간 복잡도**를 가진다.(n ~ m까지 모든 배열의 방의 관계를 파악한다.)

하지만 V개의 노드와 E개의 간선이 존재할 때 E개의 간선을 파악하기 위해서는 n ~ V까지 배열의 방을 파악하기 때문에 이는 **O(V)** 만큼의 **시간 복잡도**를 가진다. 즉, **불필요한 시간과 메모리를 소모한다는 단점이 있다.**

![인접 행렬](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc8bykY%2FbtqMcRjppZu%2FGhVO3YDkK5DAADog1hSA70%2Fimg.jpg)

## 인접 리스트

인접 리스트란 **리스트 자료형을 이용해 구현**하는 방법이며 인접 행렬이 V개의 노드에서 E개의 간선을 파악하기 위해 O(V)만큼의 시간 복잡도를 가지는 단점을 개선하기 위해 고안된 방법이다.  
이를 구현하기 위해서는 각 **리스트의 index를 노드라고 가정**하고 해당 **노드에 연결되어 있는 노드의 번호를 저장**한다. 그렇기 때문에 각 **인덱스의 길이는 해당 노드에 연결된 노드에 대한 간선의 개수**라는 뜻이다.

파이썬의 경우 자체에서 제공하는 리스트 자료형을 이용해 append() 함수를 활용하여 인접 리스트로 그래프를 구현할 수 있으며, C나 JAVA와 같은 언어는 표준 라이브러리에서 제공하는 List 클래스를 호출하여 구현할 수 있다.

![인접 리스트](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc5TLU8%2FbtqMciPhBM7%2FoU2GwGdJ0XkXDbghn8Twlk%2Fimg.jpg)

# DFS - 깊이 우선 탐색

[참고 아티클](https://gmlwjd9405.github.io/2018/08/14/algorithm-dfs.html)

DFS(Depth First Search), 깊이 우선 탐색이란 현재의 노드(혹은 루트 노드)와 인접한 노드들을 특정한 기준(ex.노드의 번호가 작은 순서대로)으로 **인접한 노드의 제일 깊은 곳까지 탐색하며 모든 노드를 방문**하는 것을 의미한다.

그렇기 때문에 DFS는 **Stack 자료 구조나 재귀 함수**를 통해 구현할 수 있으며 보통은 재귀 함수를 이용해 간결하게 코드를 작성한다.

- **Python - 인접 리스트로 구현한 코드**

```python
# DFS 메서드 정의
def dfs(graph, v, visited):
    # JAVA : stack.push()
    # Python : stack.append()

    # 현재의 노드를 방문 처리
    visited[v] = True
    print(v, end=' ')

    # 현재 노드와 연결된 다른 노드를 재귀적으로 방문
    for i in graph[v]:
        if not visited[i]:
            dfs(graph, i, visited)


graph = [
    [],
    [2, 3, 8],
    [1, 7],
    [1, 4, 5],
    [3, 5],
    [3, 4],
    [7],
    [2, 6, 8],
    [1, 7]
]

visited = [False] * 9

dfs(graph, 1, visited)
```

- **JAVA - DFS를 재귀적으로 구현한 문제 해결 코드**

```java
package 심성헌.알고리즘_4주차;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class 바이러스_2606 {

	static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	static int n;
	static int m;
	static int count;
	static boolean[][] graph;
	static boolean[] visited;

	public static void dfs(int index) {
		visited[index] = true;

		for (int i = 1; i <= n; i++) {
			if (graph[index][i] == true && visited[i] == false) {
				count++;
				dfs(i);
			}
		}
	}

	public static void main(String[] args) throws IOException {
		n = Integer.parseInt(br.readLine());
		m = Integer.parseInt(br.readLine());

		graph = new boolean[n + 1][n + 1];
		visited = new boolean[n + 1];

		for (int i = 1; i <= m; i++) {
			StringTokenizer st = new StringTokenizer(br.readLine());
			int x = Integer.parseInt(st.nextToken());
			int y = Integer.parseInt(st.nextToken());

			graph[x][y] = true;
			graph[y][x] = true;
		}
		dfs(1);
		System.out.println(count);
	}
}
```

깊이 우선 탐색의 경우 모든 노드 간 간선을 조회한다.  
따라서,

- 인접 리스트
  - **O(N + E)**
- 인접 행렬
  - **O(N^2)**

의 시간 복잡도를 가진다. 그렇기 때문에 전체를 탐색하는 깊이 우선 탐색은 너비 우선 탐색보다 느리다.

# BFS - 너비 우선 탐색

[참고 아티클](https://gmlwjd9405.github.io/2018/08/15/algorithm-bfs.html)

BFS(Breadth First Search), 너비 우선 탐색이란 현재의 노드(혹은 루트 노드)의 **인접한 노드를 먼저 탐색**하고, **인접한 노드를 특정 기준의 순서대로 다시 인접 노드의 인접 노드를 탐색**하는 방식을 의미한다.

그렇기 때문에 BFS는 **Queue 자료 구조**를 이용해 구현한다.  
따라서, **재귀적으로 구현할 수 없다는 의미**이다. 해당 알고리즘을 구현할 때 가장 중요한 것은 **방문한 노드에 대해 방문 여부를 검사해야 다음 노드의 탐색을 진행할 수 있다는 점**이다.

- **Python - deque 라이브러리를 이용해 구현**

```python
# BFS 너비 우선 탐색은 큐와 다중 반복문을 이용하여 구현할 수 있다.

from collections import deque

def bfs_function(graph, start, visited):
    # 큐 구현을 위해 deque 라이브러리 사용
    queue = deque([start])
    visited[start] = True

    while queue:
        v = queue.popleft()
        print(v, end=' ')

        for i in graph[v]:
            if not visited[i]:
                queue.append(i)
                visited[i] = True

graph = [
    [],
    [2, 3, 8],
    [1, 7],
    [1, 4, 5],
    [3, 5],
    [3, 4],
    [7],
    [2, 6, 8],
    [1, 7]
]

visited = [False] * 9

bfs_function(graph, 1, visited)
```

- **JAVA - 표준 라이브러리 Queue 클래스를 이용해 구현**

```java
package 심성헌.알고리즘_4주차;

import java.io.IOException;
import java.util.LinkedList;
import java.util.Queue;

public class BFS_너비우선탐색 {

	static Queue<Integer> queue = new LinkedList<Integer>();

	public static void dfs(int[][] graph, int start, boolean[] visited) {
		queue.offer(start);
		visited[start] = true;

		while (queue.size() != 0) {
			int save = queue.poll();
			System.out.print(save + " ");
			for (int i = 0; i < graph[save].length; i++) {
				if (!visited[graph[save][i]]) {
					queue.offer(graph[save][i]);
					visited[graph[save][i]] = true;
				}
			}
		}
	}

	public static void main(String[] args) throws NumberFormatException, IOException {

		int[][] graph = { {}, { 2, 3, 8 }, { 1, 7 }, { 1, 4, 5 }, { 3, 5 }, { 3, 4 }, { 7 }, { 2, 6, 8 }, { 1, 7 } };
		boolean[] visited = new boolean[graph.length + 1];
		for (int i = 1; i < visited.length; i++) {
			visited[i] = false;
		}
		dfs(graph, 1, visited);
	}

}
```

너비 우선 탐색는

- **인접 리스트**
  - **O(N + E)**
- **인접 행렬**
  - **O(N^2)**

의 시간 복잡도를 가진다.

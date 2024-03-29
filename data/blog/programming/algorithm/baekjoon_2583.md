---
title: '[백준 2583] 영역 구하기'
date: '2023-05-11'
tags: ['알고리즘', '코딩 테스트', '백준', '백준 2583', '그래프', 'DFS']
draft: false
summary: '백준 2583 문제 해결 방법을 공유합니다.'
---

## 문제

[백준 2468](https://www.acmicpc.net/problem/2583)

> 눈금의 간격이 1인 M×N(M,N≤100)크기의 모눈종이가 있다. 이 모눈종이 위에 눈금에 맞추어 K개의 직사각형을 그릴 때, 이들 K개의 직사각형의 내부를 제외한 나머지 부분이 몇 개의 분리된 영역으로 나누어진다.
>
> 예를 들어 M=5, N=7 인 모눈종이 위에 \<그림 1\>과 같이 직사각형 3개를 그렸다면, 그 나머지 영역은 \<그림 2\>와 같이 3개의 분리된 영역으로 나누어지게 된다.
>
> M, N과 K 그리고 K개의 직사각형의 좌표가 주어질 때, K개의 직사각형 내부를 제외한 나머지 부분이 몇 개의 분리된 영역으로 나누어지는지, 그리고 분리된 각 영역의 넓이가 얼마인지를 구하여 이를 출력하는 프로그램을 작성하시오.

## 풀이

[제출한 코드](http://boj.kr/931e993a92ca4acf8daa8da2db28359b)

### 체크 포인트 1

처음에는 주어진 이미지처럼 좌측 하단 좌표부터 우측 상단 좌표 사이에 포함된 모든 좌표에 플래그 처리를 했었다.

위와 같이 처리를 했을 때, 직사각형과 마주하는 영역의 모서리가 겹치게 되는 문제가 발생했다.

이를 해결하기 위해서 직사각형에 플래그를 세울 때, 예시 이미지의 모서리마다 플래그를 세우는 것이 아니라 간선 하나를 줄여 정사각형이 이어진 형태가 아닌 라인 형태로 만들어야 한다.

라인 형태로 줄인다는 말은, 주어진 직사각형 좌표를 만들 때 이중 for 문을 우측 상단 좌표 이전까지 반복한다는 것을 의미한다.

이렇게 하면 일반적인 DFS 알고리즘으로 직사각형 이외의 영역의 개수를 파악할 수 있다.

### 체크 포인트 2

직사각형 이외의 영역의 개수를 파악 했다면 해당 영역에 포함된 접점의 개수를 파악해야 한다.

나는 재귀함수를 돌 때 해당 값을 더하고 재귀 내부에서 상하좌우를 돌며 호출하는 `dfs()` 함수의 리턴값을

최초 호출한 `dfs()` 에서 값을 갱신하고 리턴하도록 하면 해당 영역에 포함된 접점의 개수를 파악할 수 있도록 했다.

### 메모

처음에 문제를 풀 때는 어찌저찌 풀릴 듯 하다가도 의도한대로 코드가 돌아가지 않아서 처음부터 차근차근 새로 작성 했는데, 역시 급할 수록 돌아가라는 말이 맞는 듯 하다.

조금 막힐 때는 급하지 않게 처음부터 차근차근 읽어보도록 하자.

```cpp
#include <bits/stdc++.h>
using namespace std;

int m, n, k;
int adj[104][104], visited[104][104];
int dy[] = {-1, 0, 1, 0};
int dx[] = {0, 1, 0, -1};

int dfs(int y, int x, int c) {
    visited[y][x] = 1;
    for (int i = 0; i < 4; i++) {
        int ny = y + dy[i];
        int nx = x + dx[i];
        if (ny < 0 || nx < 0 || ny >= m || nx >= n) continue;
        if (!adj[ny][nx] && !visited[ny][nx]) {
            c = dfs(ny, nx, ++c);
        }
    }
    return c;
}

int main() {
    cin >> m >> n >> k;
    for (int i = 0; i < k; i++) {
        int corner[4];
        for (int j = 0; j < 4; j++) cin >> corner[j];

        for (int y = corner[1]; y < corner[3]; y++) {
            for (int x = corner[0]; x < corner[2]; x++) {
                adj[y][x] = 1;
            }
        }
    }
    int cnt = 0;
    vector<int> areas;
    for (int y = 0; y < m; y++) {
        for (int x = 0; x < n; x++) {
            if (!adj[y][x] && !visited[y][x]) {
                cnt++;
                areas.push_back(dfs(y, x, 1));
            }
        }
    }
    sort(areas.begin(), areas.end());
    cout << cnt << '\n';
    for (int area : areas) {
        cout << area << " ";
    }
}
```

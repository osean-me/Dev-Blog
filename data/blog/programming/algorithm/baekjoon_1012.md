---
title: '[백준 1012] 유기농 배추'
date: '2023-05-08'
tags: ['알고리즘', '코딩 테스트', '백준', '백준 1012', '그래프', 'DFS']
draft: false
summary: '백준 1012 문제 해결 방법을 공유합니다.'
---

## 문제

[백준 1012](https://www.acmicpc.net/problem/1012)

> 차세대 영농인 한나는 강원도 고랭지에서 유기농 배추를 재배하기로 하였다. 농약을 쓰지 않고 배추를 재배하려면 배추를 해충으로부터 보호하는 것이 중요하기 때문에, 한나는 해충 방지에 효과적인 배추흰지렁이를 구입하기로 결심한다. 이 지렁이는 배추근처에 서식하며 해충을 잡아 먹음으로써 배추를 보호한다. 특히, 어떤 배추에 배추흰지렁이가 한 마리라도 살고 있으면 이 지렁이는 인접한 다른 배추로 이동할 수 있어, 그 배추들 역시 해충으로부터 보호받을 수 있다. 한 배추의 상하좌우 네 방향에 다른 배추가 위치한 경우에 서로 인접해있는 것이다.
>
> 한나가 배추를 재배하는 땅은 고르지 못해서 배추를 군데군데 심어 놓았다. 배추들이 모여있는 곳에는 배추흰지렁이가 한 마리만 있으면 되므로 서로 인접해있는 배추들이 몇 군데에 퍼져있는지 조사하면 총 몇 마리의 지렁이가 필요한지 알 수 있다. 예를 들어 배추밭이 아래와 같이 구성되어 있으면 최소 5마리의 배추흰지렁이가 필요하다. 0은 배추가 심어져 있지 않은 땅이고, 1은 배추가 심어져 있는 땅을 나타낸다.

## 풀이

[제출한 코드](http://boj.kr/49ad5bd507ec4a88be96397fd00aa84e)

```cpp
#include <bits/stdc++.h>
using namespace std;

int t = 0, n = 0, m = 0, k = 0, y = 0, x = 0, cnt = 0;
int dy[] = {-1, 0, 1, 0};
int dx[] = {0, 1, 0, -1};
int adj[50][50] = {}, visited[50][50] = {};

void dfs(int cy, int cx) {
    visited[cy][cx] = 1;
    for (int i = 0; i < 4; i++) {
        int ny = cy + dy[i];
        int nx = cx + dx[i];
        if (ny < 0 || nx < 0 || ny >= n || nx >= m) continue;
        if (adj[ny][nx] && !visited[ny][nx]) dfs(ny, nx);
    }
}

int main() {
    cin >> t;
    vector<int> ret = vector<int>();
    for (int i = 0; i < t; i++) {
        cin >> n >> m >> k;
        for (int j = 0; j < k; j++) {
            cin >> y >> x;
            if (y < n && x < m) adj[y][x] = 1;
        }

        for (int yy = 0; yy < n; yy++) {
            for (int xx = 0; xx < m; xx++) {
                if (adj[yy][xx] && !visited[yy][xx]) {
                    cnt++;
                    dfs(yy, xx);
                }
            }
        }
        ret.push_back(cnt);
        n = 0, m = 0, k = 0, y = 0, x = 0, cnt = 0;
        memset(adj, 0, sizeof(adj));
        memset(visited, 0, sizeof(visited));
    }

    for (int result : ret) cout << result << '\n';
}
```

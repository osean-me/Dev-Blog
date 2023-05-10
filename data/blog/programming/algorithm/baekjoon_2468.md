---
title: '[백준 2468] 안전 영역'
date: '2023-05-10'
tags: ['알고리즘', '코딩 테스트', '백준', '백준 2468', '그래프', 'DFS']
draft: false
summary: '백준 2468 문제 해결 방법을 공유합니다.'
---

## 문제

[백준 2468](https://www.acmicpc.net/problem/2468)

> 재난방재청에서는 많은 비가 내리는 장마철에 대비해서 다음과 같은 일을 계획하고 있다. 먼저 어떤 지역의 높이 정보를 파악한다. 그 다음에 그 지역에 많은 비가 내렸을 때 물에 잠기지 않는 안전한 영역이 최대로 몇 개가 만들어 지는 지를 조사하려고 한다. 이때, 문제를 간단하게 하기 위하여, 장마철에 내리는 비의 양에 따라 일정한 높이 이하의 모든 지점은 물에 잠긴다고 가정한다.
> 어떤 지역의 높이 정보가 주어졌을 때, 장마철에 물에 잠기지 않는 안전한 영역의 최대 개수를 계산하는 프로그램을 작성하시오.
>
> 첫째 줄에는 어떤 지역을 나타내는 2차원 배열의 행과 열의 개수를 나타내는 수 N이 입력된다. N은 2 이상 100 이하의 정수이다. 둘째 줄부터 N개의 각 줄에는 2차원 배열의 첫 번째 행부터 N번째 행까지 순서대로 한 행씩 높이 정보가 입력된다. 각 줄에는 각 행의 첫 번째 열부터 N번째 열까지 N개의 높이 정보를 나타내는 자연수가 빈 칸을 사이에 두고 입력된다. 높이는 1이상 100 이하의 정수이다.

## 풀이

[제출한 코드](http://boj.kr/8d8ffa1f37554b58a144896b141690e2)

### 체크 포인트

문제에서 주어진 영역의 행렬의 길이인 N 만 알려줘 처음에는 강수량(비가 온 높이)를 N 을 기준으로 삼는 것으로 이해했다.

하지만 다시 살펴보면 안전한 영역이 최대인 값을 출력하는 것이기 때문에 주어진 지역 별 높이를 Set 자료형에 담아서 중복을 제거하여 각 높이 별 안전 영역의 개수를 계산하고 이를 Vector 에 담도록 했다.

(이 때, 나는 DFS 알고리즘을 사용해 구현 했는데 다른 알고리즘으로도 풀 수 있는지 봐야겠다.)

이 후, Vector 에 담긴 주어진 높이 별 안전 영역 개수 중 최대값만 출력하면 된다.

```cpp
#include <bits/stdc++.h>

using namespace std;

int n;
int adj[104][104], visited[104][104];
int dy[] = {-1, 0, 1, 0};
int dx[] = {0, 1, 0, -1};
set<int> range;

void dfs(int y, int x, int height) {
    visited[y][x] = 1;
    for (int i = 0; i < 4; i++) {
        int ny = y + dy[i];
        int nx = x + dx[i];
        if (ny < 0 || nx < 0 || ny >= n || nx >= n) continue;
        if (adj[ny][nx] < height) continue;
        if (!visited[ny][nx]) dfs(ny, nx, height);
    }
}

int main() {
    // 지역의 행렬 길이
    cin >> n;
    for (int y = 0; y < n; y++) {
        for (int x = 0; x < n; x++) {
            cin >> adj[y][x];
            range.insert(adj[y][x]);
        }
    }

    vector<int> result;
    for (int height : range) {
        int cnt = 0;
        for (int y = 0; y < n; y++) {
            for (int x = 0; x < n; x++) {
                if (adj[y][x] >= height && !visited[y][x]) {
                    cnt++;
                    dfs(y, x, height);
                }
            }
        }
        result.push_back(cnt);
        memset(visited, 0, sizeof(visited));
    }

    cout << *max_element(result.begin(), result.end());
}
```

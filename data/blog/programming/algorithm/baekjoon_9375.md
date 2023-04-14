---
title: '[백준 9375] 나는야 포켓몬 마스터 이다솜'
date: '2023-04-14'
tags: ['알고리즘', '코딩 테스트', '백준', '백준 9375']
draft: false
summary: '백준 9375 문제 해결 방법을 공유합니다.'
---

## 문제

[백준 9375](https://www.acmicpc.net/problem/9375)

> 해빈이는 패션에 매우 민감해서 한번 입었던 옷들의 조합을 절대 다시 입지 않는다. 예를 들어 오늘 해빈이가 안경, 코트, 상의, 신발을 입었다면, 다음날은 바지를 추가로 입거나 안경대신 렌즈를 착용하거나 해야한다. 해빈이가 가진 의상들이 주어졌을때 과연 해빈이는 알몸이 아닌 상태로 며칠동안 밖에 돌아다닐 수 있을까?
>
> 첫째 줄에 테스트 케이스가 주어진다. 테스트 케이스는 최대 100이다.
>
> 각 테스트 케이스의 첫째 줄에는 해빈이가 가진 의상의 수 n(0 ≤ n ≤ 30)이 주어진다.
> 다음 n개에는 해빈이가 가진 의상의 이름과 의상의 종류가 공백으로 구분되어 주어진다. 같은 종류의 의상은 하나만 입을 수 있다.
> 모든 문자열은 1이상 20이하의 알파벳 소문자로 이루어져있으며 같은 이름을 가진 의상은 존재하지 않는다.

## 풀이

[제출한 코드](http://boj.kr/119b2799ce7d45488adca71a6074c833)

```cpp
#include <bits/stdc++.h>

using namespace std;

int t, n;
string a, b;

int main() {
    cin >> t;
    while (t--) {
        map<string, int> _map = map<string, int>();
        cin >> n;
        for (int i = 0; i < n; i++) {
            cin >> a >> b;
            _map[b]++;
        }
        long long let = 1;
        for (auto p: _map) {
            let *= ((long long) p.second) + 1;
        }
        let--;
        cout << let << "\n";
    }
}
```

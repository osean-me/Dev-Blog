---
title: '[백준 10808] 알파벳 개수'
date: '2023-03-27'
tags: ['알고리즘', '코딩 테스트', '백준', '백준 10808']
draft: false
summary: '백준 10808 문제 해결 방법을 공유합니다.'
---

## 문제

[백준 10808](https://www.acmicpc.net/problem/10808)

> 알파벳 소문자로만 이루어진 단어 S가 주어진다. 각 알파벳이 단어에 몇 개가 포함되어 있는지 구하는 프로그램을 작성하시오.

## 풀이

[제출한 코드](http://boj.kr/0f95455bc234403f9be405b88045e407)

```cpp
#include <bits/stdc++.h>

using namespace std;

string s;
int result['z' + 1];

int main() {
  cin >> s;

  for (char c: s) {
    result[c]++;
  }

  for (int i = 'a'; i <= 'z'; i++) {
    cout << result[i] << " ";
  }

  return 0;
}
```

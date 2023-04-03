---
title: '[백준 10988] 펠린드롬인지 확인하기'
date: '2023-03-29'
tags: ['알고리즘', '코딩 테스트', '백준', '백준 10988', '펠린드롬']
draft: false
summary: '백준 10988 문제 해결 방법을 공유합니다.'
---

## 문제

[백준 10988](https://www.acmicpc.net/problem/10988)

> 알파벳 소문자로만 이루어진 단어가 주어진다. 이때, 이 단어가 팰린드롬인지 아닌지 확인하는 프로그램을 작성하시오.
>
> 팰린드롬이란 앞으로 읽을 때와 거꾸로 읽을 때 똑같은 단어를 말한다.
>
> level, noon은 팰린드롬이고, baekjoon, online, judge는 팰린드롬이 아니다.

## 풀이

[제출한 코드](https://www.acmicpc.net/source/58371837)

- 입력한 문자열을 반으로 나누고, 앞 부분과 이를 뒤집은 곳에 위치한 값을 비교하는 방법

```cpp
#include <bits/stdc++.h>
using namespace std;

int solve(string str) {
  int center = (str.length() / 2) + (str.length() % 2);

  // 중간의 전면과 후면을 비교
  for (int i = 0; i < center; i++) {
    int j = str.length() - i - 1;
    if (str[i] != str[j]) return 0;
  }
  return 1;
}

string str;
int main() {
  cin >> str;
  cout << solve(str);
}
```

- 입력한 문자열과 해당 문자열을 반전한 값을 비교하는 방법

```cpp
#include <bits/stdc++.h>
using namespace std;

int solve(string str) {
  string tmp = str;
  reverse(tmp.begin(), tmp.end());
  if (str == tmp) return 1;
  return 0;
}

string str;
int main() {
  cin >> str;
  cout << solve(str);
}
```

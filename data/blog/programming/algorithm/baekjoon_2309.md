---
title: '[백준 2309] 일곱 난쟁이'
date: '2023-03-17'
tags: ['알고리즘', '코딩 테스트', '백준', '백준 2309']
draft: false
summary: '백준 2309 문제 해결 방법을 공유합니다.'
---

## 문제

[백준 2309](https://www.acmicpc.net/problem/2309)

> 왕비를 피해 일곱 난쟁이들과 함께 평화롭게 생활하고 있던 백설공주에게 위기가 찾아왔다. 일과를 마치고 돌아온 난쟁이가 일곱 명이 아닌 아홉 명이었던 것이다.
>
> 아홉 명의 난쟁이는 모두 자신이 "백설 공주와 일곱 난쟁이"의 주인공이라고 주장했다. 뛰어난 수학적 직관력을 가지고 있던 백설공주는, 다행스럽게도 일곱 난쟁이의 키의 합이 100이 됨을 기억해 냈다.
>
> 아홉 난쟁이의 키가 주어졌을 때, 백설공주를 도와 일곱 난쟁이를 찾는 프로그램을 작성하시오.

## 풀이

[제출한 코드](http://boj.kr/07985436436146e4b131664b2b886e7e)

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
  // 입력된 난쟁이들의 키
  int array[9];

  for (int i = 0; i < 9; i++) {
    cin >> array[i];
  }

  // 난쟁이 정렬 (오름차순)
  for (int n = 0; n < 9; n++) {
    for (int m = n + 1; m < 9; m++) {
      if (array[m] <= array[n]) {
        int tmp = array[n];
        array[n] = array[m];
        array[m] = tmp;
      }
    }
  }

  // 난쟁이들의 키 합
  int totalLength = 0;
  for (int i: array) {
    totalLength += i;
  }

  // 제외할 난쟁이들
  int exclude[2];

  // 찾기
  for (int n = 0; n < 9; n++) {
    for (int m = n + 1; m < 9; m++) {
      int result = totalLength - array[n] - array[m];
      if (result == 100) {
        exclude[0] = n;
        exclude[1] = m;
        break;
      }
    }
  }

  cout << "\n";
  // 인덱스
  int index = 0;
  // 결과
  int result[7];

  for (int i = 0; i < 9; i++) {
    if (i != exclude[0] && i != exclude[1]) {
      result[index] = array[i];
      index++;
    }
  }

  for (int i: result) {
    cout << i << "\n";
  }

  return 0;
}
```

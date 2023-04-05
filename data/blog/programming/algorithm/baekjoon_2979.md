---
title: '[백준 2979] 트럭 주차'
date: '2023-03-28'
tags: ['알고리즘', '코딩 테스트', '백준', '백준 2979']
draft: false
summary: '백준 2979 문제 해결 방법을 공유합니다.'
---

## 문제

[백준 2979](https://www.acmicpc.net/problem/2979)

> 상근이는 트럭을 총 세 대 가지고 있다. 오늘은 트럭을 주차하는데 비용이 얼마나 필요한지 알아보려고 한다.
>
> 상근이가 이용하는 주차장은 주차하는 트럭의 수에 따라서 주차 요금을 할인해 준다.
>
> 트럭을 한 대 주차할 때는 1분에 한 대당 A원을 내야 한다. 두 대를 주차할 때는 1분에 한 대당 B원, 세 대를 주차할 때는 1분에 한 대당 C원을 내야 한다.
>
> A, B, C가 주어지고, 상근이의 트럭이 주차장에 주차된 시간이 주어졌을 때, 주차 요금으로 얼마를 내야 하는지 구하는 프로그램을 작성하시오.

## 풀이

[제출한 코드](http://boj.kr/b1515bf5c6e44a3c89a4fa7363918848)

```cpp
#include <bits/stdc++.h>

using namespace std;

int timeCount[101], feeTable[3], parkingTime[3][2], amount;

int main() {
  // 요금 테이블 입력
  scanf("%d %d %d", &feeTable[0], &feeTable[1], &feeTable[2]);

  for (int i = 0; i < 3; i++) {
    // 주차시간 입력
    scanf("%d %d", &parkingTime[i][0], &parkingTime[i][1]);
    // 주차시간만큼 기록
    for (int t = parkingTime[i][0]; t < parkingTime[i][1]; t++) timeCount[t]++;
  }

  // 기록된 주차시간만큼 요금 계산
  for (int i = 1; i < 101; i++)
    amount += (feeTable[timeCount[i] - 1] * timeCount[i]);
  printf("%d", amount);
}

```

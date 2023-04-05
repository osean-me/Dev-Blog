---
title: '[백준 2559] 수열'
date: '2023-04-05'
tags: ['알고리즘', '코딩 테스트', '백준', '백준 2559']
draft: false
summary: '백준 2559 문제 해결 방법을 공유합니다.'
---

## 문제

[백준 2559](https://www.acmicpc.net/problem/2559)

> 매일 아침 9시에 학교에서 측정한 온도가 어떤 정수의 수열로 주어졌을 때, 연속적인 며칠 동안의 온도의 합이 가장 큰 값을 알아보고자 한다
>
> 예를 들어, 아래와 같이 10일 간의 온도가 주어졌을 때,
>
> 3 -2 -4 -9 0 3 7 13 8 -3
>
> 모든 연속적인 이틀간의 온도의 합은 아래와 같다.
>
> 이때, 온도의 합이 가장 큰 값은 21이다. 또 다른 예로 위와 같은 온도가 주어졌을 때, 모든 연속적인 5일 간의 온도의 합은 아래와 같으며, 이때, 온도의 합이 가장 큰 값은 31이다.
>
> 매일 측정한 온도가 정수의 수열로 주어졌을 때, 연속적인 며칠 동안의 온도의 합이 가장 큰 값을 계산하는 프로그램을 작성하시오.

## 풀이

#### [실패한 코드](http://boj.kr/ff52f03d5aa9428ea99d64d22e48883b)

#### 문제점

- 풀 수는 있으나 시간 초과를 고려하지 못한 아이디어
- 시간복잡도 n\*cnt 이나 소요되는 문제. 때문에 최대 길이 100,000 인 경우 시간초과 발생

```cpp
#include <bits/stdc++.h>
#include <vector>

using namespace std;

vector<int> split() {
    string input = "";
    getline(cin, input);
    string delimeter = " ";
    long long pos;
    string token;
    vector<int> result;
    while ((pos = input.find(delimeter)) != string::npos) {
        token = input.substr(0, pos);
        result.push_back(stoi(token));
        input.erase(0, pos + delimeter.length());
    }
    result.push_back(stoi(input));
    return result;
}

int main() {
    vector<int> condition = split();
    int total = condition[0];
    int cnt = condition[1];
    vector<int> days = split();
    int biggest = days[0];
    for (int i = 0; i <= (total - cnt); i++) {
        int sum = 0;
        for (int k = i; k < (i + cnt); k++) sum += days[k];
        if (biggest <= sum) biggest = sum;
        if (biggest == (100 * cnt)) continue;
    }
    cout << biggest;
}
```

### [해결 코드](http://boj.kr/eb55740cfc984e749712f3a7c292dd1d)

```cpp
#include <bits/stdc++.h>
#include <vector>

using namespace std;

vector<int> split() {
    string input = "";
    getline(cin, input);
    string delimeter = " ";
    long long pos;
    string token;
    vector<int> result;
    while ((pos = input.find(delimeter)) != string::npos) {
        token = input.substr(0, pos);
        result.push_back(stoi(token));
        input.erase(0, pos + delimeter.length());
    }
    result.push_back(stoi(input));
    return result;
}

int main() {
    vector<int> condition = split();
    int total = condition[0];
    int cnt = condition[1];
    vector<int> days = split();
    int next = 0;
    for (int i = 0; i < cnt; i++) next += days[i];
    int biggest = next;
    for (int i = 1; i <= total - cnt; i++) {
        next = next - days[i - 1] + days[i + cnt - 1];
        if (biggest < next) biggest = next;
    }
    cout << biggest;
}
```

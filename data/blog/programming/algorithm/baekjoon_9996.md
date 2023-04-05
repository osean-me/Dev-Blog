---
title: '[백준 9996] 한국이 그리울 땐 서버에 접속하지'
date: '2023-04-05'
tags: ['알고리즘', '코딩 테스트', '백준', '백준 9996']
draft: false
summary: '백준 9996 문제 해결 방법을 공유합니다.'
---

## 문제

[백준 9996](https://www.acmicpc.net/problem/9996)

> 선영이는 이번 학기에 오스트레일리아로 교환 학생을 가게 되었다.
>
> 호주에 도착하고 처음 며칠은 한국 생각을 잊으면서 즐겁게 지냈다. 몇 주가 지나니 한국이 그리워지기 시작했다.
>
> 선영이는 한국에 두고온 서버에 접속해서 디렉토리 안에 들어있는 파일 이름을 보면서 그리움을 잊기로 했다. 매일 밤, 파일 이름을 보면서 파일 하나하나에 얽힌 사연을 기억하면서 한국을 생각하고 있었다.
>
> 어느 날이었다. 한국에 있는 서버가 망가졌고, 그 결과 특정 패턴과 일치하는 파일 이름을 적절히 출력하지 못하는 버그가 생겼다.
>
> 패턴은 알파벳 소문자 여러 개와 별표(\*) 하나로 이루어진 문자열이다.
>
> 파일 이름이 패턴에 일치하려면, 패턴에 있는 별표를 알파벳 소문자로 이루어진 임의의 문자열로 변환해 파일 이름과 같게 만들 수 있어야 한다. 별표는 빈 문자열로 바꿀 수도 있다. 예를 들어, "abcd", "ad", "anestonestod"는 모두 패턴 "a\*d"와 일치한다. 하지만, "bcd"는 일치하지 않는다.
>
> 패턴과 파일 이름이 모두 주어졌을 때, 각각의 파일 이름이 패턴과 일치하는지 아닌지를 구하는 프로그램을 작성하시오.

## 풀이

### [실패한 코드](http://boj.kr/bc47118c9e5145f4a7df020da4213eeb)

#### 아이디어

1. 주어진 패턴에서 `*` 가 위치한 인덱스 번호를 찾는다.
2. 입력한 파일명의 `prefix` 와 `suffix` 를 추출한다.
3. 패턴과 대조한 결과를 출력한다.

#### 문제점

1. `substr()` 를 사용하지 않고 무분별한 반복문 사용한 것
2. `find()` 를 사용하지 않고 반복문을 사용해 `*` 의 위치를 찾은 것
3. 파일명의 길이가 패턴의 `prefix` 와 `suffix` 의 길이 합보다 작은 경우를 파악하지 못한 것

```cpp
#include <bits/stdc++.h>
using namespace std;
int cnt, idx;
string pattern;
string startPattern, endPattern;
void find() {
    for (int i = 0; i < 100; i++) {
        if (pattern[i] == '*') {
            idx = i;
            return;
        }
    }
}

int main() {
    cin >> cnt;
    cin >> pattern;
    find();
    for (int i = 0; i < idx; i++) startPattern += pattern[i];
    for (int i = idx + 1; i < pattern.length(); i++) endPattern += pattern[i];
    for (int i = 0; i < cnt; i++) {
        string input;
        cin >> input;
        string start;
        for (int j = 0; j < idx; j++) start += input[j];
        string end;
        for (int k = input.length() - idx; k < input.length(); k++) end += input[k];
        if ((start == startPattern) && (end == endPattern)) {
            cout << "DA" << "\n";
        } else {
            cout << "NE" << "\n";
        }
    }
}
```

### [해결 코드](http://boj.kr/e1f91ae6b9014dcabe9f7340644f29c4)

#### 아이디어

1. `ab*ba` 패턴일 때 파일명이 `aba` 인 경우
   1. 즉, 패턴의 `prefix` / `suffix` 의 길이 합이 파일명보다 긴 경우 `NE` 출력
2. 불필요한 반복문은 제거하고 `find()`, `substr()` 함수 사용

```cpp
#include <bits/stdc++.h>
using namespace std;
int cnt;
string pattern, input;
int main() {
    cin >> cnt;
    cin >> pattern;
    int pos = pattern.find('*');
    string prefix = pattern.substr(0, pos);
    string suffix = pattern.substr(pos + 1);
    for (int i = 0; i < cnt; i++) {
        cin >> input;
        if (prefix.size() + suffix.size() > input.size()) cout << "NE\n";
        else {
            if (input.substr(0, pos) == prefix && input.substr(input.size() - suffix.length()) == suffix)cout << "DA\n";
            else cout << "NE\n";
        }
    }
}
```

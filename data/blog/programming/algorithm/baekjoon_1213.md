---
title: '[백준 1213] 팰린드롬 만들기'
date: '2023-04-16'
tags: ['알고리즘', '코딩 테스트', '백준', '백준 1213']
draft: false
summary: '백준 9375 문제 해결 방법을 공유합니다.'
---

[백준 1213](https://www.acmicpc.net/problem/1213)

> 임한수와 임문빈은 서로 사랑하는 사이이다.
> 임한수는 세상에서 팰린드롬인 문자열을 너무 좋아하기 때문에, 둘의 백일을 기념해서 임문빈은 팰린드롬을 선물해주려고 한다.
> 임문빈은 임한수의 영어 이름으로 팰린드롬을 만들려고 하는데, 임한수의 영어 이름의 알파벳 순서를 적절히 바꿔서 팰린드롬을 만들려고 한다.
> 임문빈을 도와 임한수의 영어 이름을 팰린드롬으로 바꾸는 프로그램을 작성하시오.
>
> 첫째 줄에 임한수의 영어 이름이 있다. 알파벳 대문자로만 된 최대 50글자이다.
>
> 첫째 줄에 문제의 정답을 출력한다. 만약 불가능할 때는 "I'm Sorry Hansoo"를 출력한다. 정답이 여러 개일 경우에는 사전순으로 앞서는 것을 출력한다.

## 풀이

[제출한 코드](http://boj.kr/950ded9d49844db9bbb4f136e720278d)

### 아이디어

#### 팰린드롬으로 만들 수 있는 경우

- 입력한 문자열의 길이가 짝수인 경우, 문자열에 포함된 알파벳의 개수가 모두 짝수여야 한다.
- 입력한 문자열의 길이가 홀수인 경우, 중간에 위치한 알파벳은 홀수이면서 나머지 알파벳은 모두 짝수여야 한다.

#### 팰린드롬으로 만들 수 없는 경우

- 입력한 문자열의 길이가 홀수인 경우, 홀수개인 알파벳 종류가 2개 이상이라면 팰린드롬으로 만들 수 없다.
- 입력한 문자열의 길이가 짝수인 경우, 홀수개인 알파벳 종류가 1개 이상이라면 팰린드롬으로 만들 수 없다.

#### 팰린드롬으로 만드는 방법

- 알파벳 별 개수를 담은 배열을 만든다.
- 해당 배열을 순회하면서
  - 개수가 홀수인 경우는 중간에 위치해야 하는 알파벳이니 별도로 저장한다.
  - 알파벳 개수 / 2 만큼 순회를 돌면서 prefix 와 suffix 를 만든다.
- 입력한 문자열이
  - 홀수인 경우
    - prefix + center + suffix
  - 짝수인 경우
    - prefix + suffix

```cpp
#include <bits/stdc++.h>

using namespace std;

string input = "";
int alphabet[95] = {};

bool checkEven() {
    bool isEven = input.length() % 2 == 0;
    for (char c = 'A'; c <= 'Z'; c++) if (isEven && alphabet[c] % 2 != 0) return false;
    return true;
}

bool checkOdd() {
    bool isOdd = input.length() % 2 != 0;
    if (!isOdd) return true;
    int oddCnt = 0;
    for (char c = 'A'; c <= 'Z'; c++) {
        if (alphabet[c] % 2 != 0) oddCnt++;
        if (oddCnt > 1) return false;
    }
    return true;
}

string palindrome() {
    char center = ' ';
    string prefix = "";
    string suffix = "";
    for (char c = 'A'; c <= 'Z'; c++) {
        int size = alphabet[c];
        if (size % 2 != 0) center = c;
        for (int i = 0; i < size / 2; i++) {
            prefix += c;
            suffix = (c + suffix);
        }
    }
    if (center != ' ') return prefix + center + suffix;
    return prefix + suffix;
}

int main() {
    cin >> input;
    for (int i = 0; i < input.length(); i++) alphabet[input[i]]++;
    if (!checkEven() || !checkOdd()) cout << "I'm Sorry Hansoo";
    else cout << palindrome();
};
```

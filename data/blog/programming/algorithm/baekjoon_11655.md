---
title: '[백준 11655] ROT13'
date: '2023-04-03'
tags: ['알고리즘', '코딩 테스트', '백준', '백준 11655']
draft: false
summary: '백준 11655 문제 해결 방법을 공유합니다.'
---

## 문제

[백준 11655](https://www.acmicpc.net/problem/11655)

> ROT13은 카이사르 암호의 일종으로 영어 알파벳을 13글자씩 밀어서 만든다.
>
> 예를 들어, "Baekjoon Online Judge"를 ROT13으로 암호화하면 "Onrxwbba Bayvar Whqtr"가 된다.
>
> ROT13으로 암호화한 내용을 원래 내용으로 바꾸려면 암호화한 문자열을 다시 ROT13하면 된다.
>
> 앞에서 암호화한 문자열 "Onrxwbba Bayvar Whqtr"에 다시 ROT13을 적용하면 "Baekjoon Online Judge"가 된다.
>
> ROT13은 알파벳 대문자와 소문자에만 적용할 수 있다. 알파벳이 아닌 글자는 원래 글자 그대로 남아 있어야 한다.
>
> 예를 들어, "One is 1"을 ROT13으로 암호화하면 "Bar vf 1"이 된다.
>
> 문자열이 주어졌을 때, "ROT13"으로 암호화한 다음 출력하는 프로그램을 작성하시오.

## 풀이

[제출한 코드](https://www.acmicpc.net/source/58675963)

```cpp
using namespace std;

// 암호화 > 알파벳 위치 +13
// 복호화 > 암호화된 알파벳 위치 +13
// 예외 > 알파벳이 아닌 문자는 패스
// A : 65
// Z : 90
// a : 97
// z : 122

string input;

void encrypt(char c, bool isBig) {
    int start = 'a';
    if (isBig) start = 'A';
    int result = c - start + 13;
    if (26 <= result) result -= 26;
    cout << (char) (result + start);
}

int main() {
    getline(cin, input);

    for (char c: input) {
        if ('A' <= c && c <= 'Z') {
            encrypt(c, true);
        } else if ('a' <= c && c <= 'z') {
            encrypt(c, false);
        } else {
            cout << c;
        }
    }
}
```

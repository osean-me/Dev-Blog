---
title: '[백준 1620] 나는야 포켓몬 마스터 이다솜'
date: '2023-04-10'
tags: ['알고리즘', '코딩 테스트', '백준', '백준 1620']
draft: false
summary: '백준 1620 문제 해결 방법을 공유합니다.'
---

## 문제

[백준 1620](https://www.acmicpc.net/problem/1620)

> 첫째 줄에는 도감에 수록되어 있는 포켓몬의 개수 N이랑 내가 맞춰야 하는 문제의 개수 M이 주어져. N과 M은 1보다 크거나 같고, 100,000보다 작거나 같은 자연수인데, 자연수가 뭔지는 알지? 모르면 물어봐도 괜찮아. 나는 언제든지 질문에 답해줄 준비가 되어있어.
>
> 둘째 줄부터 N개의 줄에 포켓몬의 번호가 1번인 포켓몬부터 N번에 해당하는 포켓몬까지 한 줄에 하나씩 입력으로 들어와. 포켓몬의 이름은 모두 영어로만 이루어져있고, 또, 음... 첫 글자만 대문자이고, 나머지 문자는 소문자로만 이루어져 있어. 아참! 일부 포켓몬은 마지막 문자만 대문자일 수도 있어. 포켓몬 이름의 최대 길이는 20, 최소 길이는 2야. 그 다음 줄부터 총 M개의 줄에 내가 맞춰야하는 문제가 입력으로 들어와. 문제가 알파벳으로만 들어오면 포켓몬 번호를 말해야 하고, 숫자로만 들어오면, 포켓몬 번호에 해당하는 문자를 출력해야해. 입력으로 들어오는 숫자는 반드시 1보다 크거나 같고, N보다 작거나 같고, 입력으로 들어오는 문자는 반드시 도감에 있는 포켓몬의 이름만 주어져. 그럼 화이팅!!!

## 풀이

[제출한 코드](http://boj.kr/700829ec66d64da09cb8b300753299ce)

```cpp
#include <bits/stdc++.h>
#include <vector>

using namespace std;

vector<string> split(string input, string delimeter) {
    long long pos = 0;
    string token = "";
    vector<string> result;

    while ((pos = input.find(delimeter)) != string::npos) {
        token = input.substr(0, pos);
        result.push_back(token);
        input.erase(0, pos + delimeter.length());
    }
    result.push_back(input);
    return result;
}

vector<string> input(int n) {
    vector<string> result;
    for (int i = 0; i < n; i++) {
        string input = "";
        getline(cin, input);
        result.push_back(input);
    }
    return result;
}

map<string, int> createMap(vector<string> dictionary) {
    map<string, int> result;
    for (int i = 0; i < dictionary.size(); i++) {
        result[dictionary[i]] = i + 1;
    }
    return result;
}

int main() {
    // 포켓몬 수, 문제수 입력
    string nm = "";
    getline(cin, nm);
    vector<string> result = split(nm, " ");
    // Index To Name
    vector<string> indexToName = input(stoi(result[0]));
    // Name To Index
    map<string, int> nameToIndex = createMap(indexToName);
    // 문제의 포켓몬
    vector<string> pokemons = input(stoi(result[1]));
    for (string pokemon: pokemons) {
        // nameToIndex 의 값이 0 이면 이름 출력 아니면 인덱스 출력
        if (nameToIndex[pokemon] == 0) cout << indexToName[stoi(pokemon) - 1] << "\n";
        else cout << nameToIndex[pokemon] << "\n";
    }
}
```

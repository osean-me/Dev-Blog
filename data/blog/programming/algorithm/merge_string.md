---
title: '문자열 합성 및 완전 탐색'
date: '2020-12-10'
tags: ['Cos Pro 1급', '알고리즘', '코딩 테스트', '완전 탐색']
draft: false
summary: 'Cos Pro 1급 기출 문제인 문자열 합성을 풀고 이를 풀기 위해 필요한 개념인 완전 탐색에 대해 이해한 내용을 정리 했습니다.'
---

# 문제

[Arori | 당신의 지식](http://www.sysout.co.kr/arori/classes/quiz/play/415)

두 문자열 s1과 s2를 붙여서 새 문자열을 만들려 합니다. 이때한 문자열의 끝과 다른 문자열의 시작이 겹친다면 겹치는 부분은 한 번만 적습니다.

예를 들어 s1 = 'ababc' / s2 = 'abcdab'일 때아래와 같이 s1 뒤에 s2를 붙이면 새 문자열의 길이는 9입니다.

![예시 이미지](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbt8zPo%2FbtqPJLMKS86%2FvOVBVDaI0SzBxV1GKF2k8K%2Fimg.png)

그러나 s2 뒤에 s1을 붙이면 새 문자열의 길이는 8로 더 짧게 만들 수 있습니다.

![예시 이미지](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FtUccH%2FbtqPAuMvZWP%2F5QxVKilITl5xpj4kjbA44k%2Fimg.png)

두 문자열 s1과 s2가 매개변수로 주어질 때 s1과 s2를 붙여서 만들 수 있는 문자열 중 가장 짧은 문자열의 길이를 return 하도록 solution 메소드를 완성해주세요.

---

### 매개변수 설명

두 문자열 s1과 s2가 solution 메소드의 매개변수로 주어집니다.

- s1과 s2의 길이는 1 이상 100 이하입니다.
- s1과 s2는 알파벳 소문자로만 이루어져 있습니다.

---

### return 값 설명

s1과 s2를 붙여서 만들 수 있는 문자열 중가장 짧은 문자열의 길이를 return 해주세요.

---

### 예시

| s1      | s2       | return |
| ------- | -------- | ------ |
| "ababc" | "abcdab" | 8      |

### 예시 설명

문제에 주어진 예시와 같습니다.

# 풀이

## 설명

s1의 뒷 부분과 s2의 앞 부분의 문자열이 같을 경우 합성된다. 나는 해당 문제를 **완전 탐색**으로 풀었다.

1. 문자열 s1 을(를) 뒤집은 후 char 타입으로 배열을 만든다.
2. 뒤집은 문자열 s1를 기준으로 반복문을 실행해 s2의 첫 번째 인덱스부터 비교한다.
   - 만약 일치하는 인덱스가 있을 경우 s2의 현재 인덱스를 기준으로 각 문자열의 현재 위치한 인덱스의 다음 인덱스가 맞는지 탐색한다.

## 코드

```java
package 모바일리더_코딩테스트_03;

public class question_04 {
	public int solution(String s1, String s2) {
		// s1 문자열을 뒤집어 배열로 저장한다.
		char[] arr1 = new char[s1.length()];
		char[] arr2 = s2.toCharArray();
		int t = 0;
		for (int i = s1.length() - 1; i > -1; i--) {
			arr1[t] = s1.charAt(i);
			t++;
		}

		// 뒤집힌 s1의 0 번째 인덱스와 s2의 i 번째 인덱스가 같아야
		// s1의 1... 번째와 s2의 i + 1 번째 인덱스를 비교할 수 있다.
		int count = 0;
		for (int idx = 0; idx < arr2.length; idx++) {
			// s2 idx 번째 인덱스 중 뒤집힌 s1의 0 번째 인덱스와 같은 데이터를 찾는다.
			if (arr1[0] == arr2[idx]) {
				System.out.println("arr1[" + 0 + "] = " + arr1[0] + " / arr2[" + idx + "] = " + arr2[idx]);
				int k = 1;
				// 위의 조건에 부합하다면
				for (int i = idx - 1; i > -1; i--, k++) {
					// s2의 인덱스를 다시 역순으로 탐색한다.
					// s1의 인덱스를 순서대로 탐색한다.
					if (k < arr1.length) {
						count++;
						System.out.println("arr1[" + k + "] = " + arr1[k] + " / arr2[" + i + "] = " + arr2[i]);
						if (arr1[k] != arr2[i]) {
							break;
						}
					}
				}
				break;
			}
		}
		count += 1;

		StringBuffer sb = new StringBuffer();
		sb.append(s1);
		sb.append(s2.substring(count));
		System.out.println("합성 가능 인덱스 개수 = " + count);
		System.out.println("문자열 합성 결과 = " + sb.toString());
		int answer = sb.toString().length();
		return answer;
	}

	// 아래는 테스트케이스 출력을 해보기 위한 main 메소드입니다.
	public static void main(String[] args) {
		question_04 sol = new question_04();
		String s1 = new String("ababcz");
		String s2 = new String("zabcdab");
		int ret = sol.solution(s1, s2);

		// [실행] 버튼을 누르면 출력 값을 볼 수 있습니다.
		System.out.println("solution 메소드의 반환 값은 " + ret + " 입니다.");
	}
}
```

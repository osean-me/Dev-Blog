---
title: '비숍이 이동 가능한 칸 개수 찾기'
date: '2020-12-10'
tags: ['Cos Pro 1급', '알고리즘', '코딩 테스트']
draft: false
summary: 'Cos Pro 1급 기출 문제를 풀고 이해한 내용을 정리 했습니다.'
---

# 문제

[Arori | 당신의 지식](http://www.sysout.co.kr/arori/classes/quiz/play/415)

체스에서 비숍(Bishop)은 아래 그림과 같이 대각선 방향으로 몇 칸이든 한 번에 이동할 수 있습니다. 만약한 번에 이동 가능한 칸에 체스 말이 놓여있다면 그 체스 말을 잡을 수 있습니다.

![예시 이미지](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FP1Q7Z%2FbtqPNxAcrHb%2F4GNODhUUt3u2UH0SWeVZp0%2Fimg.png)

8 x 8 크기의 체스판 위에 여러 개의 비숍(Bishop)이 놓여있습니다. 이때비숍(Bishop)들에게 **한 번에** 잡히지 않도록 새로운 말을 놓을 수 있는 빈칸의 개수를 구하려고 합니다.

위 그림에서 원이 그려진 칸은 비숍에게 한 번에 잡히는 칸들이며 따라서 체스 말을 놓을 수 있는 빈칸 개수는 50개입니다.

8 x 8 체스판에 놓인 비숍의 위치 bishops가 매개변수로 주어질 때 비숍에게 한 번에 잡히지 않도록 새로운 체스 말을 놓을 수 있는 빈칸 개수를 return 하도록 solution 메소드를 완성해주세요.

---

### 매개변수 설명

체스판에 놓인 비숍의 위치 bishops가 solution 메소드의 매개변수로 주어집니다.

- bishops는 비숍의 위치가 문자열 형태로 들어있는 배열입니다.
- bishops의 길이는 1 이상 64 이하입니다.
- 비숍이 놓인 위치는 알파벳 대문자와 숫자로 표기합니다.
  - 알파벳 대문자는 가로 방향숫자는 세로 방향 좌표를 나타냅니다.
  - 예를 들어 위 그림에서 비숍이 있는 칸은 'D5'라고 표현합니다.
- 한 칸에 여러 비숍이 놓이거나잘못된 위치가 주어지는 경우는 없습니다.

---

### return 값 설명

비숍에게 한 번에 잡히지 않도록 새로운 체스 말을 놓을 수 있는 빈칸의 개수를 return 해주세요.

---

### 예시

| bishops              | return |
| -------------------- | ------ |
| \["D5"\]             | 50     |
| \["D5", "E8", "G2"\] | 42     |

### 예시 설명

예시 #1문제에 나온 예시와 같습니다.

예시 #2

![예시 이미지](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlLhT2%2FbtqPC1i3Zmo%2FfCB6ekUIx4iFa2BVPyMtKK%2Fimg.png)

그림과 같이 원이 그려진 칸은 비숍에게 한 번에 잡히는 칸들이며 따라서 체스 말을 놓을 수 있는 빈칸 개수는 42개입니다.

# 풀이

## 설명

![풀이 설명](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbPwGsW%2FbtqPC1XDnSu%2FyBoE3GLSH1VbU5rrAHZsN1%2Fimg.png)

나는 해당 문제를 먼저 비숍이 이동 가능한 좌표의 값을 계산하려고 했다. 먼저 체스판은 8\*8의 크기를 가지므로 비숍의 좌표는 8를 초과하거나 -1보다 작을 수 없다.

그리고 비숍은 대각선으로 상하좌우를 이동하기 때문에 현재 비숍의 위치 기준

- **x축**
  - **증가하는 영역**
    - nx = i + 주어진 좌표의 x값
    - ny = i + 주어진 좌표의 y값
  - **감소하는 영역**
    - mx = 주어진 좌표의 x값 - 1
    - my = 주어진 좌표의 y값 - 1
- **y축**
  - **증가하는 영역**
    - ix = nx
    - iy = ny - (현재 인덱스 \* 2)
  - **감소하는 영역**
    - jx = nx - (현재 인덱스 \* 2)
    - jy = ny

이렇게 총 4가지로 나누어 연산할 수 있도록 아이디어를 짰다.

그리고 방문한 좌표는 현재의 비숍이 이동할 수 있는 칸이므로 방문 체크를 해놓은 뒤 체스판을 다시 조회해서 방문한 칸의 개수를 세어서 주어진 비숍이 이동가능한 칸의 개수를 파악할 수 있게 한다.

## 코드

```java
package 모바일리더_코딩테스트_03;

public class question_03 {
	public int solution(String[] bishops) {
		// 여기에 코드를 작성해주세요.

		// 0. 비숍 위치 인덱스 번호로 변환
		int[][] idxTmp = new int[bishops.length][2];
		// 1. 비숍 좌표 방문 설정
		int[][] xy = new int[8][8];

		// 2. 비숍의 현재 위치 방문 체크
		for (int i = 0; i < idxTmp.length; i++) {
			idxTmp[i][0] = bishops[i].charAt(0) - 'A';
			idxTmp[i][1] = bishops[i].charAt(1) - '1';
			xy[idxTmp[i][0]][idxTmp[i][1]] += 1;
		}
		// 4. 비숍 이동 가능 거리 방군 설정 및 개수 세기
		// 4-1. 현재 비숍 위치 위 아래 로 이동하면서 벽에 부딪히는지, 겹치는 비숍 혹은 다른 말은 없는지 확인
		for (int go = 0; go < bishops.length; go++) {
			int x = idxTmp[go][0], y = idxTmp[go][1];
			int i = 0;
			while (i < 8) {
				// x축을 기준으로
				int nx = i + idxTmp[go][0], ny = i + idxTmp[go][1];
				int mx = x - 1, my = y - 1;
				// y축을 기준으로
				int ix = nx, iy = ny - (i * 2);
				int jx = nx - (i * 2), jy = ny;

				// x축 기준 증가하는 영역
				if (nx < 8 && ny < 8 && nx > -1 && ny > -1) {
					xy[nx][ny] = 1;
				}
				// x축 기준 감소하는 영역
				if (mx < 8 && my < 8 && mx > -1 && my > -1) {
					xy[mx][my] = 1;
				}

				// y축 기준 증가하는 영역
				if (ix < 8 && iy < 8 && ix > -1 && iy > -1) {
					xy[ix][iy] = 1;
				}
				// y축 기준 감소하는 영역
				if (jx < 8 && jy < 8 && jx > -1 && jy > -1) {
					xy[jx][jy] = 1;
				}

				x = mx;
				y = my;
				i++;
			}
		}

		int answer = 0;
		for (int i = 0; i < xy.length; i++) {
			for (int k = xy.length - 1; k > -1; k--) {
				System.out.print(xy[i][k] + " ");
				if (xy[i][k] == 0) {
					answer++;
				}
			}
			System.out.println();
		}
		System.out.println("answer = " + answer);
		return answer;
	}

	// 아래는 테스트케이스 출력을 해보기 위한 main 메소드입니다.
	public static void main(String[] args) {
		question_03 sol = new question_03();
		String[] bishops1 = { new String("D5") };
		int ret1 = sol.solution(bishops1);

		// [실행] 버튼을 누르면 출력 값을 볼 수 있습니다.
		System.out.println("solution 메소드의 반환 값은 " + ret1 + " 입니다.");
		System.out.println();
		String[] bishops2 = { new String("D5"), new String("E8"), new String("G2") };
		int ret2 = sol.solution(bishops2);

		// [실행] 버튼을 누르면 출력 값을 볼 수 있습니다.
		System.out.println("solution 메소드의 반환 값은 " + ret2 + " 입니다.");
	}
}
```

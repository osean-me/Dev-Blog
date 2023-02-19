---
title: '이벤트 기반 시스템에 대해'
date: '2020-09-26'
tags: [' Node.js', 'Blocking', 'Non Blocking', 'Event']
draft: false
summary: 'Node.js 의 이벤트 기반 시스템에 대해 학습하고 이해한 내용을 정리 했습니다.'
---

![Node.js Logo](https://upload.wikimedia.org/wikipedia/commons/d/d9/Node.js_logo.svg)

# 들어가기에 앞서

프로젝트를 진행하면서 많은 비동기들을 어떠한 이벤트 핸들러를 적용하여 구현하였는데, 왜 비동기 시스템을 구현하려면 이벤트 핸들러를 사용해야만 할까? 또, 이벤트 핸들러가 돌아가는 과정은 어떻게 구성되어 있을까?

어제 공부한 내용을 다시 살펴보면, Node.js 는 V8 엔진과 libuv로 구성되어 있고, libuv는 **이벤트 기반 시스템**, **비동기 논 블로킹 I/O 모델**을 구현한다고 했다. 오늘은 이벤트 기반 시스템에 대해 공부해보려고 한다.

---

# 이벤트?

이벤트는 시스템 하드웨어나 소프트웨어 상태의 변화라는 의미를 가진다. 이벤트는 사용자의 클릭, 마우스의 움직임, 키보드 입력이나 웹 페이지의 특정 영역으로의 이동 등 외부의 환경에 의해 생성되거나 프로그램 로딩과 같은 시스템에 의해 발생할 수 있다.

# 이벤트 기반?

그렇다면 Node.js 에서 말하는 **이벤트 기반**이란 무엇일까?

이벤트 기반이라는 것은 특정 이벤트 발생 시 미리 예약 혹은 지정해놓은 작업을 수행하는 방식을 말한다.

예를 들어 게시글 취소 버튼을 클릭(클릭 이벤트 발생)하면 게시글이 삭제된다는 모달이나 alert 창을 띄우는 것 말이다.

이러한 이벤트 기반 시스템에서는 지정한 작업을 수행하기 위해서 작업이 일어나야 할 시점에 이벤트를 미리 등록해두어야 한다.

이러한 행위를 "**이벤트 리스터(Event Listner) 에게 콜백(callback) 함수를 등록한다."라고** 한다.

Node.js 또한 이러한 이벤트 기반 시스템에 의해 동작하기에 이벤트가 발생하면 이벤트 리스너에 등록해둔 **콜백 함수를 호출한다.**

발생한 이벤트가 없거나, 발생한 이벤트를 모두 처리하면 Node.js 는 다음 이벤트가 발생할 때까지 대기한다.

## 이벤트 루프(Event Loop) / 태스크 큐(Task Queue) / 백그라운드(Background)?

이벤트 핸들러와 콜백, 그리고 이를 바탕으로 이벤트 기반 시스템에 대해 밑바탕이 그려졌다면 그다음으로 이벤트 루프라는 개념이 나온다.

이벤트 루프라는 것은 뭘까?

Javascript는 **단일 스레드(Single Thread)** 프로그래밍 언어로써 **Call Stack** 이 하나라는 말이다. 즉, 작업의 순서대로 하나씩 처리한다는 의미를 가진다. Javascript 코드 내에서 함수 호출 부분이 있다면 함수들을 **Call Stack** 영역에 넣고, 그다음 코드들이 쌓인다. (맨 위부터 한 줄씩 실행한다.)

```javascript
function first() {
  second()
  console.log('첫 번째 호출')
}

function second() {
  third()
  console.log('두 번째 호출')
}

function third() {
  console.log('세 번째 호출')
}

first()
```

위의 코드를 통해 호출의 구조를 확인해보자.

가장 먼저 **first() 함수가 호출되고, 그 안에 있는 second() 함수가 호출되며 second() 함수 안에 있는 third() 함수가 호출되면서**

**세 번째 호출**

**두 번째 호출**

**첫 번째 호출**

순으로 console에 출력된다.

또한 위의 함수들이 호출 스택에 쌓이기 전에 가장 먼저 실행 컨텍스트(전역 컨텍스트) 가 쌓이고 해당 컨텍스트 위에 다음에 올 함수들이 쌓이게 된다. 여기서 실행 컨텍스트(전역 컨텍스트) 는 코드로 작성된 함수가 호출되었을 때 생성되는 환경을 의미한다.

이 말은 Javascript 코드는 실행 시 기본적으로 전역 컨텍스트 안에서 돌아간다고 생각하는 게 좋다.(고 한다..)

모든 함수가 호출되었다면 third(), second(), first(), 실행 컨텍스트 순으로 호출 스택에서 지워지며, 모두 지워지게 되면 호출 스택은 비워지게 된다.

위의 코드에서 setTimeout() 함수가 들어가게 된다면 setTimeout() 함수 내의 콜백 함수를 호출 스택으로 설명할 수 있을까?

출력되는 순서에 대해서는 쉽게 설명할 수 있겠지만 호출 스택에 언제 들어가는지에 대해 설명하라고 하면 많이 힘들 것 같다!

이렇게 호출 스택에 들어가는 순서나 나오는 순서 등에 대해 명확히 설명하려면

**이벤트 루프, 태스크 큐, 백그라운드** 개념에 대해 파악하고 있어야 한다.(라고 했다..)

그렇다면 위의 3가지 개념은 도대체 무엇일까?

### 이벤트 루프 (Event Loop)?

이벤트 발생 시 호출할 콜백 함수를 관리하고, 호출된 콜백 함수의 실행 순서를 결정하는 역할을 담당한다. 노드가 종료될 때까지 이벤트 처리를 위한 작업을 반복하기에 루프라고 부른다.(카더라..)

### 백그라운드 (Background)?

setTimeout() 같은 타이머나 이벤트 리스너들이 대기하는 곳이다. 여러 작업이 동시에 실행될 수 있다.  
(스레드의 영역이라고 볼 수 있는 걸까? Javascript 는 싱글 스레드라고 공부했던 기억이 있는데, 싱글 스레드인 상태에서 어떻게 동시에 여러 작업이 실행될 수 있는 걸까? -> 검색해서 알아본 결과, 이는 메인 스레드인 이벤트 루프가 싱글 스레드이며, 이벤트 루프는 이벤트의 순서에 맞게 함수를 실행한다! 맞을까..? )

### 태스크 큐 (Task Queue)?

이벤트 발생 후, 백그라운드에서는 태스크 큐로 타이머나 이벤트 리스너의 콜백 함수를 보낸다. 정해진 순서대로 콜백들이 줄 서 있기 때문에 Callback Queue라고 부르리도 한다. 콜백 함수들은 보통 완료된 순서대로 줄을 서 있지만 특정한 경우에는 순서가 바뀌기도 한다.(더라..)

내가 이해한 대로라면 호출된 함수는 호출 스택에 들어가고 해당 함수 안에 있는 콜백 함수는 백그라운드 영역으로 이동하며 백그라운드에서 해당 콜백 함수를 명령에 맞게 작업을 진행한 후 해당 콜백 함수를 태스크 큐(콜백 큐)로 이동시킨다.

하나의 이벤트 루프 안에서 함수가 호출 스택에 쌓이고, 함수가 쌓인 순서대로 각각의 함수의 콜백 함수가 백그라운드로 이동한 후 해당 함수의 작업을 수행하고 완료하면 태스크 큐에 각각의 콜백 함수가 쌓인다! (라고 이해했는데, 맞는 것인지 모르겠다.)

이러한 상태에서 호출 스택에 존재하던 함수가 모두 실행되고 난 뒤 지워지게 되면 이벤트 루프는 태스크 큐에서 함수를 하나씩 가지고 와서 호출 스택에 넣고 실행한다.

그런 후, 호출 스택에서 실행된 함수는 지워지게 되며 호출 스택은 비워지게 된다. 이때, 이벤트 루프는 다음 콜백 함수가 태스크 큐에 쌓여 실행될 때까지 대기한다.

제로초 님 블로그 글을 보다 거기에 링크되어 있는 유튜브 영상을 시청하게 되었는데 명확히 내 것이 된 것은 아니지만 자바스크립트가 비동기로 어떻게 돌아가는지 머릿속에서 밑그림을 그릴 수 있게 되었다!

**즉, Web API들이 Javascript 에서 호출이 되면 해당 함수들이 콜 스택에 쌓이고 해당 콜백 함수들이 백그라운드로 이동한 뒤 각 함수에 맞는 작업을 백그라운드에서 마무리한 후 태스크 큐로 이동하여 순서대로 쌓인다. 그 이후 자바스크립트의 콜 스택에 쌓인 순서대로 작업이 완료되어 콜 스택이 비었다면 태스크 큐에 쌓인 콜백 함수들이 순서대로 콜 스택으로 이동하여 마무리 작업을 수행한다. 이 것이 이벤트 루프인 것이다! (맞나..?)**

---

# 두 번째 공부를 마무리하며

이 영상과 제로초님의 블로그 게시글을 통해 명확하지 않지만 그래도 머리카락 하나 정도는 이해가 된 것 같다.

이번 공부를 통해 자료구조에 대해 이해가 많이 필요하다는 것을 느꼈다.

오늘 저녁에는 자료구조에 대해서도 공부를 조금 하고, 낮에는 이 영상의 3분의 2 정도를 시청했는데 오늘 저녁에 나머지 뒷부분도 마저 시청해서 개념 이해에 박차를 가해야겠다!
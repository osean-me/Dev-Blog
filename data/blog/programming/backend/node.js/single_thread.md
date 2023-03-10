---
title: 'Single Thread'
date: '2020-09-27'
tags: ['Node.js', 'Singe Thread', 'Multi Thread', 'Thread']
draft: false
summary: 'Node.js 는 왜 Single Thread 를 기반으로 했는지 학습하고 이에 대해 이해한 내용을 정리 했습니다.'
---

![Node.js Logo](https://upload.wikimedia.org/wikipedia/commons/d/d9/Node.js_logo.svg)

# 들어가기에 앞서

> **Javascript 는 싱글 스레드이다.**

Node.js 공부를 시작한 지 3일 차이다. 앞서 공부한 비동기 논 블로킹 I/O 모델과 이벤트 기반 시스템을 통해 Node.js 가 어떤 시스템을 가지고 작동하는지 감을 잡을 수 있었다. 하지만 아직 부족한 부분이 있는데, 위의 두 부분을 공부하면서 Javascript 는 **싱글 스레드라는** 말이 굉장히 많이 나왔다.

그렇다면 싱글 스레드는 뭐고, 더 깊이 들어가서 스레드의 정의는 무엇일까?

오늘은 그 **스레드**에 대해서 공부하는 시간을 가지려고 한다.

---

# 스레드?

요즘 인터넷 밈으로 떠도는 사진 한 장이 있다.

![Meme Image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FmosSd%2FbtqJFZQYmHr%2FnPhIKrl0QgIkfwOs9OLPnK%2Fimg.jpg)

위의 사진에서 노예라는 예가 좀 맞지는 않겠지만 스레드를 이해하기 위해서는 **프로세스**라는 것에 대한 이해가 필요하다.

# 프로세스?

- 프로세스라는 것은 **운영체제에서 할당하는 작업의 단위**다. 비유를 하자면 내가 고용해서 일을 시킨 알바생이라고 생각해도 괜찮을 것 같다. 이러한 프로세스의 영역에 속하는 것은 노드나 웹 브라우저 등이 있는데 이들은 **개별적인 프로세스이며 이들 프로세스 간에는 메모리를 공유하지 않는다.**

# 다시 한 번 스레드?

- 즉, 스레드는 알바생(프로세스)이 몇 개의 손(스레드)을 가지고 있는지를 말한다. 프로세스는 스레드를 **여러 개 생성해 여러 작업을 동시에 처리**할 수 있게 도와주며, 이러한 스레드는 **부모 프로세스의 자원을 공유**한다.

여기서 Node.js는 싱글 스레드이며, 이러한 이유 때문에 위에서 아래로 순서대로 작업을 수행한다.

엄밀히 말하면 Node.js 는 싱글 스레드는 아니다. 프로세스가 생성될 때 여러 개의 스레드들을 생성하는데, 우리가 직접 컨트롤할 수 있는 스레드의 개수가 하나밖에 없기 때문에 싱글 스레드라고 표현한다.(라고 하더라..)

그렇다면 Node.js는 이벤트 기반 시스템, 비동기 논 블로킹 I/O 모델의 특징을 가지고 있는데 싱글 스레드이면서 어떻게 한 번에 여러 가지 작업을 할 수 있는 걸까?

쉽게 이해하자면 Node.js 혼자서만 일을 한다면 싱글 스레드이기 때문에 한 시점에서 여러 가지 일을 처리할 수 없었을 것이다.

하지만 앞서 공부한 Web API(Ajax, 브라우저 - 백그라운드와 태스크 큐)와 함께 일한다면 이야기는 조금 달라질 것이다.

이렇게 단편적으로 스레드에 이야기하면 **_스레드는 무조건 많은 게 좋은 거 아니야?라고_** 생각하기 쉽다.

하지만 **과유불급**이라는 말처럼 많다고 무조건 좋거나 적다고 좋지 않은 게 아니다.

상황에 맞게 사용하면 될 뿐이다.

그렇다면 싱글 스레드일 때와 멀티 스레드일 때의 장단점을 비교 분석해보자!

---

# 싱글, 멀티 스레드?

그래서 어쩌고 저쩌고인데?

앞의 내용을 연결하여 스레드가 한 명의 알바생이라고 생각하자.

## 예시 1

라멘집에 테이블이 총 3 테이블이 있고, 알바생(스레드)이 한 테이블에 대한 주문을 완전히 처리해야 다음 손님의 주문을 받고 처리할 수 있다.

이러한 예시가 바로 **싱글 스레드, 블로킹 모델이다.**

이러한 구조라면 첫 번째 손님만 있을 경우는 괜찮은데(작업이 단 하나일 경우) 첫 번째 손님 이후에 손님이 계속 들어온다면?

확실히 비효율 적이라고 할 수 있다. 왜냐하면 두 번째 손님은 첫 번째 손님에 대한 일을 알바생이 모두 처리할 때까지 마냥 기다려야 하기 때문이다. 비효율적이라고 말할 수 있다.

## 예시 2

그렇다면 이번에는 알바생이 첫 번째 손님의 요리가 나올 때까지 기다리지 않고 주문만 주방으로 넘긴 뒤 다음 손님의 주문을 받으면 어떨까? 그러면 앞 전의 상황보다 확실히 효율적이다.  
(물론 요리의 특성에 따라 나오는 순서는 달라질 수 있다. -> 블로킹인지 논블로킹인지에 따라서)

이러한 방식이 Node.js 에서 추구하는 **싱글 스레드, 논 블로킹 모델이다.**

하지만 이러한 방식도 문제를 발생시킬 수 있다. 알바생이 아파서 못 나오거나 요리 시간이 오래 걸린다면 말이다.

## 예시 3

이번 예시에서 알바생의 수를 테이블 개수만큼 늘린다면 어떨까?

기존에 알바생 한 명이 하던 일을 나머지 알바생들에게 분배할 수 있으니 서빙을 더 빨리 수행할 수 있다.

이러한 방식이 **멀티 스레드, 블로킹 모델이다.**

하지만 이러한 방식도 마냥 좋지는 못한 이유는 라멘집이 잘돼서 가게를 확장하면서 테이블 개수를 3개에서 10개, 100개로 늘리게 된다면 알바생도 테이블 개수만큼 늘려야 하는 것이다. 100개의 테이블에 손님이 있다면 100명의 알바생들이 쉬지 않고 적절히 일할 수 있지만 100개의 테이블 중 1개의 테이블에만 손님이 있다면 1명의 알바생만 일하고 나머지 99명의 알바생들은 아무것도 하지 않고 놀고 있는 것이다. 이러한 예시처럼 멀티 스레드, 블로킹 모델의 단점은 적은 양의 작업을 수행할 때 불필요한 자원을 낭비할 수 있다는 점이 있다.

마지막 예시를 통해

_**그렇다면 여려 명의 점원이 논 블로킹으로 일하면 효율적이지 않을까?**_

하는 의문이 생길 수 있다. 하지만 우리가 멀티 스레드 방식으로 프로그래밍을 하는 것은 상당히 어렵다고 한다.

이러한 이유 때문에 Node.js 는 I/O 요청에 대해서 멀티 스레트 방식이 아닌 **멀티 프로세스 방식**을 대신 사용한다.

**즉, 이벤트 루프가 싱글 스레드에서 동작한다는 의미이며 내부적으로는** **스레드 풀을 두어 I/O 작업에 스레드를 사용할 수 있도록한다.(병렬적 작업 진행)**

---

# 프로세스, 스레드에 대한 간략한 공부를 마치며

확실히 비전공자라서 그런지 프로세스가 뭔지 스레드가 뭔지 대충 감은 오는데 내가 설명하기 어려운 부분이 많았다.

특히나 앞 전에 공부한 비동기 논 블로킹 I/O 모델을 공부할 때 많이 느꼈다.

하지만 해당 부분을 공부하니 서툴기는 하더라고 내가 직접 설명할 수 있을 것만 같다.

그래도 아직 명확하지 않은 부분이 많다. 더 많이 검색하고 공부해서 내 것으로 만들어야겠다..!

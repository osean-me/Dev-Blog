---
title: 'Spring Framework'
date: '2020-08-02'
tags: ['Spring', 'Spring MVC', 'Spring Framework']
draft: false
summary: 'Spring Framework 의 철학과 특징에 대해 학습한 내용을 정리 했습니다.'
---

![Spring Framework](https://spring.io/img/spring-2.svg)

# 들어가기에 앞서

> 학원에서 Spring Framework / MyBatis / Maven 을 배운지 약 일주일 정도 지났다.
>
> 저번 주에는 요구사항 분석 / UML 등에 대해 시험을 치뤘고, 돌아오는 주에는 이번주에 배웠던 내용들로 시험을 볼 예정이다.
>
> 수업 시간에 배웠던 내용, 책에서 나왔던 내용, 여러 블로그에서 말하는 내용들을 Notion 에 개인적으로 정리해두었지만,
>
> 복습 겸 블로그에도 작성하면 좋을 것 같아 몇 자 적어보려고 한다.
>
> _(많이 부족한 내용이며 지극히 개인적인 견해를 담은 내용이니 읽으시는 분의 필터링이 필요할 것 같다. 왜냐면 너무 기초에 대한 내용이기도 하면서 공부한 내용을 내가 이해한 방식으로 작성했기 때문에 정확하지 않을 수 있기 때문이다. 그래서 선배님들의 태클은 정말정말 환영합니다..!😭)_

---

# Spring Framework ?

## Framework ?

> 소프트웨어 프레임워크는 복잡한 문제를 해결하거나 서술하는데 사용되는 기본 개념 구조이다. 소프트웨어 개발에 있어 하나의 뼈대 역할을 한다.

## Spring Framework ?

> 자바 플랫폼을 위한 오픈소스 애플리케이션 프레임워크이다. 경량 컨테이너로써 자바 객체를 직접 관리(IoC) 한다. 추상화된 트랜잭션을 관리할 수 있도록 지원하며, 설정 파일(xml, java, property) 을 이용한 선언적인 방식으로 운영한다.
>
> Aop (Ascept Oriented Programming) | 관계 지향 프로그래밍
> Ioc (Inversion of Control) | 제어의 역전
> DI (Dependency Injection) | 의존성 주입
> MVC (Java Design Pattern > Model + View + Controller)

### Ascept Oriented Programming (AOP)

- 관계 지향 프로그래밍
- 객체 지향 관점의 상위 개념
- 트랜잭션 / 로깅 등 다양한 기능들로 이루어진 모듈들에서 공통적으로 들어가는 기능을 분리
- 위의 분리를 위해 각 객체의 관점에서 바라보며, 종단면으로 사고하는 프로그래밍 방식

### Inversion of Cotrol (IoC)

- 제어의 역전
- 컨트롤의 제어권이 사용자가 아닌 Spring Framework 에게 넘어간 것을 의미
- 객체를 새로 생성하거나 특정 객체를 선택하지 않는다.
- 사용자가 제어해야 할 사항들을 문서(명세)화 하여 Spring Framework 에 등록 > Spring 이 자동으로 해당 제어 사항들을 관리한다.
- 인스턴스 생성 / 삭제 등을 Controller 가 수행한다.
- Object 는 자신이 어떻게 생성되고 어떻게 사용되는지 알 수 없다. > 추상적인 것에 의존
- 계층 간의 의존 관계의 결합도를 낮추기 위해 적용

### Dependency Injection (DI)

- 각 계층 혹은 서비스 간 의존성을 주입하는 것을 뜻한다.
- Maven 의 pom.xml 파일 내에 의존성을 추가하고 context-servlet.xml (sts) / dispatcher-servlet.xml (IntelliJ) 파일 내에 추가한 의존성을 사용하기 위해 Spring 에 bean 으로 등록을 하는 과정.
- Annotation 을 이용하여 의존성 주입 (@Autowired / @Controller / @Service / @Repository)
  - ex) Service 의 처리를 위해서 DAO 에 의존 -> has a 관계

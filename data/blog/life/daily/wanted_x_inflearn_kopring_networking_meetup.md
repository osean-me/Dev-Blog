---
title: 'Kotlin + Spring Networking Meetup'
date: '2023-02-18'
tags: ['원티드', '인프런', 'Kotlin', 'Spring', '네트워킹 밋업', '개발자 커뮤니티', '커뮤니케이션']
draft: false
summary: '[원티드 X 인프런] Kotlin + Spring Networking Meetup 참여 후기를 정리 했습니다.'
---

![메인](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FJNJSr%2FbtrZUBGgOkf%2F2Bs7EPY0gDki3dICjoV2a1%2Fimg.jpg)

## 지금 우리 회사는

현재 회사에서 운영 중인 어드민은 빠르게 개발하기 위해 [Trimou](http://trimou.org/) 를 채택해 개발했는데, 해당 템플릿이 Spring Boot 2.X 를 지원하지 않는 이슈가 있어 어쩔 수 없이(?) Java 8 + Spring Boot 1.5.13 을 기반으로 모든 서비스가 운영되고 있다. 또한 모든 도메인이 하나의 프로젝트에서 멀티 모듈로 관리되고 있어 쉽게 버전을 올릴 수 없는 상황이었다.

이러한 내부적인 이슈로 인해 Spring Boot 2 부터 지원하는 Optional 을 사용하지 못해 매번 별도의 Nullable Check 를 해줘야 하는 불편함을 항상 느꼈다. 그리고 여러 커뮤니티에서 Kotlin 에 대한 주제가 자주 언급되다 보니 자연스럽게 관심을 가지게 되었고, 토이 프로젝트로 가지고 놀다 보니 Java 에서는 느낄 수 없었던 편리함이 크게 다가왔다.

- 확장 함수를 적극 활용해 보일러 플레이트 코드를 줄일 수 있었다.
- Java 에서 지원하지 않는 여려 기본 함수를 활용해 보일러 플레이트 코드를 줄일 수 있었다.
- 모든 데이터는 기본적으로 Final 하기 때문에 의도치 않게 발생하는 NPE 를 줄일 수 있었다.
- when 문법을 활용해 불필요하게 중첩된 if 문 스코프를 줄여 가독성을 높일 수 있었다.
- 생성자 아규먼트에 기본 값을 할당해 초기화하거나, Builder 패턴과 유사하게 코드를 작성해 Lombok 의 @Builder 보더 더 유연한 코드를 작성할 수 있었다.

![당첨문자](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FIzeOO%2FbtrZSTfYiio%2Fnxdm0GkYkTF9eGfOFhpQF0%2Fimg.png)

위와 같이 내가 경험한 것들을 우리 팀원들도 함께 느끼고 성장했으면 하는 바람이 생겨 사내 차기 프로젝트에 Kotlin + Spring Boot 를 도입을 제안했고, 팀원들은 흔쾌히 동의해주었다. 그래서 올해 초부터 Kotlin + Spring Boot 3.0.2 기반으로 프로젝트를 진행 중이다.

하지만 팀원들 모두가 Kotlin 이 익숙하지 않은 상태라 어느 문제를 마주했을 때 이전보다 빠르게 해결하지 못하는 상태였다. 그렇기에 이미 Kotlin + Spring 을 도입한 회사에서는 어떻게 서버를 운영하고 개발하는지, 트러블 슈팅은 어떻게 진행하는지 등 선배들의 도움이 간절히 필요했다.

그런 와중에 하늘이 나를 돕는지 때마침 원티드 X 인프런에서 공동으로 주최하는 네트워킹 밋업 행사가 열린다는 소식을 듣고 밑져야 본전이라는 생각으로 빠르게 접수했는데, 이게 웬걸. 덜컥 당첨이 되어 버렸다.

---

## 첫 네트워킹 밋업

개발자로 커리어를 시작하면서 처음 가지는 외부 행사라 설레기도 하면서 한편으로는 긴장되기도 했다. 왜냐하면 안내 메일에서 네트워킹 시간이 있을 예정이라 명함을 두둑이 챙겨 오라는 내용이 있었기 때문이다.

![마루180](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FKAzeU%2FbtrZJWZCFX6%2FTith7K5kohHKvLtbhDOAnK%2Fimg.png)

![행사 팜플렛](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F2O24P%2FbtrZ0KJxKuB%2FOdYuHeHDU5247qMg7Sc3Nk%2Fimg.png)

아무리 E 와 I 사이 그 어딘가에 위치한 MBTI 라고는 해도 천성은 어쩔 수 없는 걸까? 행사장에 도착하기 전까지 마음속으로 개발자 분들이나 행사 관계자 분을 만나면 여러 케이스의 인사 시뮬레이션을 돌렸다. 그만큼 첫 행사 참여라 긴장됐다.

그렇게 행사 당일, 생각보다 행사장에 일찍 도착했다. 이름표와 각종 기념품들을 수령한 뒤 어디에 앉으면 좋을지 물색했다. 학부생 시절 항상 뒤에만 앉았던 기억이 후회로 남았기에 이번에는 큰 용기를 가지고 앞자리에 앉아서 참여해 보기로 했다.

![행사장 내부](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcdXPzB%2FbtrZYTfA7Hd%2Fn0t0qVLhSf7uWoLsSgEM2k%2Fimg.png)

![기념품](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FmMID9%2FbtrZ0KCMHNI%2F0Z5Tt4pN7Nf00c9UFy8P0K%2Fimg.png)

자리에 앉아 챙겨 받은 기념품을 살펴봤는데 모두 취향 저격하는 아이템들로 채워져 있어 기분이 좋았다. 특히 인프런 쿠폰..!  
인프런에서 가격 때문에 항상 눈물을 머금고 다음을 기약해야 하는 강의들이 많았는데 그 친구들을 다시 데려올 수 있게 됐다. (감사합니다!)

---

## 행사 시작을 알리는 아이스 브레이킹

![진행1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FN0AeO%2FbtrZLT8VhiQ%2FGZZ3n6dgKhJiIYw8Bx0VO1%2Fimg.png)

![진행2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbh3H85%2FbtrZKouSJ3x%2FckRwVWQpYxEehtKkYwkws1%2Fimg.png)

사실 개발자는 I 가 대부분일거라고 나는 믿어 의심치 않는다. 그렇기에 서로 어색한 분위기를 어떻게 헤쳐 나갈까 걱정이 이만저만이 아니었다. 그런 걱정을 안고 박수 소리와 함께 행사를 시작 했는데, 걱정과는 달리 아이스 브레이킹 시간이 정말 재밌었고, 생각보다 다들 적극적이셨다! 그 덕분인지 붙들고 있던 긴장도 많이 내려 놓을 수 있었고 조금 더 편안한 마음으로 행사에 참여할 수 있었다.

---

## 인상 깊었던 발표

![발표1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb5RtHV%2FbtrZNvNHHe9%2FqCqCLBHmZMGSniQWEJGJy0%2Fimg.png)

![발표2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FDbVFY%2FbtrZKoVXizw%2FfrvTkY8TtI5wCwtNQ3AV21%2Fimg.png)

먼저 [두들린](https://www.doodlin.co.kr/) CTO 이신 서동민님의 \[살아남아야 하니까, 코틀린\] 세션으로 본 행사의 막을 열었다.  
두들린이 어떤 서비스를 운영하고 있는지, 어떤 방식으로 일하는지 간단하게 설명해주셨고, 이후로 피봇팅 전 두들린이 당면한 문제가 무엇이고 이를 해결하기 위해 어떠한 방식으로 돌파구를 찾아 나섰는지 두들린의 고군분투 모험담을 들려주셨다.

피봇팅을 위해 빠르게 개발해야 하는 상황이나 다양한 레퍼런스와 커뮤니티로 쉽고 정확하게 트러블 슈팅이 가능해야 하는 상황 등 여러 점에서 공감이 많이 되었다. 아직 2년 조금 넘은 주니어 개발자이지만 CTO 없이 팀을 꾸려가고 있다 보니 더욱 맘에 와닿았다.

그래서 해당 세션에서 두들린 팀이 Kotlin + Spring 으로 넘어가면서 느꼈던 장점들은 다음과 같다고 말씀해 주셨다.

- **Data Class 활용**
- **Extension Functions 을 통한 공통 정책 관리**
- **Kotlin Style 코드 작성**
- **Code Quality 향상**
- **유지보수성 증가**
- **생산성 향상**

혼자 토이 프로젝트를 하며 느낀 점과 대부분 비슷했고, Python 은 잘 모르지만 코드를 비교하며 어떤 점들이 만족스러웠는지 발표해 주시는 모습을 보며 우리 팀에 Kotlin + Spring 도입하길 참 잘했다고 생각이 들었고, 세션을 들은 후 Kotlin 도입을 긍정적으로 받아들인 팀원들이 고맙기도 하면서 우리 팀이 더욱더 빠르게 성장할 수 있겠다는 믿음이 생겼다. 이번 프로젝트를 잘 마무리해내야지. 암!

---

## 네트워킹

Kotlin 선배와 후배로 팀을 나누어 섞어 여러 주제를 가지고 이야기를 나누는 시간을 가졌다.  
운이 좋았는지 처음에 앉은자리에서 뵌 선배님들이 대화를 잘 이끌어 주셨고, 주니어 개발자가 경험하기 힘든 이야기들도 해주셔서 많은 인사이트를 얻을 수 있었다. 그리고 나도 베풀 줄 아는 개발자가 되어야겠다고 생각했다.

네트워킹에서 가장 좋았던 점은, 서로 다른 환경에서 Kotlin + Spring 을 도입하며 겪은 이야기들을 들을 수 있었다는 점이다.  
한 우물에 머물러 있는 나 같은 우물 안 개구리에게 더 넓은 세상이 있다는 사실을 깨닫게 해 준 것 같았다. 지금까지 회사에서는 사수 없이 대부분 혼자서 모든 문제를 해결해야 하는 점들이 많은 어려움으로 다가왔기에 다른 개발자 선배님들은 어떻게 문제를 바라보고 해결했을까 궁금했는데 그런 간지러운 부분들을 속시원히 긁을 수 있었던 시간이라 참 좋았다.

그리고 같은 관심사를 가진 사람들과 함께 인사 나누는 시간도 좋았다. 여러 회사에서 근무 중이신 개발자분들을 만나면서 요즘 어떤 것들이 고민인지, 그 고민을 어떻게 헤쳐나가고 있는지 이야기를 들으면서 많은 자극이 되었고 나 또한 꾸준히 성장하는 개발자가 되어야지 생각할 수 있었던 대화였다.

---

## 마치며

첫 행사라 긴장을 많이 했지만, 그럼에도 잘 즐기고 왔고 생각보다 주도적으로 대화를 이끈 나 자신이 조금 기특하다고 느낀다.  
처음보는 사람들과 어렵지 않게 대화할 수 있었다는 점과 꽤나 E 성향이 강한 내 모습을 발견하게 된 점이 뜻밖의 수확이었다.

그리고 언제 다시 이런 행사에 참여할 수 있을지는 모르겠지만 꼭 이런 공식적인 행사가 아니더라도 개발자 모임이 있으면 자주 나가서 커뮤니케이션 하는 것이 질 좋은 성장을 꾀할 수 있겠구나 깨닫게 된 시간이었다. 이번 경험을 발판 삼아 앞으로 더 성장하는 내가 되어야지!

끝으로 대화 나눈 여러 개발자분들과 행사 담당자분들, 행사를 주최한 원티드와 인프런에게 심심한 감사를 전한다!

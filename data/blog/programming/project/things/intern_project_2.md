---
title: 'Things Intern Project - 2'
date: '2020-12-23'
tags: ['먼슬리씽', '씽즈', '토이 프로젝트', '게시판 구현']
draft: false
summary: '인턴 입사 후 게시판 만들기 과제를 진행하면서 경험한 내용을 정리 했습니다.'
---

# _**전체/단일 조회, 비밀번호 확인, 수정**_

1.  **게시글 페이지네이션 기능 구현하기**

    - 과거 팀 프로젝트를 진행할 때, 직접 페이지네이션 연산 코드를 Service.class 에 구현해서 사용했다.
    - JPA를 사용하면서 **페이지네이션을 기본 제공**하는 것을 알게 되었다.
    - 하지만 아직 구현 못한 기능들이 많아서 일단 전체 게시글을 조회하는 방식으로 구현했다.

    ***

2.  **LocalDateTime.class 포맷 수정하기**

    - 게시글 작성 시 BaseTimeEntity.class에서 자동으로 입력된 날짜의 포맷 형식은 아래의 사진처럼 데이터베이스에 저장된다.
    - 게시글 리스트나 게시글 상세 페이지에 해당 포맷을 그대로 출력하기에는 무리가 있어 `PostResponseDto`의 date 인스턴스 변수를 String으로 선언하고, `DateTimeFormatter.ofPattern()`을 이용해 년월일만 출력할 수 있도록 구현했다.

    ***

3.  **게시글 수정을 위한 비밀번호 확인 기능 구현하기**

    - 해당 기능을 구현하기 위해서 단순 게시글의 비밀번호를 조회하고 비교해야 하는데 `@PostMapping` 어노테이션을 사용해야 하는지 많은 고민의 시간을 가졌다.
    - 먼저 Ajax 통신을 이용하고 Http의 Method를 `GET`으로 설계해서 구현 했을 때, 전송하고자 하는 데이터를 `JSON` 데이터 형식으로 전송하지 못했다.
      - 그 이유는 `GET`에는 body영역이 존재하지 않기 때문에, `JSON`으로 넘겨도 `Query String`으로 받기는 하지만 이를 해석하지 않는다. 그래서 `JSON` 데이터를 넘기기 위해서는 `PUSH`, `PUT` Method를 사용해야 한다.
    - 위와 같은 이유로 POST로 사용해서 비밀번호를 확인할 수 있게 했고, `EntityManager.find(Post.class, id);` 를 통해 반환된 `Post` 엔티티가 `Null` 인지를 확인하고, 이에 대한 논리값을 프론트엔드 단으로 넘길 수 있게 했다.
    - 입력한 비밀번호와 게시글 비밀번호와 같다면 게시글 수정 페이지로 이동할 수 있다. 하지만 아직까지는 필터링 작업을 거치지 않았기 때문에 추후에 필터링 작업을 구현해야 한다.

    ***

    1.  **게시글 수정 기능 구현하기 → Dirty Checking(변경 감지)**

        - 게시글 수정 페이지 진입에 성공했다면, 게시글 전반적인 내용을 수정할 수 있다. 비밀번호도 함께 변경할 수 있다.
        - 해당 기능을 구현하면서 과거 **MyBatis Framework를 이용했던 경험** 때문에 내가 **직접 쿼리문을 짜지 않는 것에 대해 익숙치 않았다.**
        - 이번 프로젝트는 최대한 **Spring Data JPA를 사용하지 않으려고** 했기에, JPA에서 직접 쿼리를 사용하고 파라미터로 받은 데이터를 쿼리문에 삽입 할 수 있는 동적 쿼리 메소드로 설계하기 위해서 많이 알아봤지만 아직 개념이 잡히지 않아서 어려웠다.
        - 그래서 방향을 틀었다. 일단 수정 페이지에 진입 했다는 의미는 해당 게시글의 Id 값을 알고 있다는 의미이기 때문에, 해당 데이터를 이용해 `EntityManager.find(Post.class, id);` 를 이용해 객체를 불러오고 해당 객체를 Setter 메소드를 이용해서 JPA에서 알아서 Update 쿼리를 날릴 수 있도록 구현했다.
        - 하지만 문제가 발생했다. `EntityManager.class`가 제대로 작동하지 않았다. 해당 객체가 트랜잭션을 생성할 수 없는 문제였다. `SpringConfig.class` 에서 `PostRepository`에 `EntityManager.class`를 주입했지만 해당 방법으로는 `EntityTransaction.class` 객체를 만들 수 없는 것 같았다.
        - 해당 문제를 해결하기 위해 `PostRepository.class`에 등록한 `EntitManager.class`를 직접적으로 사용하지 않고 `EntityManagerFactory.class` 객체를 먼저 생성하고, 해당 객체로 생성된 `EntityManager`를 통해서 `EntityTransaction.class`를 생성했더니 성공적으로 게시글을 수정할 수 있었다.
        - 해당 부분은 동영상 강의를 보면서 조금 더 공부해야겠다.

        ***

        # **Controller**

    2.  비밀번호 확인 → `POST` / 게시글 수정 → `PUT`

        ```java
        @PostMapping("/api/check")
        public boolean check(@RequestBody PostRequestDto requestDto) {
            boolean check = postService.checkPw(requestDto);
            return check;
        }

        @PutMapping("/api/update")
        public ResponseEntity update(@RequestBody PostRequestDto requestDto) {
            postService.update(requestDto);
            return ResponseEntity.ok().build();
        }
        ```

        # **Service**

    3.  기존 `PostRepository.class` 의 `findById()`에서 조회된 `객체`가 `Null`인지에 따라 프론트엔드 단으로 논리값을 전달한다.

        ```java
        @Override
        public boolean checkPw(PostRequestDto requestDto) {
            Post post = postRepository.findById(requestDto.getId());
            boolean check = post.getPassword().equals(requestDto.getPassword()) ? true : false;
            return check;
        }
        ```

    4.  프론트엔드 단에서 전달받은 `PostRequestDto`를 `Post.class`로 `Build()`한 후, `PostRepository`에 전달한다.

        ```java
        @Override
        public void update(PostRequestDto requestDto) {
            Post post = Post.builder()
                    .id(requestDto.getId())
                    .title(requestDto.getTitle())
                    .content(requestDto.getContent())
                    .author(requestDto.getAuthor())
                    .password(requestDto.getPassword())
                    .build();
            postRepository.update(post);
        }
        ```

        # **Repository**

    5.  `EntityManagerFactory.class` 를 `@Autowired` 어노테이션을 통해 스프링 컨테이너에 빈으로 자동으로 등록한 후, 트랜잭션을 위한 `EntityManager.class` 객체를 새로 얻는다.
        ```java
        @Override
        public void update(Post post) {
            EntityManager em = emf.createEntityManager();
            EntityTransaction tx = em.getTransaction();
            tx.begin();
            Post changePost = em.find(Post.class, post.getId());
            changePost.setTitle(post.getTitle());
            changePost.setContent(post.getContent());
            changePost.setAuthor(post.getAuthor());
            changePost.setPassword(post.getPassword());
            tx.commit();
        }
        ```

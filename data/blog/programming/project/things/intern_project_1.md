---
title: 'Things Intern Project - 1'
date: '2020-12-22'
tags: ['먼슬리씽', '씽즈', '토이 프로젝트', '게시판 구현']
draft: false
summary: '인턴 입사 후 게시판 만들기 과제를 진행하면서 경험한 내용을 정리 했습니다.'
---

# _**게시글 작성하기**_

1.  **Domain**과 **Dto**의 차이에 대해 이해하기

    - **domain**의 경우, **Entity**의 역할을 한다. JPA를 통해 자동 스키마도 생성 가능하다.
    - **dto**는 클라이언트의 요청한 데이터를 매핑해 하나의 객체로 만들어주는 역할을 한다.

    ***

2.  **Domain**에 `@Entity` 와 `@Table` 를 붙여 **JPA**가 자동으로 스키마를 생성할 수 있도록 한다.

    - 각 필드 변수에 `@Id`, `@Column` 을 붙여 테이블의 구성요소를 선언한다.

    ***

3.  **Repository**, **Service**를 **Component Scan(의존성 자동 등록)**이 아닌 `@Configuration` 과 `@Bean` 을 이용해 직접 클래스들을 스프링 컨테이너에 Bean으로 등록한다.

    - 추후 회원 기능도 도입되었을 때, 편리하게 작업하기 위한 모듈화 작업이다.

    ***

4.  게시글은 `@ResponseBody` 를 이용해 REST API로 구현했다.  
    `@RequestParam`을 이용해서도 구현 가능하지만 트렌드를 따르기 위해서 위와 같이 구현했다. **~(사실은 테스트 코드 짜는 예시를 못 찾겠다.)~**

    - REST API로 구현하기 위해서는 `<form>` 태그의 Content-type을 `application/json` 으로 바꿔주어야 하는데, 해당 작업은 Javascript에서 가능하다.
    - 테스트를 진행하면서 http 프로토콜의 구성요소 중 Content-Type에 대한 에러를 파악하기 위한 시간이 가장 많이 소요됐따.

    ***

5.  **TDD** 우선적으로 해보기

    - 안 좋은 습관 중 하나인 애플리케이션을 빌드하고 직업 브라우저에서 테스트해보는 습관이 있다.
      1.  테스트 코드를 작성하기가 귀찮고 어려워서 포기한다.
      2.  브라우저에서 확인하면서 하는 게 가독성이 좋다고 느낀다.하지만 이번 기회로 TDD를 우선적으로 시행했을 때, 에러를 확인할 수 있는 시간이 훨씬 단축되었다.
      3.  아직까지 테스트 코드에 대한 이해가 많이 부족하지만 노력해야겠다.
      4.  이렇게 두 가지가 가장 큰 이유다.

6.  **JPA Auditing**을 이용해 작성 날짜, 수정 날짜 → **Entity**에 주입하기
    - 과거에는 MyBatis를 이용해 직접 쿼리문을 작성했기 때문에 크게 문제가 없었다.  
      하지만 JPA를 공부하면서 기본적인 CRUD는 기본적으로 JPA에서 제공하기 때문에 게시글 작성이나 회원 가입 시, 추가되는 데이터의 날짜를 어떻게 추가해야 할지 고민이었다.
    - 이러한 고민을 해결하기 위해 구글링 한 결과, JPA Auditing이라는 것을 알게 되었다. **해당 기술은 Spring Data JPA에서 시간에 대해 자동으로 값을 넣어주는 기능이다.**
7.  **Reference**

#### **\- Controller에 파라미터 데이터가 제대로 들어가지 않아 Entity에 매핑하지 못할 때 발생하는 에러**

[Spring Data JPA](https://sas-study.tistory.com/348)

#### \- **REST API TDD를 위한 게시글**

[참고 아티클](https://galid1.tistory.com/545)

#### \- **날짜 컬럼의 입력 데이터를 자동으로 갱신하고 채워 넣어주는 방법에 대한 게시글**

[참고 아티클](http://yoonbumtae.com/?p=2973)

[JPA Auditing](https://webcoding-start.tistory.com/53)

---

# **Controller**

## PostController.class

- `Getmapping` 된 메소드의 집합  
  데이터를 클라이언트 단으로 전송하거나 View Resolver를 통해 특정 Template를 반환한다.

  ```java
  package com.things.project01.controller;

  import org.springframework.stereotype.Controller;
  import org.springframework.web.bind.annotation.GetMapping;
  @Controller
  public class PostController {
      @GetMapping("/")
      public String index() {
          return "index";
      }

      @GetMapping("/created")
      public String created() {
          return "created";
      }
  }
  ```

## PostApiController.class

- `Post`, `Put`, `Delete` 등 REST API의 구성요소가 담겨 있다.

  ```java
  package com.things.project01.controller;

  import com.things.project01.dto.PostRequestDto;
  import com.things.project01.service.PostService;
  import org.springframework.http.ResponseEntity;
  import org.springframework.web.bind.annotation.*;

  @RestController
  public class PostApiController {
      private PostService postService;

      public PostApiController(PostService postService) {
          this.postService = postService;
      }

      @PostMapping("/api/created")
      public ResponseEntity created(@RequestBody PostRequestDto createdDto) {
          postService.created(createdDto);

          return ResponseEntity.ok().build();
      }
  }
  ```

# **Service**

- `PostService` / `PostServiceImpl`  
  → 추후에 회원 기능을 도입했을 때는 고려해서 설계했다.

### PostService.class

```java
package com.things.project01.service;

import com.things.project01.domain.Post;
import com.things.project01.dto.PostRequestDto;

public interface PostService {
    public Post created(PostRequestDto createdDto);
}
```

### PostServiceImpl.class

```java
package com.things.project01.service;

import com.things.project01.domain.Post;
import com.things.project01.dto.PostRequestDto;
import com.things.project01.repository.PostRepository;

import javax.transaction.Transactional;

@Transactional
public class PostServiceImpl implements PostService {

    private PostRepository postRepository;

    public PostServiceImpl(PostRepository postRepository) {
        this.postRepository = postRepository;
    }

    @Override
    public Post created(PostRequestDto createdDto) {
        postRepository.created(createdDto.toEntity());
        return createdDto.toEntity();
    }
}
```

# **Repository**

- `PostRepository` / `PostRepositoryImpl`  
  → 추후에 회원 기능을 도입 했을 때를 고려해서 설계했다.

### PostRepository.class

```java
package com.things.project01.repository;

import com.things.project01.domain.Post;

import java.util.List;

public interface PostRepository {
    // 게시글 작성
    public void created(Post post);

    // 게시글 조회
    public List<Post> findAll(int start, int finish);

    // 게시글 수정
    public Post modified(Post post);

    // 게시글 삭제
    public Post delete(Post post);
}
```

### PostRepositoryImpl.class

```java
package com.things.project01.repository;

import com.things.project01.domain.Post;

import javax.persistence.EntityManager;
import java.util.List;

public class PostRepositoryImpl implements PostRepository {

    private EntityManager em;

    public PostRepositoryImpl(EntityManager em) {
        this.em = em;
    }

    @Override
    public void created(Post post) {
        em.persist(post);
    }

    @Override
    public List<Post> findAll(int start, int finish) {
        return em.createQuery("select p from Post p", Post.class)
                .getResultList();
    }

    @Override
    public Post modified(Post post) {
        return null;
    }

    @Override
    public Post delete(Post post) {
        return null;
    }
}
```

# **Domain**

### Post.class

- `Post` 엔티티의 역할을 가지며, JPA의 자동 스키마 생성 기능을 통해 테이블을 생성한다.

  ```java
  package com.things.project01.domain;
  import lombok.*;
  import javax.persistence.*;
  @Entity
  @Table
  @Getter
  @Setter
  @Builder
  @AllArgsConstructor
  @NoArgsConstructor
  public class Post extends BaseTimeEntity{

      @Id
      @GeneratedValue(strategy = GenerationType.IDENTITY)
      private Long id;

      @Column(length = 255, nullable = false)
      private String title;

      @Column(length = 255, nullable = false)
      private String content;

      @Column(length = 30, nullable = false)
      private String author;

      @Column(length = 4, nullable = false)
      private String password;
  }
  ```

## BaseTimeEntity.class

- `BaseTimeEntity` 대부분의 엔티티에 필요한 날짜 컬럼을 자동으로 삽입해주는 추상 클래스이다. → `AuditingEntityListener` 클래스 사용

  ```java
  package com.things.project01.domain;

  import lombok.Getter;
  import org.springframework.data.annotation.CreatedDate;
  import org.springframework.data.annotation.LastModifiedDate;
  import org.springframework.data.jpa.domain.support.AuditingEntityListener;
  import javax.persistence.EntityListeners;
  import javax.persistence.MappedSuperclass;
  import java.time.LocalDateTime;

  @Getter
  @MappedSuperclass
  @EntityListeners(AuditingEntityListener.class)
  public abstract class BaseTimeEntity {
      @CreatedDate private LocalDateTime createdDate;
      @LastModifiedDate private LocalDateTime modifiedDate;
  }
  ```

# **configuration**

## JpaConfig.class

- `JpaConfig` → BaseTimeEntity를 스프링 컨테이너에 등록하기 위한 JPA 설정 파일이다.

  ```java
  package com.things.project01;

  import org.springframework.context.annotation.Configuration;
  import org.springframework.data.jpa.repository.config.EnableJpaAuditing;

  @Configuration
  @EnableJpaAuditing public class JpaConfig { }
  ```

## SpringConfig.class

- `SpringConfig` → Service, Repository를 Component Scan이 아닌 Configuration, Bean으로 직접 작성하여 스프링 컨테이너에 등록한다.

  ```java
  package com.things.project01;

  import com.things.project01.repository.PostRepository;
  import com.things.project01.repository.PostRepositoryImpl;
  import com.things.project01.service.PostService;
  import com.things.project01.service.PostServiceImpl;
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.context.annotation.Bean;
  import org.springframework.context.annotation.Configuration;
  import javax.persistence.EntityManager;

  @Configuration
  public class SpringConfig {

      private EntityManager em;

      @Autowired
      public SpringConfig(EntityManager em) {
          this.em = em;
      }

      @Bean
      public PostRepository postRepository() {
          return new PostRepositoryImpl(em);
      }

      @Bean public PostService postService() {
          return new PostServiceImpl(postRepository());
      }
  }
  ```

# **Test**

## PostServiceImplTest.class

- `PostServiceImplTest` → REST API, 게시글 작성 테스트를 위한 테스트 코드이다.

  ```java
  package com.things.project01.service;

  import com.things.project01.repository.PostRepository;
  import io.restassured.RestAssured;
  import org.junit.jupiter.api.BeforeEach;
  import org.junit.jupiter.api.DisplayName;
  import org.junit.jupiter.api.Test;
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.boot.test.context.SpringBootTest;
  import org.springframework.boot.web.server.LocalServerPort;
  import org.springframework.http.HttpStatus;
  import org.springframework.http.MediaType;
  import javax.transaction.Transactional;

  @SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
  @Transactional
  class PostServiceImplTest {
      @LocalServerPort
      int port;

      @BeforeEach void setup() {
          RestAssured.port = port;
      }

      @Autowired
      private PostRepository postRepository;

      @Test
      @DisplayName("게시글 등록") void created() {
          // given
          String requestParams = "{\n" + "\"title\": \"title\",\n" + "\"content\": \"content\",\n" + "\"author\": \"author\",\n" + "\"password\": \"1234\"\n" + "}";

          // when / then
          RestAssured.given()
                      .log()
                      .all()
                      .contentType(MediaType.APPLICATION_JSON_VALUE)
                      .accept(MediaType.APPLICATION_JSON_VALUE)
                      .body(requestParams)
                      .when()
                      .post("/api/created")
                      .then()
                      .log()
                      .all()
                      .statusCode(HttpStatus.OK.value());
      }
  }
  ```

---
title: 'Things Intern Project - 3'
date: '2020-12-24'
tags: ['먼슬리씽', '씽즈', '토이 프로젝트', '게시판 구현']
draft: false
summary: '인턴 입사 후 게시판 만들기 과제를 진행하면서 경험한 내용을 정리 했습니다.'
---

# _**게시글 CRUD, 페이지네이션**_

[DTO 참고 아티클](https://woowacourse.github.io/javable/post/2020-08-31-dto-vs-entity/)

[Setter 참고 아티클](https://velog.io/@aidenshin/%EB%82%B4%EA%B0%80-%EC%83%9D%EA%B0%81%ED%95%98%EB%8A%94-JPA-%EC%97%94%ED%8B%B0%ED%8B%B0-%EC%9E%91%EC%84%B1-%EC%9B%90%EC%B9%99)

1.  **Entity, Dto 기본 개념 이해하고, 코드 수정하기**

    - 기초적인 것들을 구현하면서 JPA 설계에 대해 많은 의문이 들었다.  
      게시글 수정 기능을 구현하면서 기존의 데이터를 수정할 때, **JPA**의 **Dirty Check**를 통해 객체의 값이 변경되어야 한다. 어떻게 구현할까 하다가 간단히 Setter 메소드를 이용해서 구현하기는 했다. 하지만 아무리봐도 이 방법은 좋지 못한 방법 같았고, 이를 해결하기 위해서 `Entity`와 `Dto` 그리고 `Entity` 객체 지향 설계 방법에 대해 검새해봤다.
    - **객체 지향 설계는 어떻게 하는 것이며, 왜 Entity 객체에서 Setter 메소드는 사용하지 않는지, Entity와 Dto 객체를 왜 나누는지 여러 부분에 있어 의문이 들었다.**
      1.  Dto → View Layer와 데이터를 주고 받을 때 사용한다.
      2.  Entity → DB Layer와 데이터를 주고 받을 때 사용한다.
      3.  이러한 사용에서 Entity의 과도한 Setter 사용은 어디에서든 객체의 데이터가 변경될 수 있다. 그러한 이유로 Entity에서는 Setter 메소드를 사용하지 않는다.
    - 그렇다면 Service와 Repository 사이에서 **Dto ↔ Entity** 로 객체를 어떻게 교체할 수 있을까?  
      딱 떨어지는 정답은 아직 찾지 못했지만, 최대한 Setter를 사용하지 않기 위해 Dto에 Post를 인자로 받는 메소드나 생성자를 만들고 이를 통해 서로 교체할 수 있도록(?) 구현했다.  
      객체지향적인 설계인지는 잘 모르겠다.

    ***

2.  **게시글 페이지네이션**

    - Spring MVC로 개발할 때는 페이지네이션 로직을 직접 구현했고, 그 로직을 통해 연산된 값을 MyBatis에 매핑된 쿼리문에 넣어주는 형식으로 구현했다.  
      그래서 계산하는게 조금 복잡하긴 했지만 어찌저찌 페이지블록, prev, next를 구현하긴 했다.  
      하지만 JPA로 구현하려고 하니 쿼리문을 직접 짜는 것이 아니라 모든 것을 JPA에게 이관하니까 어떻게 컨트롤 해야 할지 감이 잡히지 않았다.
    - 일단은 나는 DB를 접근하는 방식을 기초 JPA로 구현하고자 했고, 회원 기능을 추가 했을 때를 고려하여 인터페이스로 추상화하여 설계하고 있었다.  
      그렇기 때문에 **Spring Data JPA**의 `Page.class`를 이용하기 위해서는 `PaginationRepository`를 만들어야 하는 상황이었다.
    - 하지만 여러 문제가 있었다.

      1.  어떻게 페이지블록을 알 수 있을까.
      2.  prev, next를 의미하는 페이지블록 값은 어떻게 구할 수 있을까.
      3.  `PostService`는 `SpringConfig`에서 직접 `PostRepository`를 주입 받고 있었다. 즉, `PostService`는 `PostRepostiroy`와 `PaginationRepository` 두 개의 레퍼지토리를 주입 받아야 한다.그렇다면 인터페이스인 `PaginationRepository`를 어떻게 주입할 수 있을까.

      **Post, Pageable**

    1.  **페이지블록**
        - `Page<Post>.getTotalElement()` 메소드를 사용하면 해당 객체에 실질적으로 들어있는 데이터의 개수를 파악할 수 있다.  
          나는 한 페이지에 10개의 게시글이 조회되도록 `Pageable`의 사이즈를 10으로 설정했는데, 해당 메소드를 통해 어떤 계산을 하면 무조건 10개의 블록이 아닌 게시글 개수에 따른 페이지블록 값을 얻을 수 있다.
    2.  `**PostServiceImpl`에 두 개의 `Repository` 주입하기\*\*
        - 먼저 코드의 가독성을 위해 Lombok의 `@RequiredArgsConstructor` 을 이용했는데, PaginationRepository의 경우 인터페이스 객체이기 때문에 new 생성자로 객체를 만들 수 없었다.
        - 그렇다고 `@RequiredArgsConstructor` 을 놓치고 싶지 않았다.
        - 그래서 `SpringConfig`에서 `PaginationRepository`를 `final` 제한자로 선언하고, SpringConfig 생성자에서 객체를 주입 받을 수 있도록 했다.

---

# **Controller**

## PostController.class

- View Layer로 가기 전, 페이지네이션 로직을 수행한다.

```java
@GetMapping("/post/{page}")
public String index(@PathVariable int page, Model model) {
    // 해당 페이지에 대한 게시글 받아오기
    Pageable pageable = PageRequest.of(page, 10);
    Page<Post> postList = postService.findAll(pageable);
    model.addAttribute("post", postList);

    // 페이지블록 계산하기
    long startBlock = (pageable.getPageNumber() - 1) / postList.getSize() * postList.getSize() + 1; // 해당 페이지를 기준으로 한 첫 페이지블록
    long blockCount = (postList.getTotalElements() + postList.getSize() - 1) / postList.getSize(); // 해당 페이지를 기준으로 한 페이지블록 개수
    long finishBlock = startBlock + postList.getTotalElements() - 1; // 해당 페이지를 기준으로 한 첫 페이지 블록
    finishBlock = blockCount < finishBlock ? blockCount : finishBlock; // 만약 페이지블록 개수보다 마지막 페이지블록의 수가 크다면

    List<Long> block = new ArrayList<>();
    for (long i = startBlock; i <= finishBlock; i++) block.add(i);
    model.addAttribute("block", block);
    return "list";
}
```

# **Service**

## PostServiceImpl.class

- PaginationRepository를 주입받고, 게시글을 불러온다.

```java
@Override
public Page<Post> findAll(Pageable pageable) {
    int page = (pageable.getPageNumber() == 0) ? 0 : (pageable.getPageNumber() - 1);
    pageable = PageRequest.of(page, pageable.getPageSize(), Sort.Direction.DESC, "id");
    return paginationRepository.findAll(pageable);
}
```

# **Repository**

## PaginationRespsitory.interface

- 페이지네이션을 위한 새로운 레퍼지토리

```java
package com.things.project01.repository;

import com.things.project01.domain.Post;
import org.springframework.data.jpa.repository.JpaRepository;

public interface PaginationRepository extends JpaRepository<Post, Long> {}
```

# **Entity**

## Post.class

- Setter 메소드를 제거하고, 이를 대신할 생성자와 메소드를 생성했다.

```java
package com.things.project01.domain;

import lombok.*;

import javax.persistence.*;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

@Entity
@Table
@Getter
@NoArgsConstructor
public class Post extends BaseTimeEntity{
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(length = 255, nullable =  false)
    private String title;

    @Column(length = 255, nullable = false)
    private String content;

    @Column(length = 30, nullable = false)
    private String author;

    @Column(length = 4, nullable = false)
    private String password;

    @Builder
    public Post(Long id, String title, String content, String author, String password) {
        this.id = id;
        this.title = title;
        this.content = content;
        this.author = author;
        this.password = password;
    }

    public void updatePost(String title, String content, String author, String password) {
        this.title = title;
        this.content = content;
        this.author = author;
        this.password = password;
    }
}
```

# **Configuration**

## SpringConfing.class

- `PaginationRepository`를 해결하기 위해 수정된 코드

```java
package com.things.project01.config;

import com.things.project01.repository.PaginationRepository;
import com.things.project01.repository.PostRepository;
import com.things.project01.repository.PostRepositoryImpl;
import com.things.project01.service.PostService;
import com.things.project01.service.PostServiceImpl;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;

@Configuration
public class SpringConfig {

    private EntityManager em;
    private EntityManagerFactory emf;
    private final PaginationRepository paginationRepository;

    @Autowired
    public SpringConfig(EntityManager em, EntityManagerFactory emf, PaginationRepository paginationRepository) {
        this.em = em;
        this.emf = emf;
        this.paginationRepository = paginationRepository;
    }

    @Bean
    public PostRepository postRepository() {
        return new PostRepositoryImpl(em, emf);
    }

    @Bean
    public PostService postService() {
        return new PostServiceImpl(postRepository(),paginationRepository);
    }
}
```

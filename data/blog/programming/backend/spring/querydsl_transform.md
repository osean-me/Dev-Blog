---
title: '[QueryDSL] NoSuchMethodError Trouble Shooting'
date: '2023-02-22'
tags: ['QueryDSL', 'NoSuchMethodError', 'Trouble Shooting']
draft: false
summary: 'QueryDSL 의 transform() 을 사용해 도메인 클래스로 맵핑하는 과정에서 마주한 문제를 어떻게 해결했는지 기록 했습니다.'
---

## 들어가기 앞서

현재 QueryDSL 5.0.0 버전을 기반으로 프로젝트 개발을 진행하고 있는데, 복잡한 연관관계를 가지는 테이블을 JPA Mapping 을 사용해 비즈니스 로직을 수행할 경우 너무 많은 쿼리를 요청하게 되어 불필요한 자원을 사용하는 문제가 있었다. 이를 해결하기 위해 @Query 애노테이션을 사용해 레퍼지토리에 조회 메소드를 구현하는 것을 고려 했지만 다음과 같은 이유로 반려했다.

- 여러 테이블을 한 번에 조회해 하나의 도메인 클래스로 재구성해야 하며, 여러 조건의 결과를 GroupBy 하여 임베디드 클래스의 필드에 할당해야 하는 상황에서 @Query 애노테이션은 구현이 어려울 것이라고 생각했다.
- 특정 엔티티의 레퍼지토리에 @Query 를 사용해 조회 메소드를 구현하는 것은 해당 레퍼지토리에 불필요한 책임을 부여한다고 생각했다.

이같은 이유로 QueryDSL 에서 제공하는 결과 집합 함수를 적극 활용하여 여러 테이블을 조회하고 각 조건에 맞는 결과를 하나의 도메인 클래스에 맵핑하는 코드를 작성했다.

### @QueryProjection

맵핑할 도메인 클래스의 생성자에 @QueryProjection 적용해 도메인 클래스의 QClass 생성자 생성할 수 있도록 한다.
Projections.bean() / Projections.fields() 와 같은 함수를 사용할 수 있었지만, 어떤 이유에서인지 맵핑이 되지 않았다. (`java.lang.IllegalArgumentException: argument type mismatch` 발생)

```kotlin
// 제품 도메인 클래스
data class Product @QueryProjection constructor(
  val categoryId: String,
  val categoryName: String,
  val productId: String,
  val name: String,
  val price: Int,
  val keywords: List<Keyword>
)

// 제품 키워드 도메인 클래스
data class Keyword @QueryProjection constructor(
  val keywordId: String,
  val keyword: String,
  val productId: String
)
```

### transform(...)

PersistenceAdapter 에서 구현하고자 하는 Port 의 함수에 QueryFactory 를 사용해 도메인 클래스로 바로 리턴해주도록 한다.  
[만들면서 배우는 클린 아키텍쳐](https://github.com/wikibook/clean-architecture) 에서는 DomainMapper 를 이용해 여러 테이블의 조회 결과를 도메인 클래스에 맵핑해주도록 하는데, 해당 과정에서 위에서 언급한 불필요한 조회 쿼리 요청 및 리소스 사용 문제가 발생하기 때문에 이번 비즈니스 로직에서는 제외하도록 한다.

```kotlin
// QClass static import
@PersistenceAdapter
class ProductPersistenceAdapter(
	private val queryFactory: JpaQueryFactory
) : GetProductPort {

	override fun getProducts(categoryId: String): List<Product> {
    	return queryFactory
          .from(eCategory)
          .join(eProduct).on(eProduct.categoryId.eq(eCategory.id))
          .leftJoin(eKeyword).on(eKeyword.productId.eq(eProduct.id))
          .where(eCategory.id.eq(categoryId))
          .transform(
            groupBy(eCategory.id)
              .list(
                QProduct(
                  eCategory.id,
                  eCategory.name,
                  eProduct.id,
                  eProduct.name,
                  eProduct.price,
                  list(
                    QKeyword(
                      eKeyword.id,
                      eKeyword.keyword,
                      eKeyword.productId
                    )
                  )
                )
              )
          )
    }
}
```

## NoSuchMethodError

위와 같이 코드를 작성하고 테스트 코드를 실행하니 다음과 같은 에러가 발생했다.

```text
Caused by: java.lang.NoSuchMethodError: 'java.lang.Object org.hibernate.ScrollableResults.get(int)'
```

해당 에러를 디버깅 해보니 조회한 데이터들을 도메인 클래스의 QClass 생성자의 아규먼트로 설정하는 과정에서 발생하는 문제였고, 다른 필드들은 괜찮았는데 Left Join 으로 조회한 Keyword 데이터를 List 로 맵핑하는 중에 발생하는 것으로 확인됐다.

이를 해결하기 위해 구글링 해본 결과, 다음의 [이슈](https://github.com/querydsl/querydsl/issues/3428)를 찾을 수 있었다.
해당 버그는 transform() 사용 시 발생되는 듯 했고, 간단히 해결할 수 있는 문제였다.

## Trouble Shooting

```kotlin
@Configuration
class JpaQueryFactoryConfig {

  @PersistenceContext
  private lateinit var entityManager: EntityManager

    @Bean
  fun queryFactory() = JPAQueryFactory(JPQLTemplates.DEFAULT, this.entityManager)
}
```

우리는 QueryFactory 를 Configuration 에서 Bean 으로 등록해 사용하고 있으므로, 해당 코드로 가서 `JPAQueryFactory` 를 조금 손봐주면 된다.
기본적으로 `JPAQueryFactory` 의 인스턴스를 생성할 때 EntityManager 만 주입하게 되면 template 가 null 로 설정된다.
이로 인해 모종의 이유로 여러 계층의 QClass 생성자를 호출할 경우 NoSuchMethodError 가 발생하는 것이다. (정확히 `JPQLTemplates` 클래스가 어떠한 역할을 하는지 더 공부해보자.)

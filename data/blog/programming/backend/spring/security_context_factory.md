---
title: '[Spring Security] @AuthenticationPrincipal 테스트 트러블슈팅'
date: '2023-04-08'
tags:
  [
    'Spring Security',
    'Kotest',
    'Test',
    '@AuthenticationPrincipal',
    '@WithMockUser',
    'WithMockUserSecurityContextFactory',
  ]
draft: false
summary: 'Controller 에서 유저 인증 정보를 `@AuthenticationPrincipal` 을 이용해 가져올 경우 테스트를 진행하는 방법에 대한 트러블 슈팅 경험을 정리 했습니다.'
---

## 들어가기 앞서

현재 회사에서는 자체적인 토큰 생성 메소드를 이용해 유저 토큰을 생성하고 이를 Memcached 에 저장한 뒤 웹인 경우 Cookie 를, 앱인 경우 Http Header 에 담긴 값을 통해 유저 인증 정보를 확인하는 구조로 처리하고 있는데, 스케일 아웃으로 서버를 운영하고 있어 유저 인증 정보를 여러 대의 서버에서 함께 공유해야 하기 때문이다.

다만, 유저 인증/인가 처리를 Filter 가 아닌 별도의 인증용 클래스를 만들고 인증 처리 메소드에 `@ModelAttribute` 를 붙여 매 요청 마다 인증 정보를 확인하고 있어 불필요한 자원이 낭비되고 있는 상황이다.

때문에 Spring Security 에 익숙치도 않고, 언젠가 회사의 레거시 코드를 개선할 수 있을 것 같아 비사이드 사이드 프로젝트에서 유저 인증에 Spring Security + JWT 를 사용하기로 했다.

현재 사이드 프로젝트에서는 JWT 를 이용한 인증/인가 로직은 구현이 완료된 상태이며, 서비스 운영 시 서버 한 대로도 충분히 감당할 수 있을 것 같아 토큰 정보를 캐싱 메모리에 저장하지 않기로 했다.

때문에 인증 성공 시 유저 기본 정보를 SecurityContext 에 담아 요청 시 `@AuthenticationPrincipal` 을 이용해 유저 기본 정보를 가져오려고 하는데, 해당 애노테이션을 사용하면 웹 계층 테스트에서 유저 정보를 가져올 수 없다는 예외가 발생해 이를 트러블 슈팅한 경험을 정리하고자 한다.

### 요청 핸들러 메소드에서 유저 정보 가져오기

요청 핸들러 메소드의 인자로 유저 정보를 받아오는 경우 인증된 유저에 한해서 @AuthenticationPrincipal 를 사용해 SecurityContext 에 설정된 유저 정보를 맵핑 할 수 있다. 우리 프로젝트에서는 UserDetail 을 사용하지 않기 때문에 관련된 클래스는 사용하지 않는다.
먼저 인증 필터 클래스와 Authentication 객체를 생성하는 메소드를 확인해보자.

#### JwtAuthenticationFilter.kt

요청 시 Http Header 에 담긴 Bearer 토큰 정보를 확인하고 유효한 인증인 경우 유저 정보를 SecurityContext 에 설정한다.

```kotlin
class JwtAuthenticationFilter : OncePerRequestFilter() {
    override fun doFilterInternal(
        request: HttpServletRequest,
        response: HttpServletResponse,
        filterChain: FilterChain
    ) {
        val authHeader = request.getHeader("Authorization")
        if (authHeader.isNullOrBlank() || !authHeader.startsWith("Bearer ")) {
            return filterChain.doFilter(request, response)
        }
        validateJwt(authHeader.substring("Bearer ".length), filterChain, request, response)
    }

    private fun validateJwt(
        jwt: String,
        filterChain: FilterChain,
        request: HttpServletRequest,
        response: HttpServletResponse
    ) {
        if (JwtProvider.isValidToken(jwt)) {
            SecurityContextHolder.getContext().authentication = JwtProvider.getAuthentication(jwt)
        }
        filterChain.doFilter(request, response)
    }
}
```

#### JwtProvider.kt

요청에 담긴 JWT 토큰을 확인하고 유효한 경우 해당 토큰에 담긴 유저 정보를 이용해 Authentication 객체를 생성한 뒤 리턴한다.

```kotlin
class JwtProvider {
  companion object {

    ...

    fun getAuthentication(token: String?): Authentication {
            val claims = getAllClaims(token)
            val member = Member(
                id = (claims[MEMBER_ID] as Int).toLong(),
                email = claims[EMAIL] as String
            )
            val authorities =
                listOf(claims[ROLE]).map { role -> SimpleGrantedAuthority(role as String?) }
            return UsernamePasswordAuthenticationToken(member, token, authorities)
        }
  }
}
```

#### CreateBingoApi.kt

`@AuthenticationPrincipal` 를 이용해 SecurityContext 에 담긴 유저 정보를 가져온다.

```kotlin
@RestController
@RequestMapping("/api/bingos")
class CreateBingoApi(
    private val createBingoService: CreateBingoService
) {
  @PostMapping
    fun create(
        @AuthenticationPrincipal
        member: Member,
        @RequestBody
        @Validated
        request: CreateBingoRequest,
        bindingResult: BindingResult
    ): ApiResponse<BingoResponse> {
        if (bindingResult.hasErrors()) throw BindException(bindingResult)
        val command = request.command(member.id)
        val response = createBingoService.create(command)
        return ApiResponse.OK(response)
    }
}
```

### 빙고 생성 테스트 코드

앞서 작성한 것 처럼 우리 프로젝트에서는 별도로 UserDetails 를 구현하지 않았기 때문에 관련된 클래스를 사용하지 않는다.
그렇기에 `@WithMockUser` 에서 제공하는 기본 유저 정보는 요청 핸들러 메소드에서 처리하지 못해 다음과 같은 예외가 발생하게 된다.

#### Exception Message

> Request processing failed: java.lang.NullPointerException: Parameter specified as non-null is null: method com.beside.groubing.groubingserver.domain.bingo.api.CreateBingoApi.create, parameter member

#### CreateBingoApiTest.kt

```kotlin
@WithMockUser
@WebMvcTest(controllers = [CreateBingoApi::class])
@AutoConfigureMockMvc(addFilters = false)
@AutoConfigureRestDocs
class CreateBingoApiTest(
    private val mockMvc: MockMvc,
    private val mapper: ObjectMapper,
    @MockkBean private val createBingoService: CreateBingoService
) : BehaviorSpec({
    Given("신규 빙고 생성 요청 시") {
        val now = LocalDate.now()
        val tomorrow = now.plusDays(1)
        val memberId = Arb.long(1L..100L).single()
        val pattern = "^[a-zA-Zㄱ-ㅎㅏ-ㅣ가-힣 -@\\[-_~]{1,40}"
        val request = CreateBingoRequest(
            title = Arb.stringPattern(pattern).single(),
            type = Arb.enum<BingoType>().single(),
            size = Arb.enum<BingoSize>().single(),
            color = Arb.enum<BingoColor>().single(),
            goal = Arb.int(1..3).single(),
            open = Arb.boolean().single(),
            since = Arb.localDate(minDate = now, maxDate = tomorrow).single(),
            until = Arb.localDate(minDate = tomorrow.plusDays(1)).single()
        )

        When("데이터가 유효하다면") {
            val board = BingoBoard(
                title = request.title,
                type = request.type,
                size = request.size,
                color = request.color,
                goal = request.goal,
                open = request.open,
                since = request.since,
                until = request.until,
                member = Member(id = memberId)
            )
            val items = board.createNewItems()
            val response = ApiResponse.OK(BingoResponse(board, items))

            Then("생성된 빙고를 리턴한다.") {
                every { createBingoService.create(any()) } returns response.data

                mockMvc.post("/api/bingos") {
                    content = mapper.writeValueAsString(request)
                    contentType = MediaType.APPLICATION_JSON
                }.andDo {
                    print()
                }.andExpect {
                    status { isOk() }
                    content { json(mapper.writeValueAsString(response)) }
                }
            }
        }
    }
})
```

---

## 트러블 슈팅

### 커스텀 @WithMockUser

`@WithMockUser` 나 `@WithUserDetails` 는 요청 핸들러 메소드에서 요구하는 유저 클래스를 리턴할 수 없기 때문에 요구사항에 맞는 데이터를 리턴할 수 있는 같은 역할의 애노테이션이 필요하다.

#### WithAuthMember.kt

```kotlin
@Target(AnnotationTarget.CLASS)
@Retention
@WithSecurityContext(factory = WithAuthMemberSecurityContextFactory::class)
annotation class WithAuthMember(
    val id: Long = 0L,
    val email: String = "test@groubing.com",
    val role: MemberRole = MemberRole.MEMBER
)
```

### 커스텀 WithXXXSecurityContextFactory

`@WithMockUser` 를 사용하는 경우 `WithMockUserSecurityContextFactory` 를 사용하고, `@WithUserDetails` 를 사용하는 경우 `WithUserDetailsSecurityContextFactory` 를 사용한다. 우리는 위의 두 애노테이션과 같은 역할을 하는 애노테이션을 만들었으니, 커스텀 데이터를 생성하고 요청 핸들러 메소드로 전달할 커스텀 WithXXXSecurityContextFactory 를 구현해야 한다.

#### WithAuthMemberSecurityContextFactory.kt

유저 인증/인가 필터에서 처리하는 것과 비슷하게 인증 과정은 생략하고 임의의 유저 정보를 이용해 Authentication 객체 생성 후 SecurityContext 에 설정해준다.

```kotlin
class WithAuthMemberSecurityContextFactory : WithSecurityContextFactory<WithAuthMember> {
    override fun createSecurityContext(annotation: WithAuthMember): SecurityContext {
        val context = SecurityContextHolder.getContext()
        val jwt = JwtProvider.createToken(annotation.id, annotation.email, annotation.role)
        context.authentication = JwtProvider.getAuthentication(jwt)
        return context
    }
}
```

---

### 테스트 결과

커스텀 애노테이션과 팩토리 클래스를 구현 했다면 테스트 코드에 적용해보자.
우리의 프로젝트는 Koteset + MockK 을 이용하고, 웹 계층 테스트의 경우 BehaviorSpec 을 이용하기에 클래스 상단에 애노테이션을 적용하고자 한다.

#### CreateBingoApiTest.kt

`@AutoConfigurationMockMvc(addFilters = false)` 의 경우 테스트 코드다 보니 별도의 인증 과정을 생략하기 위해서 추가했다.

```kotlin
@WithAuthMember
@WebMvcTest(controllers = [CreateBingoApi::class])
@AutoConfigureMockMvc(addFilters = false)
@AutoConfigureRestDocs
class CreateBingoApiTest(...): BehaviorSpec({...})
```

#### 결과

디버깅을 해보면 @WithAuthMember 에서 설정한 값으로 요청 핸들러 메소드에 잘 들어오는 것을 확인할 수 있다.

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbxZRFi%2Fbtr8LrbE67w%2FKKX2M9RQyNnfAJ6kvp9xX1%2Fimg.png" alt="text" width="number" />
</p>

테스트 결과도 성공적이다.

<p align="center">
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcXh1uG%2Fbtr8MoMgIzh%2F0pw0HdwX3juMwbj8YakCZk%2Fimg.png" alt="text" width="number" />
</p>

---

(참고 문서)[https://docs.spring.io/spring-security/site/docs/5.2.0.RELEASE/reference/html/test.html]

---
title: 'Things Intern Project - 4'
date: '2020-12-26'
tags: ['먼슬리씽', '씽즈', '토이 프로젝트', '게시판 구현']
draft: false
summary: '인턴 입사 후 게시판 만들기 과제를 진행하면서 경험한 내용을 정리 했습니다.'
---

# _**Prev / Next, LocalDateTime Format**_

1.  **Prev / Next 버튼 구현하기**
    - **Prev**
      1.  이전 버튼을 구현하기 위해서는 `.mustache`에 넘어온 Model 객체의 데이터 중 첫 번째 데이터가 무엇인지 확인해야 한다.
          - 위의 행위를 행하기 위해서 페이지가 로딩 되기 전에 필요한 데이터를 받아야 한다.
      2.  현재 페이지블럭의 번호가 10 이하라면 더이상 이전으로 갈 수 없다.  
          즉, 0 이하의 페이지로 갈 수 없다는 의미이다.
          - `@PathVariable` 덕분에 URL에 페이지블럭의 번호가 있으므로 Javascript를 이용해 번호를 추출한다.
    - **Next**
      1.  Model 객체로 넘어온 Page List의 길이가 10일 때는 다음 게시글이 있을 가능성이 있다.  
          하지만 게시글 개수가 10의 배수라면 마지막 게시글이 100번째 일 때도 Next 버튼이 출력된다.
      2.  해당 문제를 해결하기 위해서 페이지블럭의 마지막 번호를 알아야 한다.  
          현재 페이지블럭의 번호가 마지막 페이지블럭의 번호와 같이 않을 때만 Next 버튼이 출력되도록 한다.
    - **해당 기능을 구현할 때의 문제점**
      - `Page<T>` 클래스의 메소드에 대해 정확히 알지 못한 상태에서 로직을 구현하다보니 계산에 문제가 발생했다. 해당 클래스는 페이지블럭의 설정된 길이, 객체 안에 존재하는 게시글의 개수 등 여러가지 값을 반환하는 메소드가 존재한다.
2.  **LocalDateTime Format**
    - 게시글 리스트를 `Jpa Repostiory`와 `Page<T>`를 이용해 손쉽게 페이지네이션을 구현할 수 있었다.
    - 하지만 나는 `Page<T>`의 제네릭 객체를 `Post Entity`로 설정했고, 이 때 게시글 작성 날짜 포맷을 어떻게 변환해야 할지 몰랐다.
    - 또한 `ResponseDto`를 만들었음에도 불구하고 `Entity` 자체를 `Controller`까지 가지고 오는 바람에 객체 지향 설계의 원칙을 위반했다.
    - 이를 해결하기 위해서 며칠을 고민했지만 서버 단에서 할 수 있는 방법을 찾을 수 없었다. 그래서 프론트 단에서 Javascript와 jQuery를 이용해 문자열을 잘라내는 형식으로 구현했다.
      - 하지만 해당 방식으로 구현 시, 일일히 문자열을 자르는 과정이 페이지 로딩 이후에 수행되어서 화면이 바뀌는 것이 보였다.
    - **해결 방법**
      1.  Post 제네릭 객체를 Post Entity가 아닌 PostResponseDto로 설정한다.
      2.  람다식을 활용, `.findAll()` 메소드를 `.map()`과 `.toList()`를 이용하여 `PostResponseDto`로 반환할 수 있도록 한다.

---

# **Controller**

## **PostController.class**

```java
@GetMapping("/post/{page}")
public String index(@PathVariable int page, Model model) {
  Pageable pageable = PageRequest.of(page, 10);
  Page<PostResponseDto> postList = postService.findAll(pageable).map(post -> new PostResponseDto(post)); // Entity -> ResponseDto로 변경
  model.addAttribute("post", postList);

  // 페이지네이션 로직 수정
  long startBlock = (page - 1) / postList.getSize() * postList.getSize() + 1; // (현재 페이지 번호 - 1) / 페이지블럭 기준 * 페이지블럭 기준 + 1
  long finishBlock = startBlock + postList.getSize() - 1;
  long blockCount = (postList.getTotalElements() + postList.getSize() - 1) / postList.getSize();
  finishBlock = blockCount < finishBlock ? blockCount : finishBlock;

  List<Long> block = new ArrayList<>();
  for (long i = startBlock; i <= finishBlock; i++) block.add(i);
  model.addAttribute("block", block);

  // next 버튼 구현을 위한 페이지블럭 총 개수
  model.addAttribute("blockCount", blockCount);
  return "list";
}
```

# **View**

## **list.mustache**

```javascript
$(function () {
  const block = {{block}};
  const blockLength = $(block).length;
  const firstBlock = $(block).get(0);
  const lastBlock = $(block).get(blockLength - 1);

  // 페이지 번호
  const path = window.location.pathname;
  const page = path.split("/").pop();

  // prev
  if (page > 10 && firstBlock != null) {
    $('#btn-prev').attr('href', '/post/' + (firstBlock - 1)).css('display', 'inline-block');
  }

  // next
  if (lastBlock < {{blockCount}} && blockLength == 10) {
    $('#btn-next').attr('href', '/post/' + (lastBlock + 1)).css('display', 'inline-block');
  }

  // 현재 페이지 active
  $('#' + page).addClass('active');
})
```

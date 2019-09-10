---
title: 스프링 기반 REST API 개발04 - 이벤트 조회 및 수정 REST API 개발(PagedResouce)
date: 2019-08-09 11:29:44
tags: RestAPI
---
# 스프링 기반 REST API 개발(inflearn)4 - 백기선

## 이벤트 조회 및 수정 REST API 개발

### 이벤트 목록 조회 API 구현

#### 페이징, 정렬
- 스프링 데이터 JPA가 제공하는 pageable을 사용한다.
- GET방식 parameter
  - page : 원하는 페이지 (0부터 시작)
  - size : 한페이지에 들어갈 리소스 수 (기본값 = 20)
  - sort : 정렬타입 및 차순 (ex.. sort=name,DESC)
- Return 
  - pageable : pageable 정보
  - content : 리소스 정보
  - totalElements : 전체 리소스 수
  - …
  
```java
@GetMapping("/api/events")
public ResponseEntity queryEvents(Pageable pageable) {
	Page<Event> page = eventsRepository.findall(pagealbe);
  return ResponseEntity.ok(page);
}
```

#### PagedResouce

- PagedResourceAssembler<T>를 사용하여 Page객체를 PagedResouce를 생성할 수 있다.
  - 첫페이지, 이전페이지, 현재페이지, 다음페이지, 마지막페이지 Link가 포함되어있다.
  - Resource<Event>로 사용할 경우, 각 리소스들의 링크정보는 포함되지 않는다.
    - 이전에 생성했던 EventResource로 적용시킨다. ( e -> new EventResouce(e); )
    ![restapi04-1](/images/restapi/restapi04-1.png)

- Profile 링크 추가

```java
@GetMapping("/api/events")
public ResponseEntity queryEvents(Pageable pageable, PagedResourcesAssembler<Event> assembler) {
    Page<Event> page = eventRepository.findAll(pageable);
    PagedResources<Resource<Event>> pagedResources = assembler.toResource(page, e -> new EventResource(e));
    pagedResources.add(new Link("/docs/index.html#resources-events-list").withRel("profile"));
    return ResponseEntity.ok(pagedResources);
}
```

- 테스트 (문서화는 생략)
  ```java
      @Test
      @TestDescription("30개 이벤트를 10개씩 두번째 페이지 조회하기")
      public void queryEvents() throws Exception {
          // given
          IntStream.range(0, 30).forEach(this::generateEvent); // mehtod reference로 간결에가 사용이 가능
  
          // when
          this.mockMvc.perform(get("/api/events")
                      .param("page", "1")
                      .param("size", "10")
                      .param("sort", "name,DESC")
                  )
                  .andDo(print())
                  .andExpect(status().isOk())
                  .andExpect(jsonPath("page").exists())
                  .andExpect(jsonPath("_embedded.eventList[0]._links.self").exists())
                  .andExpect(jsonPath("_links.self").exists())
                  .andExpect(jsonPath("_links.profile").exists())
                  .andDo(document("query-events")) // 자세한 문서화 생략
          ;
  
      }
  
      private void generateEvent(int i) {
          Event event = Event.builder()
                  .name("event" + i)
                  .description("test eveent")
                  .build();
  
          this.eventRepository.save(event);
      }
  ```
<br><br>



---
title: 스프링 MVC 설정 - 포매터 추가하기
date: 2019-06-10 20:17:39
tags: SpringWebMvc
---
![springf](/images/springframwork-logo.png)
# 스프링 웹 MVC(inflearn) - 백기선 
## Springframework

### 포매터 추가하기
- [Reference 문서](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurer.html#addFormatters-org.springframework.format.FormatterRegistry-)

#### [Formatter](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/format/Formatter.html)
- Formatter<T>를 implements 하는 클래스를 생성한다.
    ![springwebmvc](/images/springwebmvc/springwebmvc06-1.png)
- Printer, Parser를 구현한다.
    - Printer: 해당 객체를 (Locale 정보를 참고하여) 문자열로 어떻게 출력할 것인가
    - Parser: 어떤 문자열을 (Locale 정보를 참고하여) 객체로 어떻게 변환할 것인가

#### Formatter를 추가하는 방법
1. WebMvcConfigurer의 addFormatters(FormatterRegistry) 메소드 정의
    ![springwebmvc](/images/springwebmvc/springwebmvc06-3.png)
2. 해당 포매터를 빈으로 등록(스프링 부트 사용시에만 가능하다.)
    ![springwebmvc](/images/springwebmvc/springwebmvc06-2.png)

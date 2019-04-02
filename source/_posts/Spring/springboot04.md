---
title: 스프링 부트 원리 - 내장 웹 서버
date: 2019-04-02 16:59:51
tags: SpringBoot
---
![springboot](/images/springboot_logo.png)
# 스프릥 부트 개념과 활용02(inflearn) - 백기선 
## Spring boot

### 내장 웹 서버
- @SpringBootApplication 쓰지 않고 톰캣 띄우기
    ![springboot](/images/springboot/springboot04-1.png)
    - **부트는 서버가 아니다.**
    - 톰캣객체생성
    - 포트설정
    - 톰캣에 컨텍스트 추가
    - 서블릿 만들기
    - 톰캣에 서블릿 추가
    - 컨텍스트에 서블릿 맵핑
    - 톰캣실행및대기

- 이 모든 과정을 보다 상세히 또 유연하게 설정하고 실행해주는게 바로 스프링 부트의 자동 설정.
    - ServletWebServerFactoryAutoConfiguration (서블릿 웹 서버 생성)
        - TomcatServletWebServerFactoryCustomizer (서버 커스터마이징)
- DispatcherServletAutoConfiguration
    - 서블릿 만들고 등록한다.
    - DispatcherServlet이 따로 있는 이유
        - 서블릿 컨테이너는 변경될 수 있느나, 서블릿은 변경되지 않기때문에 
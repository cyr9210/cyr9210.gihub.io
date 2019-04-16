---
title: 스프링 부트 활용 - 스프링 시큐리티 
date: 2019-04-16 22:18:31
tags: SpringBoot
---

![springboot](/images/springboot_logo.png)
# 스프릥 부트 개념과 활용14(inflearn) - 백기선 
## Spring boot

### 스프링 시큐리티

---
##### 참고 - 뷰 컨트롤러 사용
![springboot](/images/springboot/springboot14-1.png)
- Webconfigure 생성
    - @Configuration
    - implements
- @Overide - addViewControllers
    - registry.addViewController("<path>").setViewName("<View_name>");
- 추가적인 일이 있다면 @Controller를 쓰는것이 유리하다.
---

#### spring-boot-starter-security
- 스프링 시큐리티
    - 웹 시큐리티
    - 메소드 시큐리티
    - 다양한 인증 방법 지원
    - LDAP, 폼 인증, Basic 인증, OAuth, ...

- spring-boot-starter-security 의존성 추가

- 의존성을 추가하게 되면 정상작동하던 컨트롤러테스트가 실패하게 된다.
    ![springboot](/images/springboot/springboot14-2.png)
    - 401 Unauthorized
    - 스프링 부트가 제공하하는 스프링 시큐리티 자동설중 하나이다.
        - 모든요청에 스프링 시큐리티로 인해 인증이 필요하다.
        - Basic Authenticate, form 인증이 둘다 적용된다.
        (상기 이미지는 Basic Authenticate 오류)
            - Basic 인증은 Accept 헤더에 따라 달라진다. 
            - accept 헤더에 text/html을 보내게 되면 form 인증으로 넘어간다.
                ![springboot](/images/springboot/springboot14-3.png)
                - 보통 브라우저에서 accpet 헤더를 text/html을 쓴다. 

- 브라우저에서 루트를 요청해도 로그인으로 이동된다.
    - 인증정보가 없기 때문에 스프링 시큐리티에서 만들어준 로그인 form페이지로 이동된다.(스프링 부트 자동설정)
        ![springboot](/images/springboot/springboot14-5.png)

- 스프링 부트 시큐리티 자동 설정
    - SecurityAutoConfiguration
        - DefaultAuthenticationEventPublisher 빈 등록
            - 여러상황에 대해 이벤트를 발생시킨다.
            ex) 비빌번호 오류, 아이도 오류, ....
            - 다양한 인증 에러 핸들러 등록 가능
    - SpringBootWebSecurityConfiguration
        - WebSecurityConfigurerAdapter가 빈으로 등록되어 있지 않을때
        - WebSecurityConfigurerAdapter의 내용을 그대로 쓰고있다.
        (상속만하고 아무것도 정의되어 있지않음)
        ![springboot](/images/springboot/springboot14-6.png)
            - WebSecurityConfigurerAdapter 핵심내용
            ![springboot](/images/springboot/springboot14-7.png)
    - WebSecurityConfigurerAdapter를 상속하는 Configuration클래스(@Configuration 빈등록)를 만들면 거의 동일하게 동작하는 나만의 시큐리티 설정을 가질 수 있다.
    ![springboot](/images/springboot/springboot14-9.png)
    
    - UserDetailsServiceAutoConfiguration
        ![springboot](/images/springboot/springboot14-8.png)
        - **UserDetailsService**, AuthenticationManager, 
        AuthenticationProvider가 없을 때
        - inMemoryDetailsManager 객체를 만든다.
            - 기본 유저 객체만드는 역할
    - spring-boot-starter-security
        - 스프링 시큐리티 5.* 의존성 추가 
    - 모든 요청에 인증이 필요함.
    - 사용자 설정
        - 기본 Username: user
        - 기본 Password: 애플리케이션을 실행할 때 마다 랜덤 값 생성 (콘솔에 출력 됨.)
        ![springboot](/images/springboot/springboot14-4.png)
        - spring.security.user.name
        - spring.security.user.password
        
- test 방법
    - 의존성 추가
    ```
    <dependency>
        <groupId>org.springframework.security</groupId>
        <artifactId>spring-security-test</artifactId>
        <scope>test</scope>
    </dependency>
    ```
    - @WithMockUser 사용
    ![springboot](/images/springboot/springboot14-10.png)
    
    - 설정한 유저로 테스트
    ![springboot](/images/springboot/springboot14-11.png)
- **대부분의 서비스들은 UserDetailsService를 등록하고 사용하며, WebSecurityConfigurerAdapter를 이용하여 보다 쉽게 설정을 하기 때문에 스프링 부트에서 지원하는 시큐리티는 사실상 사용 할 일이 없다.** 
 
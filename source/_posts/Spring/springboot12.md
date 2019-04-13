---
title: 스프링 부트 활용 - 스프링 웹 MVC
date: 2019-04-11 16:43:16
tags: SpringBoot
---
![springboot](/images/springboot_logo.png)
# 스프릥 부트 개념과 활용12(inflearn) - 백기선 
## Spring boot

### 스프링 웹 MVC
- 스프링 웹 MVC
    - [reference문서](https://docs.spring.io/spring/docs/5.0.7.RELEASE/spring-framework-reference/web.html#spring-web)
- 스프링 부트 MVC
    - 자동 설정으로 제공하는 여러 기본 기능
- 스프링 MVC 확장
    - 부분적 설정 추가
    - @Configuration + WebMvcConfigurer
- 스프링 MVC 재정의
    - 웹 MVC에 대해 전체 재정의 (잘안쓴다.)
    - @Configuration + @EnableWebMvc

#### HttpMessageConverters
- [reference문서](https://docs.spring.io/spring/docs/5.0.7.RELEASE/spring-framework-reference/web.html#mvc-config-message-converters)
- 스프링 프레임워크에서 제공하는 인터페이스
- HTTP 요청 본문을 객체로 변경하거나, 객체를 HTTP 응답 본문으로 변경할 때 사용.
    - {“username”:”keesun”, “password”:”123”} <-> User
        - HttpMessageConverter를 사용하는데 어떤요청인지, 또는 어떤 응답인지에 따라서 HttpMessageConverter는 달라진다.
        - @RequestBody
        - @ResponseBody
            - @RestController 사용 시, 메소드에 @ResponseBody를 포함하고 있다.
<br>

##### 예제
@RequestBody 및 @RestController를 활용하여 테스트 코드 작성하기
- User
![springboot](/images/springboot/springboot12-1.png)

- UserController
![springboot](/images/springboot/springboot12-2.png)

- UserControllerTest
![springboot](/images/springboot/springboot12-3.png)
    - contentType : 요청 타입
    - accept : 응답 타입
    - content : 요청
    - is(), equalTo()는 Matchers 메소드 사용

#### ViewResolver
- ContentNegotiationViewResolver
    - ViewResolver중 하나로, 들어오는 요청 accept 헤더에 따라 응답이 달라진다.
    - accept 헤더는 클라이언트가 어떤타입의 응답을 원한다고 알려주는 것.
    - accept 헤더를 제공하지 않는 경우도 많다.
        - format 매개 변수를 이용하여 처리한다.
    - 스프링 초기에는 url에 .json 등도 지원했지만, 현재는 지원하지 않는다.

- xml 메세지 컨버터 추가하기
    - accpet 헤더만 xml로 설정 후, 위의 예제 테스트를 실행하면 오류 발생
    ![springboot](/images/springboot/springboot12-4.png)
        - 이유는 .. xml 메세지 컨버터가 없기 때문에
    
    - HttpMessageConvertersAutoConfiguration → JacksonHttpMessageConvertersConfiguration에 들어가 보면 XmlMapper.class가 없어서 설정이 안 된 것을 볼 수 있다.
        ![springboot](/images/springboot/springboot12-5.png)
    
    - XmlMapper.class를 추가하기 위해 의존성을 추가한다.
    ```
    <dependency>
        <groupId>com.fasterxml.jackson.dataformat</groupId>
        <artifactId>jackson-dataformat-xml</artifactId>
        <version>2.9.8</version>
    </dependency>
    ```
    - 정상작동
    ![springboot](/images/springboot/springboot12-6.png)
    
#### 정적 리소스 지원
- 웹 브라우저나 클라이언트에 요청이 들어왔을 때, 그것에 해당하는 리소스가 만들어져 있고 그것을 응답하는 것
- 정적 리소스 맵핑 : "/**"
    - 기본 리소스 위치
        - classpath:/static
        - classpath:/public
        - classpath:/resources/
        - classpath:/META-INF/resources
        - 예) “/hello.html” => /static/hello.html
        - spring.mvc.static-path-pattern: 맵핑 설정 변경 가능
            - 매핑이 루트(/)부터 시작하지 않고 싶을 때 사용할 수 있다.  
            예) "localhost:8080/static/hello.html"
            ![springboot](/images/springboot/springboot12-9.png)
        - spring.mvc.static-locations: 리소스 찾을 위치 변경 가능
    - 서버의 Last-Modified 헤더를 보고 304 응답을 보냄.
        - if-modified-since 이후에 바귀었으면 리소르를 새로 받는다.(200)
        ![springboot](/images/springboot/springboot12-7.png)
        - 이후에 바뀌지 않았으면 리소를 새로 받지않고 이전에 받은것을 사용한다.(304)
        응답이 훨씬 빠르다.
        ![springboot](/images/springboot/springboot12-8.png)
   
    - ResourceHttpRequestHandler가 처리함.
        - WebMvcConfigurer의 addRersourceHandlers로 커스터마이징 할 수 있음       
        ![springboot](/images/springboot/springboot12-10.png)![springboot](/images/springboot/springboot12-11.png)

#### 웹 jar
- 스프링 부트는 웹 jar에 대한 기본 매핑을 지원한다.
<br>

##### 예제
- jQuery 추가
    - 의존성 추가
    - 사용 할 파일에서 webjar를 사용하여 참조
    ![springboot](/images/springboot/springboot12-13.png)
    - 버전을 생략하고 사용하려면..
        ![springboot](/images/springboot/springboot12-12.png)
        - webjars-locator-core 의존성 추가 필요
        - 버전을 사용해도 되고 안해도 된다.
     
#### index페이지(welcome페이지)와 파비콘
- 웰컴 페이지
    - index.html 찾아 보고 있으면 제공.
        ![springboot](/images/springboot/springboot12-14.png)
        - 기본 리소스 위치
    - index.템플릿 찾아 보고 있으면 제공.
    - 둘다없으면에러페이지.
        - 스프링부트 에러핸들러에서 만든 

- 파비콘
    - 파비콘이란?
    ![springboot](/images/springboot/springboot12-15.png)
    - favicon.ico 파일을 추가한다.
        - 기본 리소스 위치
    - [파이콘 만들기](https://favicon.io)
    - 파비콘이 안 바뀔 때..
        - https://stackoverflow.com/questions/2208933/how-do-i-force-a-favicon-refresh
        - 브라우저에서 favicon.ico를 읽고 브라우저를 재시작
        ![springboot](/images/springboot/springboot12-16.png)

#### Thymeleaf
> **템플릿 엔진**
코드 제너레이션, 이메일 템플릿등에 사용할 수 있으나, **주로 뷰를 만들 때, 사용한다.**
기본적인 템플릿은 같으나, 안에 값들만 달라진다.(동적 컨텐츠)

- 스프링 부트가 자동 설정을 지원하는 템플릿 엔진
    - FreeMarker 
    - Groovy
    - **Thymeleaf** 
    - Mustache

- JSP를 권장하지 않는 이유
    - 스프링부트가 지향하는 바와 다르다.
    - JAR패키징 할때는 동작하지 않고, WAR패키징 해야함.
    - Undertow는 JSP를 지원하지 않음.
    - [reference문서](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-jsp-limitations)

- Thymeleaf 사용하기
    - https://www.thymeleaf.org/
    - https://www.thymeleaf.org/doc/articles/standarddialect5minutes.html
    - 의존성 추가: spring-boot-starter-thymeleaf
    ```
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
    ```
    - 템플릿 파일 위치: /src/main/resources/​template/
    - Thymeleaf 네임스페이스 추가
        ![springboot](/images/springboot/springboot12-18.png)
        - th를 사용할 수 있다. (name 값이 있으면 사용한다.)  
    - Thymeleaf test
    ![springboot](/images/springboot/springboot12-17.png)
    - [예제](https://github.com/thymeleaf/thymeleafexamples-stsm/blob/3.0-master/src/main/webapp/WEB-INF/templates/seedstartermng.html) 

#### HTML Unit

- HTML 템플릿 뷰 테스트를 보다 전문적하기 위해 도와주는 툴
- 스프링 부트는 HTML Unit 기능 및 자동설정 지원한다.
- http://htmlunit.sourceforge.net/
- http://htmlunit.sourceforge.net/gettingStarted.html
- 의존성 추가가 필요하다.
```
<dependency>
    <groupId>org.seleniumhq.selenium</groupId>
    <artifactId>htmlunit-driver</artifactId>
    <version>2.34.0</version>
</dependency>

<dependency>
    <groupId>net.sourceforge.htmlunit</groupId>
    <artifactId>htmlunit</artifactId>
    <version>2.34.1</version>
</dependency>
```
- WebClient를 주입받아 사용한다.
    ![springboot](/images/springboot/springboot12-19.png)
    - WebClient를 만들 때, MockMvc를 사용하기 때문에 MockMvc도 주입받아 사용할 수 있다.

#### ExceptionHandler
- 스프링 부트 애플리케이션 실행하면 기본적으로 에러핸들러가 등록이 되어있다.
그리고 그 에러핸들러에 의해 메세지가 보인다.
    - 브라우저 요청 시
    ![springboot](/images/springboot/springboot12-20.png)
    - 머신핸들러 요청 시 (Json 응답)
    ![springboot](/images/springboot/springboot12-21.png)

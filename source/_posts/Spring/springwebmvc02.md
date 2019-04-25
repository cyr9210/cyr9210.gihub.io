---
title: 스프링 MVC 동작 원리 - 스프링 연동
date: 2019-04-25 16:11:55
tags: SpringWebMvc
---

![springf](/images/springframwork-logo.png)
# 스프링 웹 MVC(inflearn) - 백기선 
## Springframework

웹 서블릿 어플리케이션에 스프링을 사용한다는 의미는 크게 2가지로 나눠서 이야기 할 수 있다.
- 스프링 IoC 컨테이너를 사용하겠다.
- 스프링 MVC를 사용하겠다.

### 스프링 IoC 컨테이너 연동
- 의존성 추가
    - spring-webmvc
    
- ContextLoaderListener 등록
    - ApplicationContext를 서블릿 어플리케이션 생명주기에 맞춰서 바인딩해준다.
        - ApplicationContext를 웹어플리케이션에 등록된 서블릿들이 사용할 수 있도록 서블릿 컨텍스트에 등록해준다.
        - 서블릿이 종료될때 제거해준다.
        - 즉, ApplicationContext를 연동해준다.(ApplicationContext는 만들어야한다.)
    - 서블릿에서 IoC 컨테이너를 ServletContext를 통해 꺼내 사용할 수 있다.
    ```
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    ```
    
- ApplicationContext 생성
    - 기본적으로 xml을 사용하지만 Java설정파일 사용하겠다. (최근 추세)
        - AppConfig 생성
        ![springmvc](/images/springwebmvc/springwebmvc02-1.png)
        - context class 설정 변경
        ```
        <context-param>
            <param-name>contextClass</param-name>
            <param-value>org.springframework.web.context.support.AnnotationConfigWebApplicationContext</param-value>
        </context-param>
        
        <context-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>bong.spring.AppConfig</param-value> /* Appconfig파일위치 */
        </context-param>
        ```

    - ContextLoaderListener - initWebApplicationContext을 보면 WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE라는 이름으로 ApplicationContext를 setattribute하는것을 볼 수 있다.
        ![springmvc](/images/springwebmvc/springwebmvc02-2.png)

- 등록한 빈 사용하기
    - servletcontext에서 확인한 이름으로 getattrbute해서 확인할 수 있다.
    - 빈으로 등록한 서비스를 getBean()으로 사용할 수 있다.
    ```
    private Object getName() {
        ApplicationContext applicationContext = (ApplicationContext) getServletContext().getAttribute(WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE);
        HelloService helloService = applicationContext.getBean(HelloService.class);
        return helloService.getname();
    }
    ```
    
- 지금까지 만들었던 내용을 RootApplicationContext라고 할 수 있다.
    - 다른 서블릿에서도 공용으로 사용될 공용 리소스
    - web과 상관없는...
<img src="/images/springwebmvc/springwebmvc02-3.png" width="40%">
    

    





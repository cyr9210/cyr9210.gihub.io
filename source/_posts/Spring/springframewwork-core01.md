---
title: IoC 컨테이너와 빈
date: 2019-03-14 16:36:43
tags: Springframework
---
![springf](/images/springframwork-logo.png)
# 스프링 프레임워크 핵심기술01(inflearn) - 백기선 
## Springframework

백기선님의 Spring 시리즈 두번째 강좌 '[스프링프레임워크 핵심기술](https://www.inflearn.com/course/spring-framework_core/)' 수강을 시작하였습니다.
해당강좌는 스프링프레임워크 입문에 이어서 좀 더 심화적인 과정이며, 초급에서 중급수준으로 넘어가는 사람들에게 추천하느 강좌라고 소개하고 있습니다.
kosta에서 xml방식을 주로 사용하고 설명했었으나, 그 방법은 오래전에 사용되던 방법이라고 합니다..   
이번에는 Java Class파일을 다루도록 하겠습니다.

***
### IoC 컨테이너와 빈

#### IoC 컨테이너
- Inversion of Control
- 의존 관계 주입(Dependency Injection)이라고도 한다.
- 어떤 객체가 사용하는 의존객체를 직접 만들어 사용하는게 아니라 주입받아 사용하는 방법
- 스프링 초기에느 xml로 설정하는것이 대세였지만, annotation기반의  의존성 주입을 지원
- 최상위 인터페이스 : BeanFactory
    
> **POJO객체**
POJO(Plain Old Java Object) 특정 자바모델이나, 프레임워크 등을 따르지 않는 자바객체

#### 빈(Bean)
- IoC 컨테이너가 관리하는 객체
- 장점
    - 의존성 관리
    - 스코프
        - 싱글톤 : 하나만 사용(항상 같은 객체)
        - 프로토타입 : 매번 다른 객체
    - 라이프사이클 인터페이스
        - ex) 빈이 생성될 때, 무언가를 행함

#### Application Context
BeanFactory를 상속받는다. 즉, BeanFactory의 기능을 하면서 추가적인 기능을 가진다.
- BeanFactory 기능
- 메세지 소스 처리기능 (18n)
- 리소스 로딩 기능
- 이벤트 발행기능
<br><br>

### Application Context와 다양한 빈 설정방법

#### 스프링 IoC 컨테이너의 역할
- 빈 인스턴스 생성
- 의존 관계 설정
- 빈제공

#### ApplicationContext
- ClassPathXmlApplicationContext(XML)
- **AnnotationConfigApplicationContext(Java)**
최근에는 아래와 같이 Java형식을 사용한다.
![springcore](/images/springc/springcore01-01.png)


#### ComponentScan
Annotation이 붙은 객체를 빈 등록
- xml 설정 → context:componen-scan
- Java → @ComponentScan
![springcore](/images/springc/springcore01-03.png)![springcore](/images/springc/springcore01-04.png)
<br>

### Autowired
- 필요한 의존 객체의 타입에 해당하는 빈을 찾아 주입한다.
- 사용위치
    - 생성자
    - setter
    - 필드

#### 생성자
![springcore](/images/springc/springcore01-05.png)
- 해당 타입의 빈이 한개인 경우
    - 정상
- 해당 타입의 빈이 없는 경우
    - 에러발생
- 해당 타입의 빈이 여러 개인 경우
    - @Primary 사용하면 여러 개인경우 해당 빈을 주입
![springcore](/images/springc/springcore01-08.png)

#### setter, 필드
![springcore](/images/springc/springcore01-06.png)
- 해당 타입의 빈이 한개인 경우
    - 정상
- 해당 타입의 빈이 없는경우
    - required = false 설정 시, 없을경우 의존성을 주입하지 않는다.
![springcore](/images/springc/springcore01-07.png)

- 해당 타입의 빈이 여러 개인 경우
    - @Primary 사용하면 여러 개인경우 해당 빈을 주입
    - @Qualifier 사용하여 빈이름 지정
    ![springcore](/images/springc/springcore01-09.png)
    **Qualifier을 사용으로 해결 할 수 있지만, @Primary를 쓰는것을 추천한다.**

#### 해당 타입의 빈을 모두 받고 싶을때
- List<>로 받는다.
![springcore](/images/springc/springcore01-10.png)

#### 동작 원리
 - 라이프사이클에 의해 동작
 - BeanPostProcessor
    - 새로 만든 빈 인스턴스를 수정할 수 있는 라이프 사이클 인터페이스
 - AutowiredAnnotationBeanPostProcessor extends BeanPostProcessor
    - 스프링이 제공하는 @Autowired와 @Value 애노테이션 그리고 JSR-330의 @Inject 애노테이션을 지원하는 애노테이션 처리기.



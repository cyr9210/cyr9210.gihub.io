---
title: 스프링 부트 활용 - 외부설정
date: 2019-04-05 14:22:16
tags: SpringBoot
---
![springboot](/images/springboot_logo.png)
# 스프릥 부트 개념과 활용07(inflearn) - 백기선 
## Spring boot

### 외부 설정
#### 사용할 수 있는 외부 설정
- properties
    - 스프링 부트가 애플리케이션을 구동할 때, 자동으로 로딩 
    - key=value로 정의
    - 애플리케이션에서 참조해서 사용할 수 있다.
        
- YAML
- 환경변수
- 커맨드 라인 아규먼트

#### 프로퍼티 우선 순위
1. 유저 홈 디렉토리에 있는 spring-boot-dev-tools.properties
2. 테스트에 있는 @TestPropertySource
    ![springboot](/images/springboot/springboot07-4.png)
    - 파일로도 정의 가능하다.
    ![springboot](/images/springboot/springboot07-6.png)
3. @SpringBootTest 애노테이션의 properties 애트리뷰트
    ![springboot](/images/springboot/springboot07-3.png)
    
4. 커맨드 라인 아규먼트
    ```
    java -jar target/jar_name --bong.name=bong
    ```
5. SPRING_APPLICATION_JSON (환경 변수 또는 시스템 프로티) 에 들어있는 프로퍼티
6. ServletConfig 파라미터
7. ServletContext 파라미터
8. java:comp/env JNDI 애트리뷰트
9. System.getProperties() 자바 시스템 프로퍼티
10. OS 환경 변수
11. RandomValuePropertySource
12. JAR 밖에 있는 특정 프로파일용 application properties
13. JAR 안에 있는 특정 프로파일용 application properties
14. JAR 밖에 있는 application properties
15. JAR 안에 있는 application properties
    ![springboot](/images/springboot/springboot07-1.png)
    - test resources가 더 늦게 빌드되어 덮어씌어진다.
    - main properties에 있던 속성이 test에 없다면 에러가 발생한다.
    ![springboot](/images/springboot/springboot07-2.png)
16. @PropertySource
17. 기본 프로퍼티 (SpringApplication.setDefaultProperties)

#### application.properties 우선 순위 
- 높은게 낮은걸 덮어 씁니다. 
    1. file:./config/
    2. file:./
    3. classpath:/config/
    4. classpath:/

**단, application.properties는 프로퍼티 우선순위 14,15이기 때문에 상위 우선순위가 있을 시, 적용되지 않는다.**
<br>
##### 예제
![springboot](/images/springboot/springboot07-7.png)
- 2순위, 4순위에 해당하는 위치에 application.properties를 만들고 각각 다른값을 정의한다.
- 2순위의 application.properties가 적용된다.

#### 랜덤값 설정하기
- ${random.*}

#### 플레이스 홀더
- name = keesun
fullName = ${name} baik



---
참고 동영상 : https://www.youtube.com/user/whiteship2000/featured
<br><br>
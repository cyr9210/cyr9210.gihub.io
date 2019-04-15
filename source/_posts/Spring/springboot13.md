---
title: 스프링 부트 활용 - 스프링 데이터
date: 2019-04-15 14:34:06
tags: SpringBoot
---
![springboot](/images/springboot_logo.png)
# 스프릥 부트 개념과 활용13(inflearn) - 백기선 
## Spring boot

### 스프링 데이터
![springboot](/images/springboot/springboot13-1.png)

#### 인메모리 데이터베이스
- 스프링 부트가 지원하는 인-메모리 데이터베이스
    - H2 (추천, 콘솔 때문에...)
    - HSQL
    - Derby

- Spring-JDBC가 클래스패스에 있으면 자동 설정이 필요한 빈을 설정 해줍니다.
    - DataSource
    - JdbcTemplate

- 스프링부트는 인메모리 데이터베이스 의존성이 설정이 되있고, Data Source를 설정하지 않으면 스프링 부트는 자동으로 인메로리 데이터베이스를 설정해준다.

- 인-메모리 데이터베이스 기본 연결 정보 확인하는 방법
![springboot](/images/springboot/springboot13-2.png)
    - URL: “testdb”
    - username: “sa”
    - password: “”

- table 생성 예시
![springboot](/images/springboot/springboot13-4.png)
    >**createStatement()**
    데이터베이스로 SQL 문을 보내기 위한 SQLServerStatement 개체를 만듭니다.
    
    - 위와 같이 connection객체에서 getMetaData를 통해서도 연결정보를 확인할 수 있다.
  
    

- H2 콘솔 사용하는 방법
    - spring-boot-devtools를 추가하거나...
    - spring.h2.console.enabled=true 만 추가.
    - /h2-console로 접속 (이 path도 바꿀 수 있음)
    ![springboot](/images/springboot/springboot13-3.png)

- Data Source뿐만 아니라 JdbcTemplate도 빈으로 등록됨으로 사용할 수 있다.
![springboot](/images/springboot/springboot13-5.png)
![springboot](/images/springboot/springboot13-6.png)
    - Jdbc api(connection등)를 사용하는것보다 JdbcTemplate를 사용하는것을 추천한다.
        - 보다 간결한 코드를 사용한다.
        - 보다 안전한다. (리소스 반납처리가 잘 되어있다.(try catch finally등))
        - 보다 가독성 좋은 에러메세지도 확인할 수 있다.
        
#### MySQL
- 지원하는 DBCP(DataBaseConnectionPool)
    - HikariCP (기본)
        - [기본적인 설정](https://github.com/brettwooldridge/HikariCP#frequently-used)
    - Tomcat CP
    - Commons DBCP2

>**JDBC와 DBCP**
Java에서 Database와 연결하기 위해선 JDBC를 필요로 하며, JDBC를 이용해 생성한 Connection을 효율적으로 활용하기 위해 Connection 객체를 관리하는 것을 DBCP

- 스프링 부트 DBCP 설정
    - spring.datasource.hikari.*
    ![springboot](/images/springboot/springboot13-7.png)
    - spring.datasource.tomcat.*
    - spring.datasource.dbcp2.*
    - DBCP 확인
    ![springboot](/images/springboot/springboot13-8.png)
    
    
- MySQL 커넥터 의존성 추가
```
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
```

- MySQL 실치 및 서버 실행 (도커사용)
    ```
    docker run -p 3306:3306 --name mysql_boot -e MYSQL_ROOT_PASSWORD=1 
    -e MYSQL_DATABASE=springboot -e MYSQL_USER=keesun -e MYSQL_PASSWORD=pass -d mysql
    ```
    - docker를 실행한다. (mysql 이미지가 없으면 다운받는다.)
        - 컨테이너 3306포트를 로컬호스트 3306포트에 연결
        - 컨테이너이름 = mysql_boot
        - MYSQL_ROOT 비밀번호 : 1
        - 사용자 : bong
        - 비밀번호 : pass
- MySQL bash로 실행
    ```
    docker exec -i -t mysql_boot bash
    ```

- MySQL 접속
    ```
    mysql -u root -p
    /*or*/
    mysql -u <username> -p
    ```
    - root로 접속 시 root 비밀번호(1), user로 접속 시 user비밀번호(pass)

- [MySQL 명령어 참고 블로그](https://doorbw.tistory.com/21)

- MySQL용 Datasource 설정
    - spring.datasource.url=jdbc:mysql://localhost:3306/springboot?useSSL=false
    - spring.datasource.username=keesun
    - spring.datasource.password=pass

- MySQL 라이센스 (GPL) 주의
    - 소스 코드 공개 의무 여부 확인
    - 유료
    - 대안
        - MySQL 대신 MariaDB 사용 검토
            - 무료지만 MariaDB 또한 GPL로 소스 코드 공개가 필요하다.
        - **PostgreSQL 사용이 가장 안전하다.**

>**오픈소스 라이센스**
GPL : 소스코드 공개 의무 O
MIT : 소스코드 공개 의무 X

#### PostgreSQL(포스트그레스)
- 의존성 추가
    ```
    <dependency>
        <groupId>org.postgresql</groupId>
        <artifactId>postgresql</artifactId>
    </dependency>
    ```
    
- PostgreSQL 설치 및 서버 실행 (docker)
    ```
    docker run -p 5432:5432 -e POSTGRES_PASSWORD=pass -e POSTGRES_USER=bong 
    -e POSTGRES_DB=springboot --name postgres_boot -d postgres
    ```
    - mysql과 같은 방식으로 진행한다.

- PostgreSQL bash로 실행
```
docker start -i -t postgres_boot bash
```

- PostgreSQL 접속
```
/*유저를 포스트그레스 바꾼다.*/
su - postgres

/*DB접속*/
psql sprinfboot

/*전체 데이터베이스 조회*/
\l or \list

/*전체 테이블 조회*/
\dt
```
    - DB접속이 안될 때 (role에러)
    ![springboot](/images/springboot/springboot13-9.png)
    
    ```
    psql -U <username> <DBname>
    ```
    ex) psql -U bong springpostgres
        - 맥북도 발생한다.. os문제는 아닌것 같음.

- PostgreSQL용 Datasource 설정
    - spring.datasource.url=jdbc:postgresql://localhost:5432/springboot
    - spring.datasource.username=bong
    - spring.datasource.password=pass

- Postgres 주의사항
    - user명칭을 사용할 수 없다.
    - user가 키워드로 사용된다.
    - org.postgresql.jdbc.PgConnection.createClob() is not yet implemented 경고
        - spring.jpa.properties.hibernate.jdbc.lob.non_contextual_creation=true
        프로퍼티에 추

- 인텔리제이 DataBase기능 활용
    - docker가 실행되고 있는 상태에서 진행
    - 우측의 DataBase 탭에서 progreSQL을 추가한다.
    ![springboot](/images/springboot/springboot13-10.png)
    - DB이름 , user, password 설정
    ![springboot](/images/springboot/springboot13-11.png)
    - 테이블 더블클릭으로 내용 확인 가능하며 쿼리문도 작성하여 확인할 수 있다.
    ![springboot](/images/springboot/springboot13-12.png)
    
#### 스프링 데이터 JPA
- ORM(Object-Relational Mapping)과 JPA (Java Persistence API)
    - 객체와 릴레이션을 맵핑할 때 발생하는 개념적 불일치를 해결하는 프레임워크
    - http://hibernate.org/orm/what-is-an-orm/
    - JPA: ORM을 위한 자바 (EE) 표준

- 스프링 데이터 JPA
    - JPA 표준스펙을 아주 쉽게 사용할 수 있게 추상화 시켜놓은것.
    - 구현체는 hibernate를 사용
    - Repository 빈 자동 생성
    - 쿼리 메소드 자동 구현
    - @EnableJpaRepositories (스프링 부트가 자동으로 설정 해줌.)
    - SDJ -> JPA -> Hibernate -> Datasource
    - 기존 jdbc를 모두 활용할 수 있다. (의존성을 보면 확인이 가능하다.)
    ![springboot](/images/springboot/springboot13-13.png)

#### JPA 연동
- 스프링 데이터 JPA 의존성 추가
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```
- 스프링 데이터 JPA 사용하기
    - @Entity 클래스 만들기
    ![springboot](/images/springboot/springboot13-14.png)
        - @GernerateValue 자동으로 값을 생성
        - lombok을 사용하면 보다 쉽게 만들 수 있다.
        ![springboot](/images/springboot/springboot13-15.png)
        >**Lombok**
        자바에서 Model Object를 만들 때, getter/setter, Tostring등 반본적으로 사용되는 코드를 어노테이션을 통해 줄여주는 라이브러리
        참고 블로그 : [갓대희의 작은공간](https://goddaehee.tistory.com/95)
        
    - Repository 만들기
    ![springboot](/images/springboot/springboot13-15.png)
        - Interface
        - extends JpaRepository<entity type , id type>
    
- 스프링 데이터 리파지토리 테스트 만들기
    - 슬라이스 테스트를 할때는 인메모리 데이터베이가 반드시 필요하다.
        - H2 DB를 테스트 의존성에 추가하기
        ```
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>test</scope>
        </dependency>
        ```
    - @DataJpaTest (슬라이스 테스트) 작성
        - 레퍼지토리를 포함한 레퍼지토리와 관련된 빈들만 등록을 하여 테스트
        ![springboot](/images/springboot/springboot13-17.png)    
            - DataSource
            - JdbcTemplate
            - 해당 Repository
            - ...
        - 테스트 결과
        ![springboot](/images/springboot/springboot13-18.png)

- 어플리케이션 정상 작동을 위해서는 DataBase설정이 필요하다.
    - DB 드라이버 의존성 추가(PostgreSQL)
    - DataSource 설정
    - SpringBootTest 적용 시, 모든 빈들이 등록됨으로 프로퍼티값들도 등록된다.
    (즉, postgreSQL이 적용된다.)
    ![springboot](/images/springboot/springboot13-19.png)
        - 테스트 DB가 하나 필요하게 됨으로 추천하지 않는다.

- JPA 활용
    - 기본 JPA api
    ![springboot](/images/springboot/springboot13-20.png)
    - Repostiry에 원하는 메소드 추가 후 활용
    ![springboot](/images/springboot/springboot13-22.png)
    - 직접 SQL쿼리문을 사용하고 싶을 때
    ![springboot](/images/springboot/springboot13-23.png)
    - Optional 사용
    ![springboot](/images/springboot/springboot13-24.png)
    


---
title: 스프링 데이터 JPA - 스프링 데이터 Common
date: 2019-05-03 18:05:54
tags: JPA
---
![springf](/images/jpa_logo.png)
# 스프링 데이터 JPA(inflearn) - 백기선 
## 스프링 데이터 JPA 활용

### 스프링 데이터 JPA 활용 파트 소개
![springjpa](/images/jpa/jpa05-1.png)
- 스프링 데이터 : 하나의 프로젝트를 말하는것이 아니고, 여러개의 프로젝트들의 묶음을 말하는것.
(SQL & NoSQL 저장소 지원 프로젝트의 묶음.)
- 스프링 데이터 Common : 여러 저장소 지원 프로젝트의 공통 기능 제공.
- 스프링 데이터 REST : 저장소의 데이터를 하이퍼미디어 기반 HTTP 리소스로(REST API로) 제공하는 프로젝트.
- 스프링 데이터 JPA : 스프링 데이터 Common이 제공하는 기능에 JPA 관련 기능 추가
- [참고문서](https://spring.io/projects/spring-data)

### 스프링 데이터 Common: Repository
- 앞서 JpaRepository를 상속하는 클래스를 만들어 사용한적이 있었는데.. 사실 해당 부분은 JpaRepository가 스프링데이터 Common의 PagingAndSortingRepostiory를 상속받고 있다.
    이미지
    - PagingAndSortingRepostiory : paging, sorting 메소드를 지원
    - CrudRepository : CRUD 메소드를 지원
        - @NoRepositoryBean가 붙어있는데.. 실제 빈을 만들어 등록하지 않도록한다.
        (중간단계의 Repository에는 붙어있다. 실제 Repository가 아님을 표현한다.)
    - Repository : marker 인터페이스, 실질적인 기능을 하진 않는다.
    
#### CrudRepository 메소드
- save(entity); 
    - 하나의 엔티티를 저장하고 그 엔티티를 리턴해준다.
- saveall(Iterable<> entities);
    - 여러개의 엔티티를 저장하고 리턴(Iterable타입으로..)
- findById(id);
    - id로 엔티티를 찾아서 리턴한다.
    - Optional<>로 리턴된다.(null방지..)
- existById(id);
    - 해당 id에 해당하는 엔티티의 유무를 확인한다.
- findAll();
    - 어떤 한 엔티티의 모든 정보를 불러온다.
    - 거의 테스트용으로 사용된다.
    - Iterable로 리턴한다. (List<>로 리턴하는경우는 스프링 데이터 JPA를 사용하는것)
- findAllById(Iterable<> ids);
    - 여러id에 해당하는 목록을 가져온다.
- count();
    - 갯수
- delete(id);
    - 삭제
    - deleteAll() 도 있다.
    
##### test
- 기본적으로 데이터 테스트들은 @Transactional을 가지고 있는데.. 기본적으로 롤백을 한다.
    - 어자피 롤백할 쿼리이기 때문에 test클래스 실행 시, insert쿼리문은 날라가지 않는다.
    - 보고 싶다면 @Rollback(false)를 추가하자.
    
- CRUD
    ![springjpa](/images/jpa/jpa05-2.png)

- PagingAndSorting
    ![springjpa](/images/jpa/jpa05-3.png)
<br>

### 스프링 데이터 Common: Repository 인터페이스 정의하기
#### Repository 인터페이스로 공개할 메소드를 직접 일일히 정의하고 싶을 때
- 특정 레퍼지토리 정의
    - @RepositoryDefinition
    ![springjpa](/images/jpa/jpa05-4.png)
    - test(정의한 Repository인터페이스는 모두 테스트를 해주어야한다.)
    ![springjpa](/images/jpa/jpa05-5.png)
- 공통 인터페이스 정의
    - @NoRepositoryBean
    ![springjpa](/images/jpa/jpa05-6.png)
    - 사용할 인터페이스에서 상속
    ![springjpa](/images/jpa/jpa05-7.png)
<br>

### 스프링 데이터 Common: Null 처리하기
#### Optional
- 자바8 부터 지원
- Null을 리턴하지 않고, 비어있는 콜렉션을 리턴한다.
- 예시
    ![springjpa](/images/jpa/jpa05-8.png)
- Optional 메소드 예시
    ![springjpa](/images/jpa/jpa05-9.png)

#### Null 어노테이션
- 스프링 프레임워크 5.0부터 지원하는 Null 애노테이션 지원.
    - @NonNullApi
        ![springjpa](/images/jpa/jpa05-10.png)
        - package-info.java파일 생성 후, 어노테이션 사용
        - 패키지안의 모든 파라미터 및 리턴타입 및 모두 Null이 아니어야한다.
        - Null이 필요한곳에 @Nullable을 사용 해야한다.
    - @NonNull
        ![springjpa](/images/jpa/jpa05-11.png)
        - Null이 아니길 바랄때 사용한다.
    - @Nullable.
- 툴의 도움을 받을 수 있다.
    ![springjpa](/images/jpa/jpa05-12.png)![springjpa](/images/jpa/jpa05-13.png)
    - NonNullApi는 불가능
<br>

### 스프링 데이터 Common: 쿼리 만들기 개요
#### 스프링 데이터 저장소의 메소드 이름으로 쿼리 만드는 방법
- 메소드 이름을 분석해서 쿼리 만들기 (CREATE)
    ![springjpa](/images/jpa/jpa05-14.png)
- 미리 정의해 둔 쿼리 찾아 사용하기 (USE_DECLARED_QUERY)
    - 우선순위 Query > Procedure > NamedQuery
    - 기본적으로 JPQL을 써야하나, SQL을 쓰고 싶다면 nativequery를 true로 변경한다.
    ![springjpa](/images/jpa/jpa05-16.png)
- **미리 정의한 쿼리 찾아보고 없으면 만들기 (CREATE_IF_NOT_FOUND) (기본전략)**
    - @EnableJpaRepositories(queryLookupStrategy = )로 변경 가능하다.
        ![springjpa](/images/jpa/jpa05-15.png)

#### 쿼리 만드는 방법
- 리턴타입 {접두어}{도입부}By{프로퍼티 표현식}(조건식)\[(And|Or){프로퍼티표현식}(조건식)]{정렬 조건} (매개변수)
  - 접두어 : Find, Get, Query, Count, ...
  - 도입부 : Distinct, First(N), Top(N)
  - 프로퍼티 표현식 : Person.Address.ZipCode => find(Person)ByAddress_ZipCode(...)
  - 조건식 : IgnoreCase, Between, LessThan, GreaterThan, Like, Contains, ...
  - 정렬 조건 : OrderBy{프로퍼티}Asc|Desc
  - 리턴 타입 : E, Optional<E>, List<E>, Page<E>, Slice<E>, Stream<E>
  - 매개변수 : Pageable, Sort
<br>

### 스프링 데이터 Common: 쿼리 만들기 실습
#### 기본예제
```
List<Person> findByEmailAddressAndLastname(EmailAddress emailAddress, String lastname);
```

##### distinct
```
List<Person> findDistinctPeopleByLastnameOrFirstname(String lastname, String firstname);
List<Person> findPeopleDistinctByLastnameOrFirstname(String lastname, String firstname);
```

##### ignoring case
```
List<Person> findByLastnameIgnoreCase(String lastname);
List<Person> findByLastnameAndFirstnameAllIgnoreCase(String lastname, String firstname);
```

#### 정렬
```
List<Person> findByLastnameOrderByFirstnameAsc(String lastname);
List<Person> findByLastnameOrderByFirstnameDesc(String lastname);
```

#### 페이징
```
Page<User> findByLastname(String lastname, Pageable pageable);
Slice<User> findByLastname(String lastname, Pageable pageable);
List<User> findByLastname(String lastname, Sort sort);
List<User> findByLastname(String lastname, Pageable pageable);
```

#### 스트리밍
- try-with-resource 사용할 것. (Stream을 다 쓴다음에 close() 해야 함)
```
Stream<User> readAllByFirstnameNotNull();
```

#### 그 외 실습내용
[Repository class](https://github.com/cyr9210/springJPA-study/blob/master/03/src/main/java/me/bong/springjpa03/post/CommentRepository.java)
[test class](https://github.com/cyr9210/springJPA-study/blob/master/03/src/test/java/me/bong/springjpa03/post/CommnetRepositoryTest.java)
<br><br>

### 스프링 데이터 Common: 비동기 쿼리
#### Future
```
@Async
Future<User> findByFirstname(String firstname);
```
- Java5에서 생김
- @EnableAsync 해주어야한다.

#### CompletableFuture
```
@Async 
CompletableFuture<User> findOneByFirstname(String firstname);
```
- Java8에서 들어옴 
- @EnableAsync 해주어야한다.

#### ListenableFuture
```
@Async 
ListenableFuture<User> findOneByLastname(String lastname);
```
- 스프링에서 만들었다.
- 가장 깔끔함.
- 해당 메소드를 스프링 TaskExecutor에 전달해서 별도의 쓰레드에서 실행함.
- Reactive랑은 다른 것임

#### 권장하지 않는 이유
- 테스트 코드 작성이 어려움.
- 코드 복잡도 증가.
- 성능상 이득이 없음.
    - DB 부하는 결국 같고.
    - 메인 쓰레드 대신 백드라운드 쓰레드가 일하는 정도의 차이.
    - 단, 백그라운드로 실행하고 결과를 받을 필요가 없는 작업이라면 @Async를 사용해서 응답 속도를 향상 시킬 수는 있다.
<br><br>

### 스프링 데이터 Common: 커스텀 리포지토리
- 쿼리 메소드(쿼리 생성과 쿼리 찾아쓰기)로 해결이 되지 않는 경우 직접 코딩으로 구현 가능.
    - 스프링 데이터 리포지토리 인터페이스에 기능 추가.
    - 스프링 데이터 리포지토리 기본 기능 덮어쓰기 가능.
    
#### 구현방법
1. 커스텀 리포지토리 인터페이스 정의
![springjpa](/images/jpa/jpa05-17.png)
1. 인터페이스 구현 클래스 만들기 (기본 접미어는 Impl)
![springjpa](/images/jpa/jpa05-18.png)
    - EntityManager, JdbcTemplate 등을 주입받아 사용할 수 있다.
1. 엔티티 리포지토리에 커스텀 리포지토리 인터페이스 추가
![springjpa](/images/jpa/jpa05-19.png)

- 기본 기능을 덮어쓰고 싶다면 기본기능 메소드를 그대로 정의하고 위와 같은 방법으로 구현한다.
![springjpa](/images/jpa/jpa05-20.png)

#### 접미어 설정하기
- @EnableJpaRepositories(repositoryImplementationPostfix = "변경값")
    ![springjpa](/images/jpa/jpa05-21.png)    
    - 기본값은 "Impl" 이다.
<br><br>

### 스프링 데이터 Common: 기본 리포지토리 커스터마이징
- 모든 리포지토리에 공통적으로 추가하고 싶은 기능이 있거나 덮어쓰고 싶은 기본 기능이 있을때

#### 구현방법
1. JpaRepository를 상속 받는 인터페이스 정의
    ![springjpa](/images/jpa/jpa05-22.png)
    - @NoRepositoryBean
2. 기본 구현체를 상속 받는 커스텀 구현체 만들기
    ![springjpa](/images/jpa/jpa05-23.png)
3. @EnableJpaRepositories에 설정
    - repositoryBaseClass
    ![springjpa](/images/jpa/jpa05-25.png)
4. 엔티티 레포지토리에 설정 
    ![springjpa](/images/jpa/jpa05-24.png)
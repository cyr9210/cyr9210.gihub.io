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



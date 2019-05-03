---
title: 스프링 데이터 JPA - Cascade
date: 2019-04-30 20:12:00
tags: JPA
---
![springf](/images/jpa_logo.png)
# 스프링 데이터 JPA(inflearn) - 백기선 
## JPA 프로그래밍
### 엔티티의 상태와 Cascade

#### 엔티티의 상태
![springjpa](/images/jpa/jpa04-1.png)
- Transient: JPA가 모르는 상태
    - ex)  new 사용하여 객체생성 상태
- Persistent: JPA가 관리중인 상태 (1차 캐시, Dirty Checking(변경사항을 계속체크), Write Behind, ...)
    - ex)  session.save()를 통해 저장한 상태
    - 1차 캐시(persistent context(session, entitiymanager등)에 저장된다.)
        ![springjpa](/images/jpa/jpa04-2.png)
        - 바로 쿼리문을 날리지 않고 1차캐싱 후, 트랜잭션 종료 시 객체를 체크하여 쿼리문을 날린다.
        (불필요한 쿼리문 날리지 않는다.)
- Detached: JPA가 더이상 관리하지 않는 상태.
    - session이 끝난상태
- Removed: JPA가 관리하긴 하지만 삭제하기로 한 상태.

#### Cascade
- 엔티티의 상태 변화를 전파 시키는 옵션
- 참조하고 있던 객체들도 함께 변화한다.(설정을 해주어야한다.)
    ![springjpa](/images/jpa/jpa04-3.png)
    - remove 옵션을 주면 함께 사라진다.
    ![springjpa](/images/jpa/jpa04-4.png)
    





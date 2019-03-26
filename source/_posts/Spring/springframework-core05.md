---
title: 스프링 AOP
date: 2019-03-26 16:23:40
tags: Springframework
---
![springf](/images/springframwork-logo.png)
# 스프링 프레임워크 핵심기술05(inflearn) - 백기선 
## Springframework

### AOP 개념
- Aspect-oriendted Programming (AOP)은 OOP를 보완하는 수단으로, 흩어진 Aspect를 모듈화 할 수 있는 프로그래밍 기법.

img

#### 주요개념
- Aspect : 분류된 모듈
- Target : 대상 
ex\) class A, B, C
- Advice : 해야할 일
- Pointcut : 어디에 적용해야 하는지 정보를 가지고 있다.
- Join Point : 메소드 실행 시점 (**끼어들 수 있는 시점**)

0730
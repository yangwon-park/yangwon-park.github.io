---
layout: single
title: Spring 핵심 원리 (Basic) - 02. IoC, Framework, DI, Container
categories: spring
tag: [java, spring, web]
toc: true 
author_profile: false
sidebar:
    nav: "docs"
---

<br/>

# IoC (Inversion of Control)

- 제어의 역전
- 프로그램의 제어 흐름을 구현 객체가 직접 제어하는 것이 아니라 외부에서 관리하는 것을 의미



## 프레임워크 VS 라이브러리

### 프레임워크

- 제어의 역전이 잘 적용된 예시
- 설계와 구현을 재사용이 가능하게끔 일련의 협업화된 형태로 클래스들을 제공하는 것
- 스스로 흐름을 가지고 있어 개발자의 코드 사용 공간을 강제함
- 체계적인 관리가 가능하나, 많은 학습이 필요하고 유연한 개발이 불가능함

### 라이브러리

- 개발을 위해 필요한 것들을 미리 구현해놓은 도구
- 프레임워크라는 틀 안에 있는 재사용이 가능한 도구

<br>

# DI (Dependency Injection)

- 의존 관계 주입
- 실행 시점(런타임)에 외부에서 실제 구현 객체를 생성하고 클라이언트에 전달하여 클라이언트와 실제 의존 관계가 연결되는 것
- 객체 인스턴스를 생성하고 그 참조값을 전달하여 연결됨
- 클라이언트 코드를 변경하지 않고, 클라이언트 호출 대상의 타입 인스턴스를 변경 가능
- **정적 클래스 의존 관계를 변경하지 않고, 동적인 객체 인스턴스 의존 관계를 쉽게 변경할 수 있음**

## 정적 클래스 의존 관계

- 클래스가 사용하는 import 코드만을 보고 의존 관계 파악 가능
- 즉, 실행하지 않아도 의존 관계 자체를 눈으로 파악 가능

## 동적 객체(인스턴스) 의존 관계

- 애플리케이션 실행 시점에 의존 관계가 결정됨
- 실행 시점에 실제 생성된 객체 인스턴스의 참조가 연결되는 의존 관계

<br>



# DI 컨테이너

- **객체를 생성하고 관리하면서 의존 관계를 연결해주는 역할**
- IoC 컨테이너라고도 불렀으나, 의존 관계 주입에 초점을 맞춰 현재 DI 컨테이너로 주로 불림





<div class='notice--warning'>
    <br/>
    인프런 - 김영한님의 Spring 핵심 원리 기본편을 정리한 내용입니다. <br/><br/>
    <a href="https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8/dashboard" class="btn btn--info">김영한님 인프런 강의</a><br/>
    <br/>
</div>

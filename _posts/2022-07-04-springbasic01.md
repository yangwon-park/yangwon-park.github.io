---
layout: single
title: Spring 핵심 원리 (Basic) - 01. 객치 지향 설계의 5가지 원칙
categories: spring
tag: [java, spring, web]
toc: true 
author_profile: false
sidebar:
    nav: "docs"
---

<br/>

# SOLID

- 로버트 마틴이 정리한 좋은 객체 지향 설계를 위한 5가지 원칙

<br>

## SRP (Single Responsibility Principle)

- 단일 책임 원칙
- 하나의 클래스는 하나의 책임만을 가져야 한다
- 즉 변경이 있을 시, **전체 코드에 파급 효과가 적어야 한다는 의미**
- ex) 객체의 생성과 사용을 분리함

## OCP (Open/Closed principle)

- 개방 - 폐쇄 원칙
- 확장에는 열려있으나 변경에는 닫혀 있어야 함
- 인터페이스로 역할, 이를 구현한 클래스를 통해 **역할과 구현의 분리를 이룸**
- OCP를 완전히 지키려면 별도의 조립자, 구성 영역을 담당해주는 설정자가 필요함 (AppConfig)

## LSP (Liskov Substitution Principle)

- 리스코프 치환 원칙

- 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 함
- 즉, 하위 클래스가 인터페이스의 규약을 다 지켜야 한다는 의미
- ex) 앞으로 가는 기능은 죽이 되든 밥이 되든 앞으로 가야하지 뒤로 가면 안 된다

## ISP (Interface Segregation Principle)

- 인터페이스 분리 원칙
- 특정 클라이언트를 위한 인터페이스 다수가 범용 인터페이스 하나보다 나음
- 인터페이스가 명확해지며, 대체 가능성이 높아짐

## DIP (Dependency inversion principle)

- 의존 관계 역전 원칙
- 구현 클래스에 의존하지 말고, 인터페이스에 의존하라!
- 역할에 의존하라!
- 구현에 의존하게 되면 변경이 어려움

<br>



<div class='notice--warning'>
    <br/>
    인프런 - 김영한님의 Spring 핵심 원리 기본편을 정리한 내용입니다. <br/><br/>
    <a href="https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8/dashboard" class="btn btn--info">김영한님 인프런 강의</a><br/>
    <br/>
</div>

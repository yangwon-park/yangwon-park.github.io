---
layout: single
title: Spring 핵심 원리 (Basic) - 04. 다양한 의존 관계 주입
categories: spring
tag: [java, spring, web]
toc: true 
author_profile: false
sidebar:
    nav: "docs"
---

<br/>

# 다양한 의존 관계 주입

## 수정자 주입 (Setter)

- 자바빈 프로터티 규약의 setter 메소드에 @Autowired를 사용
- 테스트 코드에서 의존 관계를 한 눈에 파악하기 힘듬

## 필드 주입

- 필드에 바로 @Autowired 사용
- 간결하다는 큰 장점이 있으나, 변경이 불가능한 단점이 있어 사용하지 않음
- DI 프레임 워크가 없으면 할 수 있는게 없음

## 일반 메소드 주입

- 메소드를 하나 만들고, 그 안에서 주입을 받음
- 사용 X

## 생성자 주입

- **Best Practice (얘를 사용하자)** 
- 생성자에 @Autowired를 붙여 의존 관계를 주입
- 필드에 final 키워드를 사용할 수 있음 => 필드의 값을 누락하지 않음
- 객체를 생성할 때, 데이터 누락을 방지할 수 있음
- 대부분의 의존 관계는 불변해야 하는데, 이를 유지할 수 있음

<br/>

<div class='notice--warning'>
    <br/>
    인프런 - 김영한님의 Spring 핵심 원리 기본편을 정리한 내용입니다. <br/><br/>
    <a href="https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8/dashboard" class="btn btn--info">김영한님 인프런 강의</a><br/>
    <br/>
</div>

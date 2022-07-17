---
layout: single
title: Spring 핵심 원리 (Basic) - 03. 싱글톤
categories: spring
tag: [java, spring, web]
toc: true 
author_profile: false
sidebar:
    nav: "docs"
---

<br/>

# 싱글톤

## 싱글톤 패턴

- 클래스의 인스턴스가 오직 **1개**만 생성되는 것을 보장하는 디자인 패턴

- private 생성자를 이용하여 외부에서 임의로 new 키워드를 사용하지 못하게 해야 함

- ```java
  public class SingletonService {
      
      // static 영역에 객체를 하나만 생성
      private static final SingletonService instance = new SingletonService();
      
      // 이 객체의 인스턴스가 필요하면 아래의 static 메소드를 통해서만 조회 가능
      public static SingletonService getInstance() {
          return instance;
      }
      
      // private으로 생성자 만듬 => 외부에서 생성 불가!!
      private SingletonService() {
          
      }
      
      public void logic() {
          System.out.println("싱글톤 객체 호출");
      }
  }
  ```

- getInstance() 메소드를 통해서만 조회 가능하므로 항상 같은 인스턴스를 반환해줌

## 싱글톤 패턴의 문제점

- 아래의 이유들로 인하여 유연성이 떨어져 **안티 패턴**으로 불리기도 함

1. 구현 코드가 많이 필요
2. 클라이언트가 구체 클래스에 의존함 (구현체.getInstacne 사용 -> DIP 위반)
3. OCP 또한 위반할 가능성이 높음
4. 테스트가 어려움
5. 내부 속성을 변경 또는 초기화 하기 어려움
6. private 생성자로 자식 클래스를 만들기 어려움

<br/>

## 스프링 컨테이너로 해결

- 스프링 컨테이너는 싱글톤 컨테이너 역할을 함
- 위에서 언급한 모든 문제점을 해결하고 싱글톤으로 객체(스프링 빈)를 관리해줌
- 싱글톤 레지스트리
  - 싱글톤 객체를 생성하고 관리하는 기능

## 싱글톤 방식의 주의점!!!

- 스프링 컨테이너를 쓰든 말든 싱글톤 패턴은 객체 인스턴스를 하나만 생성하여 공유함
- 따라서 객체의 상태를 절대 유지하게 설계해선 안되고 항상 **무상태(stateless)**로 설계해야 함
- 무상태 설계
  - 특정 클라이언트에 의존적인 필드가 없어야 함
  - 특정 클라이언트가 값을 변경할 수 있는 필드를 가져선 안됨
  - 가급적이면 **읽기만 가능** (수정 X)
  - 필드가 아닌 **지역변수, 파라미터, ThreadLocal** 등을 사용해야 함

### 예시

- ```java
  public class StatefulService {
  
      private int price;
  
      public void order(String name, int price) {
          System.out.println("name = " + name);
          System.out.println("price = " + price);
          this.price = price; // 여기가 문제!!!
      }
  
      public int getPrice() {
          return price;
      }
  }
  ```

- ```java
  class StatefulServiceTest {
  
      @Test
      void statefulServiceSingleton() {
          AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(TestConfig.class);
          StatefulService statefulService1 = ac.getBean(StatefulService.class);
          StatefulService statefulService2 = ac.getBean(StatefulService.class);
  
          // ThreadA : A 사용자 10000원 주문
          statefulService1.order("userA", 10000);
  
          // ThreadB : B 사용자 20000원 주문
          statefulService2.order("userB", 20000);
  
          // ThreadA : A 사용자 주문 금액 조회
          int price = statefulService1.getPrice();
          System.out.println("price = " + price);
  
          // 기대한 값은 1만원 But 실제 값은 2만원이 나옴
          //  => 스프링 컨테이너는 하나의 인스턴스를 공유하는 싱글톤 패턴임
          //  => servic1, servic2 모두 하나의 인스턴스이므로 뒤에 넣은 2만원으로 덮어씌워짐
          Assertions.assertThat(statefulService1.getPrice()).isEqualTo(20000);
      }
          static class TestConfig {
  
              @Bean
              public StatefulService statefulService() {
                  return new StatefulService();
              }
          }
  
  }
  ```

- 위의 경우 공유되는 price필드를 특정 클라이언트가 값을 변경할 수 있게 설계되어 있음

- 따라서 userA의 주문 금액을 1만원으로 기대하였으나 실제로 2만원이 조회됨

### 수정

- Service의 order 메소드가 price값을 반환하게 만듬
- 반환된 값을 지역변수로 생성하여 값을 담아주면 별도의 변수가 2개 생성되므로 위의 문제를 해결할 수 있음

- ```java
  public class StatefulService {
  
      private int price;
  
      // int를 반환하게 만듬
      public int order(String name, int price) {
          System.out.println("name = " + name);
          System.out.println("price = " + price);
          this.price = price; // 여기가 문제!!!
  
          return price; // 아예 price값을 반환해주면 문제 해결!!!
      }
  }
  ```

## Spring에서 Appconfig의 싱글톤 보장

- ```java
  @Test
  void configurationDeep() {
      AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
      AppConfig bean = ac.getBean(AppConfig.class);
  
      // CGLIB으로 나옴
      // 바이트코드 조작 라이브러리를 사용하여 AppConfig 클래스를 상속받은 임의의 클래스를 생성
      // 위의 클래스를 스프링 빈에 등록해줌 => 이 친구가 싱글톤을 보장해줌
      System.out.println("bean = " + bean.getClass());
  }
  
  // bean = class com.example.basicreview.AppConfig$$EnhancerBySpringCGLIB$$8743a0
  ```

- @Configuration이 붙어있는 Appconfig의 경우 CGLIB이라는 라이브러리를 이용하여 싱글톤 보장

- @Configuration이 없는 경우, 스프링 빈으로는 등록이 되지만 싱글톤 패턴은 보장해주지 않음

<div class='notice--warning'>
    <br/>
    인프런 - 김영한님의 Spring 핵심 원리 기본편을 정리한 내용입니다. <br/><br/>
    <a href="https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8/dashboard" class="btn btn--info">김영한님 인프런 강의</a><br/>
    <br/>
</div>

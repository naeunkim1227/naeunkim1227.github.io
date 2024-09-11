---
layout: post
title: spring IoC Container
subtitle: 
categories: spring
tags: [spring, Interceptor]
---


# 01.IoC 와 DI

Inversion of Control( 제어역전 ) 이란 프로그램 코드 내에서 참조하려는 객체를 직접 생성 하지 않고 외부의 다른 존재가 생성하여 제공한다는 개념

종류

1. org.springframework.beans.factory.BeanFactory 빈(Bean) 팩토리 인터페이스

2.org.springframework.context.ApplicationContext 애플리케이션 컨텍스트 인터페이스'

2 개의 인터페이스를 구현한 다수의 Container 클래스가 존재한다.

XmlBeanFactory, ClassPathXmlApplicationContext, FileSystemXmlApplication,

XmlWebApplicationContext (JEE 환경에서 설정이 가능)

# 02.Spring Container

컨테이너의 필요성

1.  컴포넌트, 객체의 자유로운 삽입이 가능하도록 하기 위한 호출의 독립성
2. 서비스를 설정하거나 찾기 위한 일관된 방법을 제시
3. 개발자가 직접 싱글톤이나 팩토리를 구현할 필요 없이 단일화된 서비스의 접근방법을 제공
4. 비즈니스 객체에 부가적으로 필요한 각종 엔터프라이즈 서비스를 제공

### 1)Bean Factory (XMLBeanFactory) 빈 팩토리 인터페이스

- 기본적인 Bean의 생성과 소멸을 담당한다.

- XML 파일에 기술되어 있는 정의를 바탕으로  Bean을 생성한다.

- 생성자에  xml 파일 로딩에 필요한  Reasorce 객체를 넘겨 주어야 한다.

- FileSystemResource 와 ClassPathResource 를 주로 사용한다.

( ByteArrayResource, DescriptiveResource, InputStreamResource, 등…  )

```java
BeanFactory beanFactory = 
	new XmlBeanFactory( new ClassPathResource( "config/applicationContext.xml" ) );

```

### 2) ApplicationContext 애플리케이션 컨텍스트 인터페이스

1. BeanFactory는 기초적인 컨테이너 기능을 제공하지만  Spring Framework의 완벽한 기능을 사용하기 위해서는 ApplicationContext를 사용해야  한다.
2. BeanFactory와 유사하지만  Bean을 로딩하는 방법에  차이가 있다.
3. 리스너로 등록된 빈에 이벤트를 발생 시킬 수 있다.
4. 추가적인 기능과 성능 차이로 ApplicationContext를 Spring Container를 사용한다.
5. 종류
    
    ClassPathXmlApplicationContext
    
    XmlWebApplicationContext
    
    FileSystemXmlApplicationContext
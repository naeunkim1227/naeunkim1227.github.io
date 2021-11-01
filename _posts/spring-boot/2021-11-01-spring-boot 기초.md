---
layout: post
title: spring-boot 설정
subtitle: 
categories: spring-boot
tags: [spring-boot]
---



### 스프링 부트와 스프링 프레임 워크의 차이점

단독 스프링 부트 애플리케이션 :(spring-boot-start-parent 를 부모로 사용해서 의존성 버전관리) : 일반적

Maven Project의 모듈로 스프링 부트 애플리케이션 : spring-boot-start-parent를 부모로 사용X, 버젼관리, 레포지토리 따로 지정하는 방식 

```xml
<!-- jar로 빌드하기 위함 -->
	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
```

위의 코드로 다음과 같이 변환시켜준다. uber.jar를 만들어 준다. 

java -cp mmmm.jar Main

uber jar

java -jar mmmm.jar



# 01. 단독 스프링 부트 애플리케이션 생성

maven 프로젝트 생성 시 jar형식으로 생성한다

## 1.pom설정

pom.xml 형식가져오기

[Spring Initializr](https://start.spring.io/)

![initial](https://user-images.githubusercontent.com/83413364/139650928-2c94e55f-b389-4722-8d57-780289e50e98.png)


generate로 xml파일을 생성해 사용한다.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	
	<!-- 부모스타터 : 의존성 버전관리 -->	
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.5.6</version>
		<!-- 레포지토리에서 부모를 검색한다. -->
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	
	<groupId>com.douzone</groupId>
	<artifactId>springboot-ex</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	
	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
	</properties>
	
	<dependencies>
		<!-- spring boot basic dependency -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter</artifactId>
		</dependency>
		
		<!-- spring boot test -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>
	
	<!-- uber.jar로 빌드하기 위함,스프링 부트 메이븐 플러그인  -->
	<build>
		<finalName>springboot-ex</finalName>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
	
</project>
```

## 2.빌드하기

![build](https://user-images.githubusercontent.com/83413364/139650984-1dabed52-3e73-4169-985c-9bce11121f9e.png)


![build2](https://user-images.githubusercontent.com/83413364/139651014-d4fcbbaf-8e70-4c0a-832f-2810c073c346.png)


goals : clean package 치고 run

![cmd](https://user-images.githubusercontent.com/83413364/139651054-290c1682-e9d4-4589-8c35-eca463abbbad.png)


goals 설정을 아래와 같이 지정해서 run as를 하면 따로 실행하지 않아도 빌드하고서 jar를 실행한다.

clean package spring-boot:run 

# 02.maven project > maven module 있는 프로젝트

## pom설정

**상위 프로젝트 pom.xml**

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.douzone</groupId>
  <artifactId>springboot-practices</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>pom</packaging>
  
  <properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
</properties>	
  
  <modules>
  	<module>myweb</module>
  	<module>helloworld</module>
  	<module>myapplication</module>
  </modules>
</project>
```

**하위 모듈 pom.xml**

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>com.douzone</groupId>
		<artifactId>springboot-practices</artifactId>
		<version>0.0.1-SNAPSHOT</version>
	</parent>
	<artifactId>myapplication</artifactId>

	<properties>
		<maven.compiler.source>1.8</maven.compiler.source>
		<maven.compiler.target>1.8</maven.compiler.target>
	</properties>

	<dependencyManagement>
		<dependencies>
			<dependency>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-dependencies</artifactId>
				<version>2.5.6</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
		</dependencies>
	</dependencyManagement>

	<dependencies>
		<!-- spring boot basic dependency -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter</artifactId>
		</dependency>

		<!-- spring boot test -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<finalName>myapplication</finalName>
		<plugins>
			<!-- 스프링 부트 메이븐 플러그인 -->
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
</project>
```


# 3. Bootstrapping Class

1. **Bootstrapping Class**
- 스프링 부트 애플리케이션의 부트 스트래핑(Bootstrapping)
- 설정 클래스로 역할을 한다.(Configuration Class)

1. **run이 실행되면 하는 일**
    1.  애플리케이션 컨텍스트(Application Context, Spring Container) 생성
        1. Application Context 구성 후, 실행할 코드(Application Context 환경) 가 있는 경우, ApplicationRunner 인터페이스 구현 클래스 빈 생성하기
    2. 웹애플리케이션 타입(Wep Application인 경우, MVC or Reactive)
    3. 어노테이션 스캐닝(Auto) or Configuration.java 클래스(Explicit) 를 통한 빈생성 및 등록
    4. 웹 어플리케이션인 경우 (MVC)
     - 내장(emebedded) 서버(TomcatEmbeddedServletContainer) 인스턴스 생성
    - 서버 인스턴스에 웹 애플리케이션을 배포
    - 서버 인스턴스 실행
    5. 인터페이스 ApplicatinRunner 구현 빈의 run() 실행 > run이 오면 자동으로 실행된다. 

# 4.TEST

**@SpringBootConfiguration +@ComponentScan + @EnableAutoConfiguration = @SpringBootApplication** 

### 1. @Component를 작성한 MyComponent 클래스

```java
package ex05.component;

import org.springframework.stereotype.Component;

@Component
public class MyComponent {
	public void printHello() {
		System.out.println("Hello World");
	}
}
```

<project xmlns="[http://maven.apache.org/POM/4.0.0](http://maven.apache.org/POM/4.0.0)"
xmlns:xsi="[http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)"
xsi:schemaLocation="[http://maven.apache.org/POM/4.0.0](http://maven.apache.org/POM/4.0.0) [https://maven.apache.org/xsd/maven-4.0.0.xsd](https://maven.apache.org/xsd/maven-4.0.0.xsd)">
<modelVersion>4.0.0</modelVersion>
<parent>
<groupId>com.douzone</groupId>
<artifactId>springboot-practices</artifactId>
<version>0.0.1-SNAPSHOT</version>
</parent>
<artifactId>myapplication</artifactId>

```
<properties>
	<maven.compiler.source>1.8</maven.compiler.source>
	<maven.compiler.target>1.8</maven.compiler.target>
</properties>

<dependencyManagement>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-dependencies</artifactId>
			<version>2.5.6</version>
			<type>pom</type>
			<scope>import</scope>
		</dependency>
	</dependencies>
</dependencyManagement>

<dependencies>
	<!-- spring boot basic dependency -->
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter</artifactId>
	</dependency>

	<!-- spring boot test -->
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-test</artifactId>
		<scope>test</scope>
	</dependency>
</dependencies>

<build>
	<finalName>myapplication</finalName>
	<plugins>
		<!-- 스프링 부트 메이븐 플러그인 -->
		<plugin>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-maven-plugin</artifactId>
		</plugin>
	</plugins>
</build>

```

</project>

### 2. ApplicationRunner를 구현한 HelloWorldRunner 클래스

```java
package ex05.component;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.ApplicationArguments;
import org.springframework.boot.ApplicationRunner;

public class HelloWorldRunner implements ApplicationRunner {	
	@Autowired
	private MyComponent myComponent;
	
	@Override
	public void run(ApplicationArguments args) throws Exception {
		myComponent.printHello();
	}
}
```

### 3.@SpringBootApplication 을 작성한 bootstrapping 클래스

```java
package ex05;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.ApplicationArguments;
import org.springframework.boot.ApplicationRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.context.annotation.Bean;

import ex05.component.MyComponent;

/**
 * 
 * Application Context 구성 후, 실행할 코드(Application Context 환경) 가 있는 경우
 * ApplicationRunner 인터페이스 구현 클래스 빈 생성하기
 *
 */
@SpringBootApplication
public class MyApplication {
	
	@Bean
	public ApplicationRunner applicationRunner() {
		//1. 작성된 구현 클래스를 사용하는 방법
		// return new HelloWorldRunner();
		
		//2. Anonymouse Class 사용하는 방법
		return new ApplicationRunner() {
			@Autowired
			private MyComponent myComponent;
			
			@Override
			public void run(ApplicationArguments args) throws Exception {
				myComponent.printHello();
			}
		};
		
		//3. 함수형(람다 표현식)
//		return (args) -> {
//			System.out.println("Hello World");
//		};
		
	}
	
	public static void main(String[] args) {
		try(ConfigurableApplicationContext c = SpringApplication.run(MyApplication.class, args)) {
		}	
	}
}
```
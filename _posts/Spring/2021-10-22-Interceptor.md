---
layout: post
title: spring Interceptor
subtitle: 
categories: spring
tags: [spring, Interceptor]
---



# 01.인터셉터란

1. Spring에서 HTTP Request와  HTTP Response를  Controller 앞과 뒤에서 가로채는역할을 한다.
2. Servlet의 앞과 뒤에서 HTTP Request와  HTTP Response를 가로채는 필터와 유사하다.
3. Interceptor를 구현하기 위해서는 HandlerInterceptor 인터페이스를 구현하여야 한다
4. 인증과 관련된 부분을 인터셉터가 수행한다.
5. 보안관련 처리
6. 인증 관련이디 prehandler메소드를 자주 사용하게 된다.

## 구조

> -encoding Filter
> 

### tomcat

> — dispatcher servlet |
> 

> 
> 

> 
> 

> 
> 

interceptor가  가로채는 구간

### Web Application Context

> -controller - handler
> 

> 
> 

> 
> 

> 
> 

interceptor가  가로채는 구간

### Root Application Context

> -service
> 

> 
> 

> 
> 

> 
> 

### DB

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/42d1de0c-a2d1-4b22-904e-0b14ce14bfa7/Untitled.png)

필터와의 차이점

1. 호출 시점
    1. filter는 dispathcher servlet이 실행되기 전, interceptor는 실행 후
2. 설정위치
    1. filter는 web.xml, interceptor는 spring-servlet
3. 구현 방식
    1. interceptor는 클래스 구현이 필요하다. filter는 태그 삽입만해도 실행 가능
    

### interceptor클래스 구현 전, HandlerInterceptor에 대한 이해가 필요하다.

```java
package com.douzone.hellospring.interceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

public class MyInterceptor01 implements HandlerInterceptor {

	@Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws Exception {
		System.out.println("MyInterceptor01.preHandle() call");
		return true;
		//true로 하면 아래 메소드 모두실행
		//false인 경우 prehandle만 실행
	}

	@Override
	public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
			ModelAndView modelAndView) throws Exception {
		System.out.println("MyInterceptor01.postHandle() call");
	}

	@Override
	public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex)
			//뷰 리절브가 끝난 다음 한번더 실행한다.
			throws Exception {
		System.out.println("MyInterceptor01.afterCompletion() calll");

	}

}
```

**PreHandle**

컨트롤러로 가기전에 실행된다.

true일 경우 실행, false면 멈춘다.

Object handler는 핸들러 매핑이 찾은 컨트롤러 클래스 객체이다...

# 02.구현실습

## 1) Interceptor 구현하기

어노테이션 @Auth로 인터셉터 실행이 가능하다. (인증 받고 true 반환하면 메소드 실행하라)

### 1.spring-servlet.xml

인터셉터를 거칠, url 매핑해주기

```xml
<!-- Interceptors -->
	<mvc:interceptors>
		<mvc:interceptor>
			<mvc:mapping path="/user/auth" />
			<bean class="com.douzone.mysite.security.LoginInterceptor" />
		</mvc:interceptor>
		<mvc:interceptor>
			<mvc:mapping path="/user/logout" />
			<bean class="com.douzone.mysite.security.LogoutInterceptor" />
		</mvc:interceptor>
		<mvc:interceptor>
			<mvc:mapping path="/**" />
			<mvc:exclude-mapping path="/user/auth" />
			<mvc:exclude-mapping path="/user/logout" />
			<mvc:exclude-mapping path="/assets/**" />
			<bean class="com.douzone.mysite.security.AuthInterceptor" />
		</mvc:interceptor>
	</mvc:interceptors>
```

url에 /user/auth가 입력되면, 컨트롤러로 이동해 로직을 수행하기 전에

com.douzone.mysite.security.LoginInterceptor 클래스로 가게된다.

*<mvc:annotation-driven</mvc:annotation-driven>을 사용하게 되면 메서드 마다 매핑 명시 해주기.

안하면 메소드 이름으로 알아서 매핑하게 된다. 

<mvc:mapping path=""> 로 적힌 url로 들어가면

<bean class="">에 지정한 인터셉터를 수행하게 된다. 

<mvc:exclude-mapping path=""/>를 통해 인터셉터를 수행하지 않을 url을 지정할 수 있다.

### 2. com.douzone.mysite.security.LoginInterceptor 클래스

extends HandlerInterceptorAdapter

```java
package com.douzone.mysite.security;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.servlet.handler.HandlerInterceptorAdapter;

import com.douzone.mysite.service.UserService;
import com.douzone.mysite.vo.UserVo;

public class LoginInterceptor extends HandlerInterceptorAdapter{
	
	@Autowired
	UserService userService;
	
	@Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws Exception {
		String email = request.getParameter("email");
		String password = request.getParameter("password");

		//UserVo authUser =  new UserService().getuser(email, password);
		//new 로 선언해주면 안 된다. > new 로 선언해버리면 새로운 객체 생성 > 레포지토리가 null값이 떠서 , null예외 발생함
		//autowired를 통해 주입이 가능하다.
		System.out.println("LoginInterceptor 실행");
		UserVo authUser = userService.getUser(email, password);
		if(authUser == null) {
			request.setAttribute("result", "fail");
			request.getRequestDispatcher("/WEB-INF/views/user/login.jsp").
			forward(request, response);
			
			return false;
		}
		
		//session 처리
		HttpSession session = request.getSession(true);
		session.setAttribute("authUser", authUser);
		
		response.sendRedirect(request.getContextPath());
		return false;
	}

	
}
```

1. 파라미터로 email, password를 가져온다. DB에서 값을 확인하는 메소드를 실행한 후, authUser로 저장하는데, 값이 없을 경우 회원이 아닌 경우이므로, login페이지로 보내준다. return false로 이후의 코드가 실행 되지 않게 한다.
2. 세션 처리를 한다. 세션에 authUser를 저장하고 메인으로 이동한다. 

## 03.AuthInterceptor 구현하기

### 1.spring-servlet에 url 매핑하기

```xml
<mvc:interceptor>
			<mvc:mapping path="/**" />
			<mvc:exclude-mapping path="/user/auth" />
			<mvc:exclude-mapping path="/user/logout" />
			<mvc:exclude-mapping path="/assets/**" />
			<bean class="com.douzone.mysite.security.AuthInterceptor" />
		</mvc:interceptor>
```

login , logout interceptor를 제외한 다른 메소드에 적용할 authInterceptor 구현 

exclude로 제외할 페이지 선택 가능하다. ex) assets에 있는 css는 interceptor거칠 필요없음 

include로 포함할 url 선택가능

AuthInterceptor : 모든 핸들러에 관한 인증 체크를 하는 것

@auth 가 없으면 그냥 실행 시킨다.  있을 경우 인증이 필요하다고 처리하여 AuthInterceptor 를 실행한다.

### 2.Auth.class

```java
package com.douzone.mysite.security;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target({ElementType.METHOD,ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
public @interface Auth {

	//@interface로 명시하면 어노테이션으로 사용가능
	//Target으로 어디에 쓸건지 명시한다.
	//ElementType.METHOD 메소드 위에 @auth 사용 가능,ElementType.TYPE 클래스 위에 @auth 사용가능
	//컨트롤러에 있는 모든 메소드가 인증이 필요하다고 할때 클래스 위에 @Auth 선언한다.
	//실행시까지 붙어있어라... RetentionPolicy.RUNTIME
	
	
	//롤을 준다. 디폴트값 유저
	//public String value() default "USER";
	public String role() default "USER";
		
	//public boolean test() default false;
}
```

### 3.AuthInterceptor.class

```java
package com.douzone.mysite.security;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import org.springframework.web.method.HandlerMethod;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.handler.HandlerInterceptorAdapter;

import com.douzone.mysite.vo.UserVo;

public class AuthInterceptor extends HandlerInterceptorAdapter {

	@Override
	public boolean preHandle(
			HttpServletRequest request, 
			HttpServletResponse response, 
			Object handler)
			throws Exception {
		
			//업로드 이미지에서 exclude를 하지 않는 이유는 handler가 알아서..뭐..타입을...분류해줘서 라고..한다..정확하지 않다..
			
		//1.handler 종류 확인
		if(handler instanceof HandlerMethod == false) {
			//컨트롤러가 아니면, 이미지등의 다른 데이터일 경우 제어
			return true;
		}
		
		//2. casting
		HandlerMethod handlerMethod = (HandlerMethod)handler;
		
		//3. Handelr Method의 @Auth 정보 받아오기
		Auth auth = handlerMethod.getMethodAnnotation(Auth.class);
		
		//4. Handelr Method에 @Auth가 없으면, Type에 있는지 확인
		if(auth == null) {
			//과제
			
			//클래스에 @Auth가 선언되어있는지 확인
			auth = handlerMethod.getBeanType().getAnnotation(Auth.class);
			
		}
		
		//5. Type과 Method에 @Auth가 적용이 안되어 있는 경우
		//인증이 필요없는 경우
		if(auth == null) {
			return true;
		}
		
		
		//6. @Auth가 적용이 되어 있기 때문에 인증(Authenfication) 여부 확인
		HttpSession session = request.getSession();
		if(session == null) {
			//세션 없을경우 로그인 창으로 보내기
			response.sendRedirect(request.getContextPath() + "/user/login");
			return false;
		}
		
		UserVo authUser = (UserVo)session.getAttribute("authUser");
		if(authUser == null) {
			response.sendRedirect(request.getContextPath() + "/user/login");
			return false;
		}
		
		//7. 권한(Authorization) 체크를 위해서 @Auth의 rolw가져오기("USER", "ADMIN")
		String role = auth.role();
		
		
		System.out.println("----------------");
		System.out.println(role);
		System.out.println(authUser.getRole());
		System.out.println("----------------");
		//로그인한 유저가 admin이면 
		
		//role을 뺏더니 ADMIN이다 그럼 admin페이지로 보내고 user면 user 페이지로 보내기
		if(role.equals(authUser.getRole()) || authUser.getRole().equals("ADMIN")){
			return true;
		}
		
		//8.권한 체크
		//과제....
		response.sendRedirect(request.getContextPath());
		
		
		//여기서 컨트롤러로 보내면 안돼서 false
		return false;
	}

	
}
```

1. 핸들러 종류 확인
2. 핸들러 > 핸들러 메소드로 캐스팅
3. 클래스에 @Auth 어노테이션이 선언되어 있는지 확인하기 위해 어노테이션 가져오기
4. 메소드위에 @Auth가 없다면 클래스 위에는 있는지 가져오기
5. @Auth가 없다 > 인증이 필요 없다. 통과 시킨다. return true
6. 세션 가져오기  > 세션이 없으면 로그인 페이지로 이동시키기
7. authUser 값 가져오기 > 없으면 로그인페이지
8. 권한 체크를 위해 auth의 role을 가져온다.
9. authUser에 있는 role과 auth의 role을 비교해서  같으면 페이지로 보내고, 다르면 안 보낸다. 단, admin은 모든 페이지에 접근 가능하니 명시해준다.
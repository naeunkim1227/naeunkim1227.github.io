---
layout: post
title: jsp-servlet 서블릿 생성하기
subtitle: jsp-servlet
categories: jsp-servlet
tags: [jsp-servlet]
---


서블릿 생성 시

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/70c30f32-9597-499c-97bf-f6f016f6c329/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6074066c-6eb9-4471-857a-1a225080f2c5/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/626dc731-a348-48da-8526-a30df8b732b0/Untitled.png)

web.xml에서 servlet 태그 생성

```xml
<servlet>
		<servlet-name>EmaillistController</servlet-name>
		<servlet-class>com.douzone.emaillist.controller.EmaillistController</servlet-class>
	</servlet>

<servlet-mapping>
		<servlet-name>EmaillistController</servlet-name>
		<url-pattern>/el</url-pattern>		
	</servlet-mapping>
```

서블릿 이름, 서블릿파일 주소 작성해주기

여러 url로 매핑 하기 위해 매핑 태그를 따로 작성해준다.

```java
package com.douzone.emaillist.controller;

import java.io.IOException;
import java.util.List;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.douzone.emailist.dao.EmaillistDAO;
import com.douzone.emailist.vo.EmaillistVO;

public class EmaillistController extends HttpServlet {
	private static final long serialVersionUID = 1L;

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		
		String action = request.getParameter("a");
		if("list".equals(action)) {
			EmaillistDAO dao = new EmaillistDAO();
			List<EmaillistVO> list = dao.findAll();
			
			//목적지 작성하기
			request.setAttribute("list", list);
			
			RequestDispatcher rd= request.getRequestDispatcher("/WEB-INF/views/index.jsp");
			//위의 주소로 보낸다.
			rd.forward(request, response);
		
		}
	
	
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}

}
```

다음과 같이 주소생성

//http://localhost:8080/emaillist02/el?a=list
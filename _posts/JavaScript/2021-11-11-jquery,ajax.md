---
layout: post
title: jquery,ajax
subtitle: 
categories: javaScript
tags: [javaScript]
---

**post 2021-11-11**

1. Marshalling
: 데이터(Object)를 xml로 만드는것
2. Unmarshalling
: xml데이터를 특정 데이터 형태(Object)로 만드는 것
3. 하는 방법
1) OXM(Object Xml Mapping)
: XML데이터와 객체를 매핑
: MarshallingHttpMessageConverter
2) JAXB(Java Architecture for XML Binding)
: OXM를 쉽게 도와주는 Tool
: 마살링/언마살링 dmf Annotation 기반으로 한다.
: JAXBAnnotation(@XmlElementRoot)를 사용해서 직관적 매핑
: Jax2RootElementMessageConverter


### JSONResult.java,XMLResult.java작성하기

1) **XMLResult.java**

```java
package com.douzone.ch08.controller.dto;

import javax.xml.bind.annotation.XmlRootElement;

import com.douzone.ch08.controller.vo.GuestbookVo;

@XmlRootElement(name="response")
public class XmlResult {
	private String result;
	private GuestbookVo data;    /* if success, set */
	private String message;
	
	private XmlResult() {}

	private XmlResult(GuestbookVo data) {
		this.result = "success";
		this.data = data;
	}
	
	private XmlResult(String message) {
		this.result = "fail";
		this.message = message;
	}
	
	
	public String getResult() {
		return result;
	}

	public GuestbookVo getData() {
		return data;
	}

	public String getMessage() {
		return message;
	}

	public void setResult(String result) {
		this.result = result;
	}

	public void setData(GuestbookVo data) {
		this.data = data;
	}

	public void setMessage(String message) {
		this.message = message;
	}

	public static XmlResult success(GuestbookVo data) {
		return new XmlResult(data);
		//성공시, 리턴할 메세지 반환
	}

	public static XmlResult fail(String message) {
		//실패 했을때, 리턴할 메세지 반환
		return new XmlResult(message);
	}
	
	
	
	@XmlRootElement(name="data")
	public static class GuestbookVo {
		private Long no;
		private String name;
		private String password;
		private String regDate;
		private String message;
		
		public Long getNo() {
			return no;
		}
		public void setNo(Long no) {
			this.no = no;
		}
		public String getName() {
			return name;
		}
		public void setName(String name) {
			this.name = name;
		}
		public String getPassword() {
			return password;
		}
		public void setPassword(String password) {
			this.password = password;
		}
		public String getRegDate() {
			return regDate;
		}
		public void setRegDate(String regDate) {
			this.regDate = regDate;
		}
		public String getMessage() {
			return message;
		}
		public void setMessage(String message) {
			this.message = message;
		}
		
	}

		
}
```

```xml
<response>
		<result></result>
		<data>
				<no></no>
				<name></name>
					..
		</data>
		<message></message>
</response>
```

XMLResult파일을 만들때, 변수가 없는 기본생성자를 먼저 만들어 줘야한다.

데이터를 받았을 경우는 data를 전달하고, 그렇지 못할 경우는 message를 전송해준다. 

다음과 같은 형식으로 이루어져 있기 때문에, @XmlRootElement(name="response")로 명시 해준다.

<data>안에 다른 태그값도 있기 때문에, @XmlRootElement(name="data")도 명시해줘야함

**2)JSONResult.java**

```xml
package com.douzone.ch08.controller.dto;

public class JsonResult {
	private String result;  /* "success" or "fail" */
	private Object data;    /* if success, set */
	private String message; /* if fail, set */
	
	private JsonResult() {}
	private JsonResult(Object data) {
		result = "success";
		this.data = data;
		message = null;
	}
	private JsonResult(String message) {
		result = "fail";
		data = null;
		this.message = message;
	}
	
	public static JsonResult success(Object data) {
		return new JsonResult(data);
	}
	
	public static JsonResult fail(String message) {
		return new JsonResult(message);
	}
	public String getResult() {
		return result;
	}
	public Object getData() {
		return data;
	}
	public String getMessage() {
		return message;
	}
}
```

### 1.AJAX- jquery없이 구현하기

```java
<script>
//DOM Load Event
//1. load: 모두 다(DOM, CSSOM, Image) 로딩
//2. DOMContentLoaded: DOM만 로딩(CSSOM x, Image x)
window.addEventListener("DOMContentLoaded", function(){
	document
		.getElementsByTagName("button")[0]
		.addEventListener('click', function(){
			console.log("click");
			var xhr = null;
			
			//1. ActiveXObject ,XMLHttpRequest 생성하기 
			if(window.ActiveXObject){
				// IE8 이하에서 사용가능
				xhr = new ActiveXObject('Microsoft.XMLHTTP');
			}else if(window.XMLHttpRequest){ //Other Standard Browser
				xhr = new XMLHttpRequest();
			}else{
				console.log("Ajax 기능을 사용할 수 없습니다.");
				return;
			}
			
			
			//2.데이터 통신하기
			xhr.addEventListener('readystatechange', function(){
				if(this.readyState == XMLHttpRequest.UNSENT){
					//readyState 0
					//request 초기화 되기 전
					console.log("State: UNSENT")
				}else if(this.readyState == XMLHttpRequest.OPENED){
					console.log("State: OPENED")
					//readyState 1
					//서버와 연결이 성공
					
				}else if(this.readyState == XMLHttpRequest.HEADERS_RECEIVED){
					console.log("State:HEADERS_RECEIVED ")
					//readyState 2
					//서버가 request를 받음
				}else if(this.readyState == XMLHttpRequest.LOADING){
					console.log("State: LOADING")
					//readyState 3
					//response 처리중
				}else if(this.readyState == XMLHttpRequest.DONE){
					console.log("State: DONE")
					//readyState 4
					//response 처리가 끝남
					
					if(this.status != 200){
						console.error(this.state);
						return;
					}
					
					//console.log(this.responseText, typeof(this.responseText));
					//this.responseText는 String 타입이다
					//Object로 변환한다.
					var response= JSON.parse(this.responseText);
					console.log(response, typeof(response));
					
					
					var html = "";
					html += ("<h2>"+ response.data.no +"</h2>");
					html += ("<h2>"+ response.data.name +"</h2>");
					html += ("<h2>"+ response.data.message +"</h2>");
					
					document.getElementById("data").innerHTML = html;
					
					
				}
				
			});	
			
			//3. 데이터를 받을 주소 작성하기
			xhr.open("get",'${pageContext.request.contextPath }/api/json', true);
			xhr.send();
		});

});
</script>
</head>
<body>	
	<h1>AJAX Test - JSON Format Data</h1>
	
	<button>데이터가져오기</button>
	<div id="data">
			
	</div>
	
</body>
</html>
```



1. ActiveXObject ,XMLHttpRequest 생성하기
    
    ActiveXObject는 IE8이하에서 사용가능하다.
    
    ActiveXObject가 있다면, new ActiveXObject('Microsoft.XMLHTTP');로 생성하고
    
    아니면 XMLHttpRequest로 XMLHttpRequest를 생성하자, 둘다 없다면 ajax기능 사용 불가
    
2. 데이터 통신하기
    
    readystate =4 모든 통신이 끝난 상태
    status =200 통신이 성공한 상태
    
    **readystate 상태** 
    
    - UNSENT - 0 request가 초기화 되기전
    - OPENED -1 서버와 연결이 성공
    - HEADERS_RECEIVED - 2 서버가 request를 받음
    - LOADING - 3 response 처리중
    - DONE - 4 response 처리가 끝남

3.  변환

var response= JSON.parse(this.responseText);

responseText는 String 타입으로 반환하게 된다. 따라서 JSON.parse를 통해 Object타입으로 변경해준다.

1. 데이터를 가져올 주소 작성하기
xhr.open("get",'${pageContext.request.contextPath }/api/json', true);
xhr.send();
    
    
    위의 주소로 가서(open) 데이터를 가져온다. 
    
    send로 페이지에 데이터를 전달한다.
    



### 2.Jquery를 사용하여 AJAX구현

```java
<script type="text/javascript" src="${pageContext.request.contextPath }/jquery/jquery-3.6.0.js"></script>
<script>
$(function(){
	vo = {
		name: "뽀로로",
		password: "1234",
		message: "노는게 제일 좋아.."
	};
	$("button").click(function(){
			$.ajax({
				url: "${pageContext.request.contextPath }/api/post02" ,
				async: true,
				type: 'post', //요청 method 
				datatype: 'json',	//받을 포맷
				contentType: 'application/json', 
				data: JSON.stringify(vo),
				success: function(response){
					console.log(response);
					var html = "";
					html += ("<h2>"+ response.data.no +"</h2>");
					html += ("<h2>"+ response.data.name +"</h2>");
					html += ("<h2>"+ response.data.message +"</h2>");
					
					$("#data").append(html);
					
				}
				
			});
	});
	
});
</script>
</head>
<body>	
	<h1>AJAX Test - JSON Format Data</h1>
	
	<button>데이터보내기(post, delete, put) : json 포맷</button>
	<div id="data">
			
	</div>
	
</body>
</html>
```

url: 데이터를 받을 주소
async: 동기화여부
type: 'post', 요청 method 
datatype: 'json', 받을 포맷
contentType: 'application/json', 

data: JSON.stringify(vo),  JSON타입 파일을 String으로 변환 시켜줌
success: function(response){

성공시 실행할 함수

}


ajax흐름

1. jsp에서 데이터를 받는다 (VIEW)
2. ajax를 통해 데이터를 컨트롤러로 전달한다. (Controller)
3. 컨트롤러에서 서비스 , 레포지토리로 이동 
4. DB내용 변경
5. 이때 컨트롤러가 (JsonResult)성공유무, 변경 데이터를 다시 리턴하게된다. 
6. 변경데이터 자바스크립트를 사용하여 뷰페이지에 표시
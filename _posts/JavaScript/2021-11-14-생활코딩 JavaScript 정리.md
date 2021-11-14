---
layout: post
title: 생활코딩 JavaScript 정리
subtitle: 
categories: javaScript
tags: [javaScript]
---

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/48cfa64f-7766-46db-b397-8cfce07be1ec/Untitled.png)

Document 문서 전체를 의미

HTMLDocument

Text 태그안의 텍스트

Comment 주석

Attr 속성

---

# 1.HTML에서 javaScript 로드하기

1. inline 방식
    
    <h1></h1> 과 같은 태그 안에 자바스크립트를 기술하는 방식
    
    정보와 제어가 섞여 있음, 비추천
    

1. script 방식
    
    <script>태그를 넣어 작성하는 방식, html태그와 js코드를 분리할 수 있다.
    

1. 외부파일로 분리
    
    js를 별도의 파일로 분리한다. 재활용성 높음, 전송량의 경량화 
    
2. script 태그의 위치
    
    head위에 위치시킬 수 있으나, window.onload = function(){}안에 작성해야한다.
    
    웹브라우저의 모든 구성요소에 대한 로드가 끝났을 때, 브라우저에 의해 호출되는 함수임
    
    script 파일은 head 태그 보다 페이지의 하단에 위치시키는 것이 더 좋은 방법이다.
    

# 2.Object Model

JSC | DOM | BOM

자바스크립트로 제어하기 위해서, 객체를 만든다.

브라우저에서 html문서의 각각의 태그를 객체로 만들어 놓는다.

객체를 자바스크립트로 제어할 수 있다.

document.getElementsByTagName('img')

img태그를 배열 형태로 모두 가져옴

리턴 형태는 배열이 된다.

### 01)window

전역객체, window, frame을 제어하기 위한 객체

document property에 접근 가능하다. 

### 02)JavaScript Core

브라우저, 노드 js와 같은 스크립트를 제어할 수 있다.

자바스크립트 자체 객체인 Object, Array, Function 을 사용할 수 있다.

### 03)DOM(Document Object Model)

document가 하는일 - html태그를 제어하는 역할을 한다.

문서를 제어한다.

---

# 3.BOM

### BOM(Browser Object Model)

window객체의 property에 저장되어 있다.

현재 웹브라우저의 페이지 리로드, 경고창 등을 담당한다.

브라우저를 제어함

BOM(Browser Object Model)이란 웹브라우저의 창이나 프래임을 추상화해서 프로그래밍적으로 제어할 수 있도록 제공하는 수단이다. BOM은 전역객체인 Window의 프로퍼티와 메소드들을 통해서 제어할 수 있다. 따라서 BOM에 대한 수업은 Window 객체의 프로퍼티와 메소드의 사용법을 배우는 것이라고 해도 과언이 아닐 것이다. 본 토픽의 하위 수업에서는 Window 객체의 사용법을 알아볼 것이다.

window. ** 로 접근한다.

---

# 4.Document 객체

---

# 5.Text 객체

- document.getElementById : 리턴 데이터 타입은 HTMLLIELement
- document.getElementsByTagName : 리턴 데이터 타입은 HTMLCollection

즉 실행결과가 하나인 경우 HTMLLIELement, 복수인 경우 HTMLCollection을 리턴하고 있다

---

# 6.Jquery

유사배열로 리턴한다.

for(var i=0; i<li.length; i++){

console.log(li[i]);

}

li는 object

li[i] HTMLLIElement이다. > 제이쿼리 객체가 아니라 DOM 객체임

따라서 제이쿼리 메소드 .css를 사용할 수 없다.

사용하려면 $() 제이쿼리 함수에 감싸서 사용하면 된다.

$(''li")

map을 통한 조회

map을 통해 모든 엘리먼트 실행가능하다. 

li,map(function(index,elem){

console.log(index,elem);

$(elem).css('color', 'red');

})

index가 몇번째값

elem가 모든 엘리먼트값을 호출한다.

elem는 DOM객체이기 때문에 $()감싸서 사용해준다.

---

# 7.Element 객체

1. 모든HTML태그는 HTMLElement에 속한다. style과 같은 속성을 제어 할 수 있다.
2. html, xml,svg와 같은 다양한 언어 형식들이 존재하기 때문에 html은 HTMLElement로 구별한다.
    
    style같은 property사용 가능하다. 
    
3. 식별자, 조회, 속성 기능이 있다.

### **식별자**

문서내에서 특정한 엘리먼트를 식별하기 위한 용도로 사용되는 API

- Element.classList
- Element.className
- Element.id
- Element.tagName

### **조회**

엘리먼트의 하위 엘리먼트를 조회하는 API

- Element.getElementsByClassName
- Element.getElementsByTagName
- Element.querySelector
- Element.querySelectorAll

### **속성**

엘리먼트의 속성을 알아내고 변경하는 API

- Element.getAttribute(name)
- Element.setAttribute(name, value)
- Element.hasAttribute(name);
- Element.removeAttribute(name);

## 식별자 API

1. document.getElementById('id').tagName : 값 변경 불가
2. document.getElementById('id').id :값 변경 가능
3. document.getElementById('id').className :값 변경가능
4. document.getElementById('id').classList 
    - 유사배열 형태를 리턴하는데 class= "a b"이면 길이 두개, classList[0] 은 a , classList[1]은 b가 나오게 된다.
    - classList.add를 통해 추가 가능하다.
    - classList.toggle로 값을 추가했다가, 없앴다가가 가능함
    

## 조회 API

1. Element도 getElementBy 메소드를 가지고 있다., 조회의 범위를 좁히고자 한다면 getElementBy*로 조회한다.
2. document.getElementsBy* 는 문서전체를 대상으로 조회해서 적용한다.  Element의 하위 메소드  getElementById*를 하면,  Element 가 가지고 있는 하위 메소드만 찾아 적용한다.

## 속성 API

-<a id='' class= '' href=''></a> 의 태그 안에 있는 id, class 등등 속성을 조회하고, 추가, 삭제등의 변경이 가능한 api이다.

1. Element.getAttribute(name)
2. Element.setAttribute(name, value)
3. Element.hasAttribute(name);
4. Element.removeAttribute(name);

**속성과 프로퍼티**

- 속성방식

target.setAttribute('class', '')

- 프로퍼티 방식

target.ClassName = ''

## jQuery 속성 제어 API

- jquery에서는 attr을 통해 속성을 제어할 수 있다.
- attr
- removeAttr

**attribute 와 property**

jquery에서는 attribute를 attr, property는 prop로 사용한다.

각각 href를 조회 했을 때, 

attr은 ex) ./demo.html 

prop은 전체주소를 리턴한다. ex) [http://localhost/jQuery_attribute_api/demo.html](http://localhost/jQuery_attribute_api/demo.html)

제이쿼리를 통해, prop의 값으로 property의 제약을 보완해준다.

## JQuery 조회 범위 제한

1. **selector context**
    1. $("선택 인자1", "선택인자2").css 로 명시하면 선택인자2하위의 선택인자1만 속성 적용
    2. $("선택인자1 선택인자2") 도 사용가능
2. **.find()**
    1. $("선택인자1.").find("선택인자2").css 로도 적용이 가능하다. 체인이 가능하기 때문이다.

---

# 8.Node 객체

가장 상위의 객체이다. Node 객체를 통해 모든 객체에서 사용 가능하다. 각각의 관계들을 유추할 수 있어 프로그래밍적으로 유용하게 사용할 수 있다.

1. 관계
    - Node.childNodes
    - Node.firstChild
    - Node.lastChild
    - Node.nextSibling
    - Node.previousSibling 현재 li Element의 이전 형제
    - Node.contains()
    - Node.hasChildNodes()
    - Node.parentNode
2. 노드의 종류
    - Node.nodeType : 값이 Text인지 document인지 등의 타입을 알려줌
    - Node.nodeName:
3. 값
    - Node.nodeValue
    - Node.textContent
4. 자식관리
    - Node.appendChild()
    - Node.removeChild()

### 01)Node 관계 API

**공백,줄바꿈 문자도 child로 취급함 (*text객체도 포함하기 때문이다.)**

1. Node.childNodes 
    1. childNodes로 자식들을 조회가능하다.
    2. 유사배열로 반환한다.
    3. 전체적으로 속성을 지정하고자 할때, text엘리먼트가 있으면 전체 지정이 되지 않는다.
2. Node.firstChild
3. Node.lastChild
4. Node.nextSibling 
    1. 지정한 Element의 다음 형제
5. Node.previousSibling 
    1. 지정한 Element의 이전 형제
6. Node.contains() 
7. Node.hasChildNodes() 

### 02)노드 종류 API

노드 작업을 하게 되면 현재 선택된 노드가 어떤 타입인지를 판단해야 하는 경우가 있다. 이런 경우에 사용할 수 있는 API가 nodeType, nodeName이다.

```xml
for(var name in Node){
   console.log(name, Node[name]);
}
```

위를 통해 노드 종류를 조회할 수 있다.

1. Node.nodeType
    1. 노드 타입을 의미한다. 번호로 출력된다.
2. Node.nodeName
    1. 노드의 이름 (태그명을 의미한다.)

### 03)노드 변경 API

1. 노드 추가
    1. appendChild(child) - 노드의 마지막 자식으로 주어진 엘리먼트 추가
    2. insertBefore(new Element, referenceElement) -두번째 인자 앞에 엘리먼트 추가
    3. 노드를 추가하기 위해서 추가 엘리먼트를 생성해야 한다. document객체의 기능을 사용하여 추가해야한다.
        1. document.createElement(tagname) - <li>와 같은 태그
        2. document.createTextNode(data) - 텍스트 데이터
    
    순서 createElement로 태그 생성 > .createTextNode로 텍스트 데이터 추가 > appendChild로 태그 안에 텍스트데이터 넣기 > appendChild로 html에 추가 해주기
    
2. 노드 제거
    1. removeChild(child)
        1. 부모 노드에서, 자식 노드를 삭제해야 <l태그>데이터</태그>가 삭제된다.
3. 노드 바꾸기

### 04)jQuery 노드 변경 API

### 05)문자열로 노드 제어

---

# 9.Document 객체

# 10.Text 객체

값 API

조작 API

---

# 11.문서의 기하학적 특성

---

# 12.이벤트

### 등록방법

### inline

### 프로퍼티 리스터

### addEventListener()

### 이벤트 전파(버블링과 캡처링)

### 기본동작의 취소

### 이벤트타입

1. 폼
2. 문서로딩
3. 마우스

### JQuery 이벤트

on API 사용법

---

# 13.네트워크 통신

### AJAX

- 웹브라우저와 웹서버가 내부적으로 데이터 통신을 하여, 로딩없이 데이터를 전달 받는다.
- 사용 API : XMLHttpRequest
- XMLHttpRequest:
    
    ```java
    open(''GET/POST', '페이지주소');
    send():
    ```
    

```java
<p>time : <span id="time"></span></p>
<input type="button" id="execute" value="execute" />
<script>
document.querySelector('input').addEventListener('click', function(event){
    var xhr = new XMLHttpRequest();
    xhr.open('GET', './time.php');
    xhr.onreadystatechange = function(){
        if(xhr.readyState === 4 && xhr.status === 200){
            document.querySelector('#time').innerHTML = xhr.responseText;
        }
    }
    xhr.send(); 
}); 
</script>
```

 readystate =4 모든 통신이 끝난 상태

status =200 통신이 성공한 상태

status 404 500이면 오류페이지

- POST방식
    
    정보를 post로 전달해서 알맞은 데이터를 가져온다.
    

### JSON

- 

### JQueryAjax
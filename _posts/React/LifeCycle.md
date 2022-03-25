---
layout: post
title: LifeCycle 간단 정리
subtitle:
categories: React
tags: [React]
---


페이지에서 렌더링되기 전인 준비과정에서 시작해서, 페이지에서 사라질때 끝이 난다.

<aside>
🗣 생성 > 업데이트 > 제거의 과정
</aside>

### 1.생성

컴포넌트가 브라우저에 나타나기 전, 후에 호출되는 API

1. **constructor** : 컴포넌트가 새로만들어 질때마다 이 함수가 호출된다.
2. static  getDerivedStateFromProps() : 16.3버전이후 만들어짐, props로 받아온 값을 state로 동기화하는 작업을 해줘야하는 경우에 사용된다. 최초 마운트시와 갱신시 render() 메소드 호출 직전에 호출된다.  props에 state가 의존하는 경우에 사용한다.  다만 state를 끌어오면 코드가 장황해지므로 다른 api사용을 추천한다고 함
3. render()
4. **componentDidMount : 컴포넌트가 화면에 나타나게 됐을 때, 호출된다. 주로 axios,fetch등을 통해 ajax요청을 할때의 작업을 진행한다.**

### 2.업데이트

props,state의 변화에 따라 결정된다. 

1. static getDerivedStateFromProps()
2. shouldComponentUpdate : 컴포넌트 최적화 작업에서 사용,  효율성을 위해 도입된 API로 조건을 지정한다고 생각하면 된다. 기본적으로 true함수를 호출한다 false일경우 render(), **`UNSAFE_componentWillUpdate()` ,** componentDidupdate() 를 호출하지 않는다.
3. render()
4. getSnapshotBeforeUpdate()  : render()함수 호출 후에 실행하게 되는데, 변화가 일어나기 전의 DOM상태를 가져와서 다시 업데이트를 할 수 있게 한다.  예를 들어 새로운 데이터가 들어왔어도 채팅창 스크롤위치를 그대로 고정하고 싶을때,  사용하면 된다. (이전의 스크롤 top,height를 가져와서, 이 값으로 다시 되돌려 놓는 것임)
5. componentDidUpdate : render()를 호출하고 난 뒤 발생하게 된다. 

### 3.제거

1. componentWillUnmount : 등록했었던 이벤트를 제거한다.

<aside>
🗣 호출순서 constructor > componentWillMount  > render > **componentDidMount >** shouldComponentUpdate(조건이 성립될 경우 ex) props의 값이 5의 배수일때만 호출) >  componentWillUpdate > render > componentDidUpdate
</aside>
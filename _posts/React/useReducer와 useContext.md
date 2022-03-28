---
layout: post
title: useReducer와 useContext
subtitle:
categories: React
tags: [React]


# useReducer

- useState의 대체함수
- 복잡한 정적로직을 만드는 경우, 다음 state가 이전 state에 의존적인 경우 useReducer를 사용한다.
- 컴포넌트 안에서만 데이터를 저장한다.
- 선택적으로 init을 가질 수 있다.
    

useReducer는 다음 두 함수를 가지게 된다.

### 1) reducer function(콜백함수)

- state,action을 가지고 값을 리턴하는 함수
    
    ```jsx
    function reducer(state,action) {
    	return state + action;
    }
    ```
    
    ```jsx
    function reducer(state,action){
    	if(action.type === '증가'){
    			return state+1;
    	else if(action.type === '감소')	
    			return state-1;
    	}			
    }
    ```
    

### 2)dispatch function

- reducer fuction에 action을 전달하는 함수
    
    ```jsx
    <button onClick= {() => dispatch({type: "증가"})}+ </button>
    ```
    

> const [state,dispatch]=useReducer(reducer,initialState); 가 기본형식, reducer는 리듀서 함수를 지칭하고 initialState는 초기상태
>
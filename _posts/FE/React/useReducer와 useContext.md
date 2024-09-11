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
    
 const [state,dispatch]=useReducer(reducer,initialState); 가 기본형식, reducer는 리듀서 함수를 지칭하고 initialState는 초기상태
 사용자가 클릭하는 버튼 > dispatch 함수 실행이라 보면 된다.



# useReducer

useReducer를 통해 setState로 관리했던 상태를 조금 더 안정성있는 코드로 개선할 수 있다. 
  체크가 되있는 상태면 해제하게 하는 onChange함수 로직이 있다. 

 

### 1. onChange함수에 set 함수 사용

```jsx

function Checkbox() {

	const [checked,setCheked] = useState(false);

return (

	<input type="checkbox" value={checked} 
		onChange = {() => setChecked (checked => !checked)} />
	{checked? "checked" : "not checked"}
)

}

```

위의 코드는 개발자가 잘못된 정보를 보냈을때 문제가 발생하는 경우가 있으므로 익명함수를 추가해서 개선할 수 있다.

### 2. 익명함수 사용

```jsx
function Checkbox() {

	const [checked,setCheked] = useState(false);

	function toggle() {
		setChecked(checked => !checked)
	}

return (

	<input type="checkbox" value={checked} onChange={toggle} />
	{checked? "checked" : "not checked"}
)

}
```

토글 함수는 체크 상태에서 , 언체크상태로 변경을 한다. 다음과 같이, 현재 상태를 받아서 새상태를 반환하는 것을 useState대신 useReducer를 사용하면 된다.

### 3.useReducer사용

```jsx
function Checkbox() {

	const [checked,setCheked] = useReducer(checked => !checked,false)
//초기상태 false를 받음

return (

	<input type="checkbox" value={checked} onChange={toggle} />
)

}
```





# useContext

### 1)context

- react는 props이용해 데이터를 전달할 수 있는 구조이다. 그러나 A,B,C 컴포넌트가 각각 부모자식 관계일때, A에서 C로 데이터를 전달하려면 중간 컴포넌트인 B를 거쳐야했다. 이러한 불편함을 개선하기 위해 사용하는것이 useContext이다.
- context 값이 변경되면 항상 리렌더링한다.
- 단계마다 일일이 props를 넘겨주지 않고 트리 전체에 데이터를 제공해줄 수 있다.
- API
    - React.createContext
        
        ```jsx
        const MyContext = React.createContext(defaultValue);
        ```
        
    - React.Provider
        - context의 변화를 알리는 역할을 한다. provider의 value가 바뀔때마다 다시 렌터링한다.
        
        ```jsx
        <MyContext.Provider value={/* 어떤 값 */}>
        ```
        
    - Class.contextType
    - Context.Consumer
        - context의 변화를 감시하는 컴포넌트
            
            ```jsx
            <MyContext.Consumer>
              {value => /* context 값을 이용한 렌더링 */}
            </MyContext.Consumer>
            ```
            
    - Context.displayName
# Vue-Router
작성일자 : 2022-12-19
## Watch
> $route를 이용하여 route의 변화를 감지하여 함수를 실행할 수 있음  
> handler 속성의 경우 route의 변화를 감지하여 해당 이름의 함수를 실행, immediate 속성은 즉시 실행  
> router -> index.js 에서 /:param-id 를 사용하여 할당받을 수 있음
```javascript
// router/index.js
// param에 데이터 할당 방법
{
		path: '/b/:bid',
		name: 'board',
		component: Board,
		children: [
			{
				path: 'c/:cid',
				component: Card,
			},
		],
	},
```
```javascript
// watch 사용법
watch: {
		$route: {
			handler: 'fetchData',
			immediate: true,
		},
	},
```

***
## $route vs $router
> `$router`는 Router 인스턴스임. 웹 애플리케이션에서 하나만 존재. 전반적인 라우터 기능 관리.  
> `router-link` 요소 없이 프로그램적으로 페이지 이동 `this.$router.push()`할 때 사용함  


> `$route`는 Route 인스턴스임. 페이지 이동 등으로 라우팅이 발생할 때마다 생성됨. 현재 활성화된 라우트 상태를 저장한 객체  
> 현재 경로, url parameter, 이름등의 정보를 받을 수 있음. `watch`를 통해 모니터링 하기도 함

<br>  

***

## Router Instance Methods

|메소드|설명|
|:---:|:---:|
|push|URL 이동. 히스토리 스택에 추가되므로 뒤로가기 버튼 동작시 이전 URL 로 이동|
|replace|	URL 이동. 현재 URL 을 대체하기 때문에 히스토리 스택 쌓지 않음|
|go|	숫자만큼 뒤로가기 또는 앞으로 가기 (음수:backward, 양수: forward)|  

<br>

### push
> `<router-link :to='path'>`에서 페이지 이동을 하면 내부에서 `$router.push` 를 호출함  
> push 메소드를 사용하면 히스토리 스택에 추가됨 -> 뒤로가기를 눌렀을 때, 순차적으로 전 페이지로 이동함

### replace
> push 메소드와 같이 페이지 이동하지만 히스토리 스택이 쌓이지 않음  
> 단순히 현재 페이지를 전환하는 역할만 함 
### go
> 인자로 넘긴 숫자만큼 히스토리 스택에서 앞, 뒤로 이동함  
>해당 숫자의 URL이 스택에 없으면 라우팅 실패함

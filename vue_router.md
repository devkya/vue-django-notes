# Vue-Router
## watch
> $route를 이용하여 route의 변화를 감지하여 함수를 실행할 수 있음  
> handler 속성의 경우 route의 변화를 감지하여 해당 이름의 함수를 실행, immediate 속성은 즉시 실행  
> router -> index.js 에서 /:param-id 를 사용하여 할당받을 수 있음
```javascript
// router/index.js
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
watch: {
		$route: {
			handler: 'fetchData',
			immediate: true,
		},
	},
```
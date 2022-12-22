# Axios
## `api/index.js` 분리
> vue cli 로 프로젝트 진행 시 `src -> api` 폴더를 생성하여 별도로 관리하는 것이 좋음  
> 아래 코드는 예시
```javascript
// api/index.js
import axios from 'axios';
import router from '@/router';

const DOMAIN = 'http://localhost:3000';
const UNAUTHORIZED = 401;
const onUnauthrorized = () => {
	router.push('/login');
};

const request = (method, url, data) => {
	return axios({
		method,
		url: DOMAIN + url,
		data,
	})
		.then(result => {
			result.data;
		})
		.catch(result => {
			const { status } = result.response;
			if (status === UNAUTHORIZED) return onUnauthrorized();
			throw Error(result);
		});
};

export const board = {
	fetch() {
		return request('get', '/boards');
	},
};
```
***
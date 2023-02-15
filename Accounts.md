# Accounts
## `dj-rest-auth` & `django-allauth`
### `dj-rest-auth`([참고문서](https://dj-rest-auth.readthedocs.io/en/latest/index.html))
> `dj-rest-auth`는 사용자 등록 및 인증 작업을 처리하기 위한 엔드포인트를 제공함
* 특징
    1. 활성화를 통한 사용자 등록
    2. 로그인/ 로그아웃
    3. Django 사용자 모델 검색/업데이트
    4. 비밀번호 변경
    5. 이메일을 통한 비밀번호 재설정

    
## JWT 토큰 안전하게 저장하는 방법([참고문서](https://hshine1226.medium.com/localstorage-vs-cookies-jwt-%ED%86%A0%ED%81%B0%EC%9D%84-%EC%95%88%EC%A0%84%ED%95%98%EA%B2%8C-%EC%A0%80%EC%9E%A5%ED%95%98%EA%B8%B0-%EC%9C%84%ED%95%B4-%EC%95%8C%EC%95%84%EC%95%BC%ED%95%A0-%EB%AA%A8%EB%93%A0%EA%B2%83-4fb7fb41327c))
> 발행한 JWT 토큰은 어디에 저장해야 할까?  
> 프론트엔드에서 저장하는 방법은 두 가지이다. 첫번째는 `localstorage`에 저장하는 방식이고, 두번째는 `cookies`에 저장하는 방식이다. 
### LocalStorage 장단점
* 장점
    - 편리함
* 단점
    - XSS 공격에 취약함
        - 웹사이트에서 공격자가 `JavaScript`를 실행할 수 있을 때 발생함
        - `localStorage`에 저장되어 있는 `access token`을 탈취할 수 있음
### Cookies 장단점
* 장점
    - 쿠키는 `JavaScript`로 접근 불가 -> XSS 공격에 취약하지 않음(완전 안전하지는 않음)
    - 자동으로 모든 HTTP 요청에 포함되어 보내짐
* 단점
    - 일부 케이스에서 토큰을 쿠키에 저장하지 못할 수 있음
    - CSRF 공격에 취약함

### 이상적인 설정 방법
1. `access token`을 in-memory에 저장함(front-end variable 저장) -> 탭 전환 및 새로고침 시에 사라짐
2. `refresh token`을 쿠키 또는 `localStorage`에 저장함 -> `access token`이 사라지거나 만료되었을 때, `/refresh-token` 엔드포인트로 보내고 새로운 `access token`을 받음 

## 다른 도메인으로 Credentials 보내기
> vuejs `index.html`를 빌드하기 전까지는 vuejs와 django의 도메인이 다름.  
> 기본적으로 브라우저가 제공하는 요청 API들은 별도의 옵션 없이 브라우저의 쿠키와 같은 인증과 관련된 데이터를 함부로 요청 데이터에 담지 않도록 되어있음.   
>따라서 백엔드에서 Set-Cookie를 Response에 담아도, 프론트엔드 단에서 설정되지 않음.
### 해결 방법
>`withCredentials` 옵션을 `true` 설정하면 된다. 이 옵션은 다른 도메인(Cross Origin)에 요청을 보낼 때 요청에 인증 정보를 담아서 보낼 지를 결정하는 옵션임
* `withCredentials` 옵션 `true` 설정
* 백엔드와 프론트엔드 둘다 `Access-Control-Allow-Credentials` 헤더를 `true` 설정

# 문제점
1. 내가 필요한건 kakao/login/finish/ response 이나 kakao/callback/ response를 받음
    * kakao/login/finish/ 를 redirect_url로 설정한 후 오버라이딩 하는 방법은?
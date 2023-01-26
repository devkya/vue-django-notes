# Social Login
## 프로세스
[참고 링크](https://medium.com/swlh/how-to-build-google-social-login-in-django-rest-framework-and-nuxt-auth-and-refresh-its-jwt-token-752601d7a6f3)
<p align="center">
    <img src="https://miro.medium.com/max/1400/1*9woo6M--3ZWuUExOY_K0YQ.webp" alt="deploy" style="width: 500px" />
</p>

1. 프론트엔드 소셜 로그인 화면(ex, Click google login button)
2. 소셜에서 code & access token 반환
3. 프론트엔드에서 code & access token 제공 -> 백엔드에 JWT 토큰 요청
4. 제공받은 code & access token 소셜 서버에서 검증
5. 유저 생성
6. JWT access & refresh token 발급
7. 백엔드에 유저 정보 요청
8. 유저 정보 반환

## 참고
1. [중간 페이지를 피하기 위한 settings.py 변경](https://stackoverflow.com/questions/70873098/login-with-google-redairecting-on-conformation-page-to-continue-django)
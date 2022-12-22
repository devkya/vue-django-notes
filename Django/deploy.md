# Django 배포하기

## 기본 개념
### Web Server
> 단순히 정적 파일을 응답

### WAS(Web Application Server)
> 클라이언트 요청에 대해 동적인 처리가 이뤄진 후 응답

### WSGI(Web Server Gateway Interface)
>

***

## 인프라 구성
<p align="center">
    <img src="../assets/deploy.png" alt="deploy" style="width: 800px" />
</p>


### Nginx
> Web Server 역할 : 정적 요청(html, css, image ...) 처리와 WAS의 부담을 덜어주기 위해 사용함

> 클라이언트의 요청을 처리해주는 경량 웹서버로서 정적요청은 `Nginx` 에서 자체적으로 처리하고, 동적 요청만 `Gunicorn` + `Django`에 전달하여 처리하기 위해 사용

* 장점
    1. 빠른 속도(메모리 사용량 적음)
    2. 리버스 프록시(Reverse proxy)로 사용 가능
        - 여러 WAS가 존재하면 요청 분산시키는 역할 수행 -> Load balancing
        - 캐싱 기능(WAS까지 요청하지 않아도 클라이언트 요청에 빠르게 응답)
        - WAS 정보(기기 id, MAC Address)를 숨기는 보안 역할 수행
    3. SSL 지원
    4. 웹페이지 접근 인증
        - 로그인 정보(관리자, 사용자)를 WAS에서 하지 않고 Nginx에서도 가능
    5. 비동기 처리 
        - 많은 트래픽을 동시 처리 가능

### Gunicorn()
> `Nginx`가 `Django`에게 동적 요청을 전달할 수 있도록 도와주는 역할
### RDS
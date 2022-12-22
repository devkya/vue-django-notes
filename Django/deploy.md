# Django 배포하기

## 기본 개념
### Web Server
> 단순히 정적 파일을 응답


### WAS(Web Application Server)
> 클라이언트 요청에 대해 동적인 처리가 이뤄진 후 응답

### WSGI(Web Server Gateway Interface)
> CGI(Common Gateway Interface)의 일종으로 여러 언어 사용자들의 다양한 요청을 이해할 수 있도록 공통된 규칙으로 변환하는 관문 역할

> Client 요청 -> CGI로 일관된 형태로 해석 -> WAS에서 처리
* CGI 기본 동작 과정
    1. Input으로 HttpRequest를 받음
    2. 요청에 대한 정보를 환경변수 형식으로 만들어 파이썬 스크립트의 stdin형식의 input으로 받음
    3. 스크립트가 print와 같은 stdout 형식으로 응답하면 HTTP 형식으로 변환




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

### Gunicorn

> `Nginx`가 `Django`에게 동적 요청을 전달할 수 있도록 도와주는 역할  
> `runserver`와 똑같은 역할을 수행하지만, 보안적, 성능적으로 검증되지 않았기 때문에 production 환경에서는 사용 불가  

### AWS EC2
> Amazon Elastic Compute Cloud(Amazon EC2)는 확작 가능 컴퓨팅 용량을 제공함  
* 기능  
    [EC2 참고 링크](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/concepts.html)
    1. 인스턴스 : 가상 컴퓨팅 환경
    2. 인스턴스 유형 : 인스턴스를 위한 CPU, Memory, Storage, Networking 등 구성 제공

### AWS RDS
> 인프라 및 데이터베이스 업데이트를 관리해주고 DB 설치, 운영, 관리를 지원하는 서비스    

> 데이터베이스 사용 방안은 2가지로 나뉨 -> RDS 사용 vs EC2에 DB 직접 설치  
> DB가 복잡한게 아니라면 EC2에 직접 설치하여도 별 문제가 없다고 보여짐

***

## 연동하기
# HTTP

Last Edited: Feb 13, 2019 3:55 PM
Tags: HTTP,WEB

## 3xx Redirection

- HTTP 리다이렉트는 3xx 상태 코드를 지닌 응답
- 리다이렉트 응답을 수신한 브라우저는, 제공된 새로운 URL(Location에 표시된)을 다시 GET으로 요청

[Redirections in HTTP](https://developer.mozilla.org/ko/docs/Web/HTTP/Redirections)

## HTTP 요청 데이터
```
    POST /user/create HTTP/1.1               요청 라인
    HOST: localhost:8080
    Connection-Length: 59
    Content-Type: application/x-wwww-form-urlencoded
    Accept: */*
    																				 헤더와 본문 사이의 빈 공백 라인
    userId=javajigi&password=password        요청 본문(body)
```

## HTTP 응답 데이터
```
    HTTP/1.1 200 OK                        상태라인
    Content-Type: text/html;charset=utf-8
    Content-Length: 20
    																			 헤더와 본문 사이의 빈 공백 라인
    <h1>Hello World<h1>                    응답 본문
```
## 한 페이지 당 웹브라우저 요청이 여러번 일어나는 이유

/index.html 요청을 한번 보냈는데 css, js 등 여러 개의 추가 요청이 발생하는 이유는 index.html을 받은 브라우저가 HTML 내용을 분석해서 CSS, JS, 이미지 등 자원이 포함되어 있으면 서버에 해당 자원을 다시 요청하게 된다. 하나의 웹 페이지를 사용자에게 정상적으로 보여주려면 클라이언트와 서버 간의 여러 번의 요청과 응답이 필요하다. 그리고 여러 자원에 대한 요청은 GET방식으로 요청한다.

- HTML은 기본적으로 GET, POST 메소드만 지원한다.
- <form>태그가 지원하는 메소드 속성이 GET과 POST 뿐이다.

## GET, POST 사용기준

- GET : 서버에 존재하는 데이터를 조회하는 역할만 하는 것.
- POST : 데이터의 상태를 변경하는 작업을 담당.(추가, 수정, 삭제)
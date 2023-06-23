---
title:  "HTTP 기초"
excerpt: "DevOps 부트캠프 Section 1 "

categories:
  - Blog
tags:
  - [Blog, DevOps]

toc: true
toc_sticky: true
 
date: 2023-03-16
last_modified_at: 2023-03-16
---
# HTTP
 웹 상에서 웹 서버 및 웹브라우저 상호 간의 데이터 전송을 위한 응용계층 프로토콜
   - 처음에는, WWW 상의 하이퍼텍스트 형태의 문서를 전달하는데 주로 이용
   - 현재에는, 이미지,비디오,음성 등 거의 모든 형식의 데이터 전송 가능

<br><br>

## HTTP의 특징
요청 및 응답 메세지로 대응되는 구조
   - 동작형태가 클라이언트/서버 모델로 동작
   ![alt text](/images/http.jpg)<br><br>

메세지 교환 형태의 프로토콜 
   - 클라이언트와 서버 간에 `HTTP 메세지`를 주고받으며 통신 <br>
   클라이언트-서버 프로토콜 <br>
   SMTP 전자메일 프로토콜과 유사 <br> 


트랜잭션 중심의 비연결성 프로토콜
   - 종단간 연결이 없음 (Connectionless) 
   - 이전의 상태를 유지하지 않음 (Stateless)

수송계층 프로토콜 및 사용 포트 번호        
   - 수송계층 프로토콜 : TCP  
   - 사용 포트 번호    : 80번


### HTTP 요청 메서드
GET <br>
특정 리소스를 받기 위한 요청이다. 따라서, 리소스의 생성, 수정 및 삭제 등에 사용해서는 안된다.<br>

POST<br>
 리소스를 생성하거나 컨트롤러를 실행하는 데 사용한다.<br>

PUT<br>
변경 가능한 리소스를 업데이트하는 데 사용되며 항상 리소스 식별 정보를 포함해야 한다.<br>

PATCH<br>
 변경 가능한 리소스의 부분 업데이트에 사용되며 항상 리소스 식별 정보를 포함해야 한다.<br>
 PUT을 사용해 전체 객체를 업데이트하는 것이 관례여서 거의 사용되지 않는다.<br>

DELETE<br>
 특정 리소스를 제거하는 데 사용한다.<br>
 일반적으로 Request body가 아닌 URI 경로에 제거하려는 리소스의 ID를 전달한다.<br>

HEAD<br>
 클라이언트가 본문 없이 리소스에 대한 헹더만 검색하는 경우 사용한다.<br>
 일반적으로 클라이언트가 서버에 리소스가 있는 지 확인하거나 메타 데이터를 읽으려는 때만 GET 대신 사용한다.<br>

OPTIONS<br>
 클라이언트가 서버의 리소스에 대해 수행 가능한 동작을 알아보기 위해 사용한다.<br> 
 일반적으로 서버는 이 리소스에 대해 사용할 수 있는 HTTP 요청 메서드를 포함하는 Allow 헤더를 반환한다. (CORS에 사용)<br><br>

 #### PUT VS POST 
 멱등성(Idempotence): 멱등성이란 여러번 수행해도 결과가 같음을 의미한다.<br>
 HTTP 메소드를 예를 들자면, GET, PUT, DELETE는 같은 경로로 여러 번 호출해도 결과가 같다.<br>
 그러나 POST는 매 호출마다 새로운 데이터가 추가된다. <br>
 따라서, POST 연산은 결과가 Idempotent하지 않지만, PUT은 반복 수행해도 그 결과가 Idempotent 하다. 
<br><br>

 ## HTTP 메세지
- HTTP 메시지는 서버와 클라이언트 간에 데이터가 교환되는 방식을 말한다. 
- ASCII로 인코딩된 텍스트 정보이며 여러 줄로 되어 있다.
- 요청(request)과 응답(response) 두 가지 타입이 존재하며, 각각 특정한 포맷을 가지고 있다. 
- 개발자가 HTTP 메시지를 텍스트로 작성하는 경우는 잘 없다. 대신 소프트웨어, 브라우저, 프록시, 또는 웹 서버가 그 일을 대신한다.
- HTTP 메시지는 설정 파일(프록시 혹은 서버의 경우), API(브라우저의 경우), 혹은 다른 인터페이스를 통해 제공된다.
단순한 메시지 구조를 이루고 있고, 확장성이 좋다는 특징이 있다.


### 공통적 구조
HTTP 요청과 응답의 구조는 서로 유사한 면이 있는데, 구체적으로는 아래와 같다.
   ![alt text](/images/common.png)<br>
   [출처](https://developer.mozilla.org/ko/docs/Web/HTTP/Messages): https://developer.mozilla.org/ko/docs/Web/HTTP/Messages<br>

1. 시작 줄(start-line): HTTP 요청 / 또는 요청에 대한 성공 또는 실패가 기록된다. 항상 한 줄로 끝난다.
2. HTTP 헤더: 시작 줄 다음으로 요청에 대한 설명 / 또는 메시지 본문에 대한 설명이 들어간다.
3. 빈 줄: 요청에 대한 모든 메타 정보가 전송되었음을 알리는 빈 줄이 삽입된다. (헤드와 본문 사이)
4. 본문(optional): 요청과 관련된 데이터(HTML form 콘텐츠 등) / 또는 응답과 관련된 문서(document)가 선택적으로 들어간다. 본문의 존재와 크기는 시작 줄 및 HTTP 헤더에 명시된다.
- HTTP 메시지의 시작 줄과 HTTP 헤더를 묶어서 요청 헤드(head) 라고 부르며, 이와 반대로 HTTP 메시지의 페이로드는 본문(body) 이라고 한다.


### Request
요청(request)은 클라이언트가 서버로 전달하는 메시지로, 서버 측 액션을 유도한다.
   ![alt text](/images/request.png)<br>
   [출처](https://developer.mozilla.org/ko/docs/Web/HTTP/Messages): https://developer.mozilla.org/ko/docs/Web/HTTP/Messages <br>

시작 줄(start-line, Request-line)은 아래 세 가지 요소로 이루어져 있다.
> HTTP 메소드 + URI + HTTP 버전

1. HTTP 메소드: 영어 동사(GET, PUT,POST) 혹은 명사(HEAD, OPTIONS)를 사용해 서버가 수행해야 할 동작을 나타낸다.

2. URI: URL, 또는 프로토콜, 포트, 도메인의 절대 경로가 온다. URI 포맷은 아래와 같이 나눌 수 있다.

   - origin 형식: 끝에 '?'와 쿼리 문자열이 붙는 절대 경로이다. 가장 일반적인 형식으로, GET, POST, HEAD, OPTIONS 메소드와 함께 쓰인다.

   > POST / HTTP 1.1 <br>
   GET / background.png HTTP/1.0 <br>
   HEAD /test.html?query=alibaba HTTP/1.1<br>
   OPTIONS /anypage.html HTTP/1.0

   - absolute 형식: 완전한 URL 형식이다. 프록시에 연결하는 경우 대부분 GET과 함께 쓰인다.
   > GET http://developer.mozilla.org/en-US/docs/Web/HTTP/Messages HTTP/1.1

   - authority 형식: 도메인 이름 및 옵션 포트(':'가 앞에 붙는 형태)로 이루어진 URL의 authority component 이다. HTTP 터널을 구축하는 경우에만 CONNECT와 함께 사용할 수 있다.
   > CONNECT developer.mozilla.org:80 HTTP/1.1

   - asterisk 형식: OPTIONS와 함께 별표('*') 하나로 간단하게 서버 전체를 나타낸다.
   > OPTIONS * HTTP/1.1

3. HTTP 버전: 메시지의 남은 구조는 명시된 버전에 따라 달라진다. 또한 응답 메시지에서 써야 할 HTTP 버전을 알려주는 역할을 한다.


### 헤더
HTTP 헤더는 대소문자를 구분하지 않는 이름, 콜론(:), 값으로 구성된다. 값 앞의 공백은 무시된다. <br>
헤더는 값까지 포함해 한 줄로 구성되지만 꽤 길어질 수 있다.<br>
헤더는 아래와 같이 몇몇 그룹으로 나눌 수 있다.<br>

   ![alt text](/images/header.png)<br>
   [출처](https://developer.mozilla.org/ko/docs/Web/HTTP/Messages): https://developer.mozilla.org/ko/docs/Web/HTTP/Messages <br>

Request 헤더: 요청의 내용을 좀 더 구체화하고, 컨텍스트를 제공한다. 조건부로 제한하여 요청 내용을 수정하기도 한다. <br><br>

각 헤더 필드 설명
- Host: 서버의 도메인 이름(포트는 Optional)
- User-Agent: 사용자 에이전트(클라이언트)의 브라우저, 운영 체제, 플랫폼 및 버전
- Accept: 클라이언트가 이해 가능한 (허용하는) 파일 형식 (MIME TYPE)
- Accept-Encoding: 클라이언트가 해석할 수 있는 인코딩, 콘텐츠 압축 방식
- Authorization: 인증 토큰(JWT나 Bearer 토큰)을 서버로 보낼 때 사용하는 헤더, API 요청을 할 때 토큰이 없으면 거절을 당하기 때문에 이 때, Authorization을 사용
- Origin: POST와 같은 요청을 보낼 때, 요청이 어느 주소에서 시작되었는지를 나타냄. 여기서 요청을 보낸 주소와 받는 주소가 다르면 CORS 문제가 발생하기도 함
- Referer: 이 페이지 이전의 페이지 주소가 담겨 있음. 이 헤더를 사용하면 어떤 페이지에서 지금 페이지로 들어왔는지 알 수 있음
- IF-Modified-Since: 최신 버전 페이지 요청을 위한 필드, 요청의 부하를 줄임. 서버는 지정된 날짜 이후에 마지막으로 수정된 경우에만 200 상태 코드로 요청된 리소스를 다시 보냄, 만약 이후에 리소스가 수정되지 않았다면 서버는 본문 없이 304 상태 코드로 응답을 보냄

<br>

General 헤더: 메시지 전체에 적용되는 헤더이다.
- Connection: 현재의 전송이 완료된 후 네트워크 접속을 유지할지 말지를 제어함.<br> keep-alive면, 연결은 지속되고, 동일한 서버에 대한 후속 요청을 수행할 수 있음. <br>
일반적으로 HTTP/1.1을 사용하고 Connection은 기본적으로 keep-alive로 되어있음.
- Via: 요청헤더와 응답헤더에 포워드 프록시와 리버스 프록시에 의해서 추가됨. 포워드 메시지를 추적하거나, 요청 루프 방지, 요청과 응답 체인에 따라 송신자의 프로토콜 정보를 식별함

<br>

Entity 헤더: 요청 본문에 적용되는 헤더이다. 요청 내에 본문이 없는 경우에는 당연히 전송되지 않는다.
- Content-Length: 요청과 응답 메시지의 본문 크기를 바이트 단위로 표시해줌. 메시지 크기에 따라 자동으로 만들어짐


### 본문
본문은 요청의 마지막 부분에 들어가며, 모든 요청에 본문이 들어가지는 않는다. <br>
GET, HEAD, DELETE , OPTIONS처럼 리소스를 가져오는 요청에서는 보통 본문이 필요없다.<br>
일부 요청은 업데이트를 하기 위해 서버에 데이터를 전송한다.<br>
보통 (HTML Form 데이터를 포함하는) POST 요청일 경우 서버에 데이터를 전송한다.

넓게 보면 본문은 두 종류로 나뉜다.

- 단일-리소스 본문(single-resource bodies): 헤더 두 개(Content-Type와 Content-Length)로 정의된 단일 파일로 구성된다.
- 다중-리소스 본문(multiple-resource bodies): multipart 본문으로 구성되며, 파트마다 다른 정보를 포함한다. 보통 HTML Form과 관련이 있다.


## Response
   ![alt text](/images/response.png)<br>
   [출처](https://developer.mozilla.org/ko/docs/Web/HTTP/Messages): https://developer.mozilla.org/ko/docs/Web/HTTP/Messages <br>
   응답(response)은 요청에 대한 서버의 답변 이다.

### 상태 줄
HTTP 응답의 시작 줄은 상태 줄(status line)이라고 말한다. <br>
상태 줄은 아래 세 가지 요소로 구성되어 있다.
> 프로토콜 버전 + 상태 코드 + 상태 텍스트

1. 프로토콜 버전: 보통 HTTP/1.1 이다.
2. 상태 코드: 요청의 성공 또는 실패를 숫자 코드로 나타낸다. 200, 404 혹은 302 등이 있다.
3. 상태 텍스트: 짧고 간결하게 상태 코드에 대한 설명을 글로 나타내어 사람들이 HTTP 메시지를 이해할 때 도움이 됩니다.

상태 줄은 일반적으로 HTTP/1.1 200 OK, HTTP/1.1 404 Not Found. 와 같이 생겼다.

### 헤더
요청의 헤더와 같은 역할.
- HTTP 헤더는 대소문자를 구분하지 않는 이름, 콜론(:), 값으로 구성된다. 값 앞의 공백은 무시된다.
- 헤더는 값까지 포함해 한 줄로 구성되지만 꽤 길어질 수 있다.
- 헤더는 몇몇 그룹으로 나눌 수 있다.


### 본문
본문은 요청의 마지막 부분에 들어가며, 모든 응답에 본문이 들어가지는 않는다. <br>
201, 204과 같은 상태 코드를 가진 응답에는 보통 본문이 없다.<br>
넓게 보면 본문은 세 종류로 나뉜다.<br>

- 이미 길이가 알려진 단일 파일로 구성된, 단일-리소스 본문: 헤더 두개(Content-Type와 Content-Length)로 정의한다.

- 길이를 모르는 단일 파일로 구성된, 단일-리소스 본문: Transfer-Encoding가 chunked로 설정되어 있으며, 파일은 청크로 나뉘어 인코딩 되어 있다.

- 서로 다른 정보를 담고 있는 
multipart로 이루어진 다중 리소스 본문: 이 경우는 위의 두 경우에 비해 상대적으로 보기 힘들다.

[출처](https://velog.io/@bky373/Web-HTTP%EC%99%80-HTTPS-%EC%B4%88%EA%B0%84%EB%8B%A8-%EC%A0%95%EB%A6%AC) : 
https://velog.io/@bky373/Web-HTTP%EC%99%80-HTTPS-%EC%B4%88%EA%B0%84%EB%8B%A8-%EC%A0%95%EB%A6%AC


### CRUD(Create/Read/Update/Delete) <br>
Create: Post 
- 리소스를 생성하거나 컨트롤러를 실행하는 데 사용한다.<br>

Read: Get
- 특정 리소스를 받기 위한 요청이다. 따라서, 리소스의 생성, 수정 및 삭제 등에 사용해서는 안된다

Update: Put
- 변경 가능한 리소스를 업데이트하는 데 사용되며 항상 리소스 식별 정보를 포함해야 한다

Delete: Delete
- 특정 리소스를 제거하는 데 사용한다.<br>
 일반적으로 Request body가 아닌 URI 경로에 제거하려는 리소스의 ID를 전달한다.<br>

 ### PUT VS POST <br>
 멱등성(Idempotence): 멱등성이란 여러번 수행해도 결과가 같음을 의미한다.<br>
 HTTP 메소드를 예를 들자면, GET, PUT, DELETE는 같은 경로로 여러 번 호출해도 결과가 같다.<br>
 그러나 POST는 매 호출마다 새로운 데이터가 추가된다. <br>
 따라서, POST 연산은 결과가 Idempotent하지 않지만, PUT은 반복 수행해도 그 결과가 Idempotent 하다.


 ### HTTP 응답 코드<br>
 200번대: 성공, 이 작업을 성공적으로 받았고, 이해했으며, 받아들여졌다는 의미이다. 
  - 200 OK: 성공적으로 처리했을 때 쓰인다. 가장 일반적으로 볼 수 있는 HTTP 상태
  - 201 Created: 요청이 성공적으로 처리되어서 리소스가 만들어졌음을 의미한다.
  - 204 No Content: 성공적으로 처리했지만 컨텐츠를 제공하지는 않는다. 일반 사용자가 볼 일은 거의 드물며 처리 결과만 중요한 API 요청 등에서 주로 사용한다.
  - 206 Partial Content: 컨텐츠의 일부 부분만 제공한다. 보통 클라이언트에서 시작 범위나 다운로드할 범위를 지정한 경우 자동으로 해당 부분만 제공할 때 사용하는 코드이다.<br><br>

300번대: 리다이렉션, 이 요청을 완료하기 위해서는 리다이렉션이 이루어져야 한다는 의미이다. 짧은 주소(단축 URL) 서비스의 경우 접속 시 301이나 302 코드를 보내고, 헤더의 location에 리다이렉션할 실제 URL을 적어 보낸다.
 - 301 Moved Permanently(영구 이동): 영구적으로 컨텐츠가 이동했을 때 사용된다.
 - 302 Found: 일시적으로 컨텐츠가 이동했을때 사용된다.
 - 304 Not Modified: 200 다음으로 많이 볼 수 있는 HTTP 상태이다. 이 경우 보통 브라우저에 캐시되어 있는 버전을 쓴다.<br><br>

400번대: 클라이언트 오류, 이 요청은 올바르지 않다는 의미이다. 여기서부터 브라우저에 직접 표출된다.
- 400 Bad Request(잘못된 요청): 요청 자체가 잘못되었을 때 사용하는 코드이다.
- 401 Unauthorized(권한 없음): 인증이 필요한 리소스에 인증 없이 접근할 경우 발생한다.<br>
이 응답 코드를 사용할 때에는 반드시 브라우저에 어느 인증 방식을 사용할 것인지 보내야 한다. 단순히 권한이 없는 경우 이 응답 코드 대신 아래 403 Forbidden을 사용해야 한다.
- 403 Forbidden(거부됨): 서버가 요청을 거부할 때 발생한다. <br>
관리자가 해당 사용자를 차단했거나 서버에 index.html 이 없는 경우에도 발생할 수 있다. <br>
혹은 권한이 없을 때(로그인 여부와는 무관하다)에도 발생한다.
- 404 Not Found(찾을 수 없음): 찾는 리소스가 없다는 뜻으로, 가장 흔하게 볼 수 있는 오류 코드이다.<br><br>

500번대: 서버 오류, 서버가 응답할 수 없다는 의미이며, 요청이 올바른지의 여부는 알 수 없다.
- 500 Internal Server Error(내부 서버 오류): 서버에 오류가 발생해 작업을 수행할 수 없을 때 사용된다.<br>
보통 설정이나 퍼미션 문제. 아니면 HTTP 요청을 통해 호출한 문서가 실제 HTML 문서가 아니라 JSP, PHP, 서블릿 등의 프로그램일 경우 그 프로그램이 동작하다 세미콜론을 빼먹는 등의 각종 에러로 비정상 종료를 하는 경우 이 응답코드를 보낸다.
- 502 Bad Gateway(게이트웨이 불량): 게이트웨이가 연결된 서버로부터 잘못된 응답을 받았을 때 사용된다.
- 503 Service Temporarily Unavailable(일시적으로 서비스를 이용할 수 없음): 서비스를 일시적으로 사용할 수 없을 때 사용된다. 주로 웹서버 등이 과부하로 다운되었을 때 볼 수 있다.
- 504 Gateway Timeout(게이트웨이 시간초과): 게이트웨이가 연결된 서버로부터 응답을 받을 수 없었을 때 사용된다.
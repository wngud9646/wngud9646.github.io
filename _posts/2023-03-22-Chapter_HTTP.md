---
title:  "HTTP"
excerpt: "DevOps 부트캠프 Section 1 "

categories:
  - Blog
tags:
  - [Blog, DevOps]

toc: true
toc_sticky: true
 
date: 2023-03-22
last_modified_at: 2023-03-22
---
# Cookie
쿠키(영어: cookie)란 하이퍼 텍스트의 기록서(HTTP)의 일종으로서 인터넷 사용자가 어떠한 웹사이트를 방문할 경우 그 사이트가 사용하고 있는 서버를 통해 인터넷 사용자의 컴퓨터에 설치되는 작은 기록 정보 파일을 일컫는다. <br>
이 기록 파일에 담긴 정보는 인터넷 사용자가 같은 웹사이트를 방문할 때마다 읽히고 수시로 새로운 정보로 바뀐다. <br>
쿠키는 소프트웨어가 아니다. 쿠키는 컴퓨터 내에서 프로그램처럼 실행될 수 없으며 바이러스를 옮길 수도, 악성코드를 설치할 수도 없다. <br>
하지만 스파이웨어를 통해 유저의 브라우징 행동을 추적하는데에 사용될 수 있고, 누군가의 쿠키를 훔쳐서 해당 사용자의 웹 계정 접근권한을 획득할 수도 있다.<br>
 - 위키백과 -

## 쿠키를 사용하는 이유
HTTP 프로토콜의 특징이자 약점을 보완하기 위해서 사용된다.
 1. Connectionless 프로토콜 (비연결지향)<br>
    클라이언트가 서버에 요청(Request)을 했을 때,
    그 요청에 맞는 응답(Response)을 보낸 후 연결을 끊는 처리방식이다.
        - HTTP 1.1 버전에서 연결을 유지하고, 재활용 하는 기능이 Default 로 추가되었다.
          (keep-alive 값으로 변경 가능)

 2. Stateless 프로토콜 (상태정보 유지 안함)<br>
    클라이언트의 상태 정보를 가지지 않는 서버 처리 방식이다.<br>
    클라이언트와 첫번째 통신에서 데이터를 주고 받았다 해도,
    두번째 통신에서 이전 데이터를 유지하지 않는다.<br>
    But, 실제로는 데이터 유지가 필요한 경우가 많다.<br>
    정보가 유지되지 않으면, 매번 페이지를 이동할 때마다 로그인을 다시 하거나, 상품을 선택했는데 구매 페이지에서 선택한 상품의 정보가 없거나 하는 등의 일이 발생할 수 있다.


→ 따라서, Stateless 경우를 대처하기 위해서 쿠키와 세션을 사용한다.<br>
쿠키와 세션의 차이점은 크게 상태 정보의 저장 위치이다.<br>
쿠키는 '클라이언트(=로컬PC)'에 저장하고, 세션은 '서버' 에 저장한다.



## 쿠키의 작동
 ![alt text](/images/cookie.png)<br><br>

위 클라이언트와 서버간의 통신하는 다이어그램을 보자.

클라이언트는 서버와 HTTP프로토콜 통신으로 데이터를 주고 받고 한다.

다이어그램에서 2번을 보면, 서버가 클라이언트로 응답할때 Cookie에 "Superpil"데이터를 포함해서 보내게 된다. (Cookie로 설정된 "Superpil"데이터는 HTTP Response Header에 저장되어 클라이언트에게 전송된다.)

응답을 받은 클라이언트는 "Superpil"데이터를 브라우저의 Cookie에 자체적으로 저장한다.

이후, 클라이언트는 동일한 서버와 재통신 할 경우 쿠키에 저장한 데이터("Superpil")를 함께 서버로 전송 한다.(다이어그램 3번) <br>


## 특징
1. 쿠키는 클라이언트에 저장되며, Key와 Value로 이루어진 데이터 이다.
2. 주로 쿠키는 서버에서 생성하여 Set-Cookie HTTP Response Header에 넣어 클라이언트에게 전달 하고, 클라이언트에서 직접 쿠키에 데이터를 저장 할 수도 있다.
3. 쿠키는 사용자가 별다른 컨트롤 없이 Request시 자동으로 브라우저가 Request Header를 넣어서 서버에 전송한다.
4. 쿠키는 만료기간을 설정할 수 있으며, 만료기간 전 까지 브라우저가 종료 되어도 삭제 되지 않는다. <br><br><br>



## 속성
> key=value; path=경로; expires=만료날짜; domain=도메인; secure; httpOnly

쿠키는 Key=Value쌍으로 구성되고 path, expires 등의 속성을 가진다. 해당 속성들은 ;로 구분된다.<br><br>

👉 key=value <br>
value에 실제 데이터가 담겨져 있다.
key값으로 쿠키의 value값을 조회, 갱신, 삭제를 할 수 있다. 

👉 path <br>
> "super=pil;path=/hello"

특정 경로에서만 쿠키를 활성화 시키는 속성이다. <br>
가령 localhost:8080에서 위와 같이 path=/hello로 cookie를 설정했다면, localhost:8080/hello와 localhost:8080/hello/모든 하위경로 는 cookie가 저장되어 확인 할 수 있지만, 그외 다른 경로(/hello를 제외한)의 경우 cookie를 확인 할 수 없다. <br><br>

👉 domain <br>
쿠키가 전송되어질 서버 도메인을 지정하는 속성 이다. <br>
도메인을 설정할 경우 해당 도메인 및 하위 도메인에서만 쿠키에 접근할 수 있다.<br>
가령, domain에 tistory.com로 설정할 경우 skin.tistory.com에서도 쿠키 정보에 접근이 가능하다. <br>
반면 domain을 설정하지 않을 경우 tistory.com에서만 쿠키에 접근 가능하다.

👉 expires<br>
쿠키의 만료기간을 의미하며 만료기간이 지난경우 자동 삭제된다.<br>
반대로 만료기간 전까지는 삭제되지 않는다.(직접 쿠키를 삭제하는 경우는 제외)
 

👉 secure<br>
secure속성을 설정하면 HTTPS통신이 아닌경우 서버로 쿠키를 전송하지 않는다.<br>
HTTP통신을 하게 되면 쿠키를 암호화 하지 않은 상태로 서버로 전달하기 때문에 보안상 secure속성을 설정하는게 좋을 것 같다.

👉 httpOnly<br>
저장된 쿠키는 서버와 통신할때만 정보를 확인 할 수 있으며, 자바스크립트로 document.cookie에 접근하여 쿠키 정보를 조회 할 수 없다.<br><br>

### CSRF 문제
쿠키에 별도로 설정을 가하지 않는다면, 크롬을 제외한 브라우저들은 모든 HTTP 요청에 대해서 쿠키를 전송하게 됩니다. 그 요청에는 HTML 문서 요청, HTML 문서에 포함된 이미지 요청, XHR 혹은 Form을 이용한 HTTP 요청등 모든 요청이 포함됩니다.

CSRF(Cross Site Request Forgery)는 이 문제를 노린 공격입니다. 간단히 소개해보자면 아래와 같은 방식입니다.

1. 공격대상 사이트는 쿠키로 사용자 인증을 수행함.
2. 피해자는 공격 대상 사이트에 이미 로그인 되어있어서 브라우저에 쿠키가 있는 상태.
3. 공격자는 피해자에게 그럴듯한 사이트 링크를 전송하고 누르게 함. (공격대상 사이트와 다른 도메인)
4. 링크를 누르면 HTML 문서가 열리는데, 이 문서는 공격 대상 사이트에 HTTP 요청을 보냄.
5. 이 요청에는 쿠키가 포함(서드 파티 쿠키)되어 있으므로 공격자가 유도한 동작을 실행할 수 있음.
<br>

👉 SameSite <br>
SameSite 쿠키는 앞서 언급한 서드 파티 쿠키의 보안적 문제를 해결하기 위해 만들어진 기술이다. <br>
크로스 사이트(Cross-site)로 전송하는 요청의 경우 쿠키의 전송에 제한을 두도록 한다.

SameSite 쿠키의 정책으로 None, Lax, Strict 세 가지 종류를 선택할 수 있고, 각각 동작하는 방식이 다르다.

- None: SameSite 가 탄생하기 전 쿠키와 동작하는 방식이 같습니다.<br>
None으로 설정된 쿠키의 경우 크로스 사이트 요청의 경우에도 항상 전송됩니다.<br> 
즉, 서드 파티 쿠키도 전송됩니다. <br>따라서, 보안적으로도 SameSite 적용을 하지 않은 쿠키와 마찬가지로 문제가 있는 방식입니다.
- Strict: 가장 보수적인 정책입니다.<br> 
Strict로 설정된 쿠키는 크로스 사이트 요청에는 항상 전송되지 않습니다.<br>
즉, 서드 파티 쿠키는 전송되지 않고, 퍼스트 파티 쿠키만 전송됩니다.<br>
- Lax: Strict에 비해 상대적으로 느슨한 정책입니다. <br>
Lax로 설정된 경우, 대체로 서드 파티 쿠키는 전송되지 않지만, 몇 가지 예외적인 요청에는 전송됩니다. <br>
=> Lax 쿠키가 전송되는 경우<br>
같은 웹 사이트일 때는 (당연히) 전송된다는 것이고, 이 외에는 Top Level Navigation(웹 페이지 이동)과, "안전한" HTTP 메서드 요청의 경우 전송된다는 것입니다.

SameSite 속성으로 None을 사용하려면 반드시 해당 쿠키는 Secure 쿠키여야 합니다.<br> 
Secure 쿠키는 HTTPS가 적용된 요청에만 전송되는 쿠키입니다.<br> 
이 정책을 구현하는 브라우저도 현재로서는 크롬밖에 없습니다. 그래서 크롬에서는 SameSite=None으로 Set-Cookie를 사용하면 다음과 같이 쿠키 자체가 제대로 설정되지 않습니다. <br><br>

[출처](https://pygmalion0220.tistory.com/entry/HTTP-Cookie-%EA%B8%B0%EB%B3%B8-%EA%B0%9C%EB%85%90) : https://pygmalion0220.tistory.com/entry/HTTP-Cookie-%EA%B8%B0%EB%B3%B8-%EA%B0%9C%EB%85%90 <br>
[출처](https://seob.dev/posts/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80-%EC%BF%A0%ED%82%A4%EC%99%80-SameSite-%EC%86%8D%EC%84%B1/) : https://seob.dev/posts/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80-%EC%BF%A0%ED%82%A4%EC%99%80-SameSite-%EC%86%8D%EC%84%B1/

<br><br>

# HTTP 헤더
1. 공통헤더<br>
요청과 응답에 모두 사용되는 헤더

1.1 Date<br>
HTTP 메시지가 만들어진 시각입니다. 자동으로 만들어집니다.<br>
ex) Date: Sun, 13 Jan 2019 17:28:13 GMT


1.2 Connection<br>
일반적으로 HTTP/1.1을 사용하며 Connection은 기본적으로 keep-alive로 되어있다. 별 의미 없는 헤더.<br>
ex) Connection: keep-alive


1.3 Content-Length<br>
요청과 응답 메시지의 본문 크기를 바이트 단위로 표시해준다. 메시지 크기에 따라 자동으로 만들어진다.<br>
ex) Content-Length: 88052


1.4 Cache-Control<br>
HTTP 에서 캐시 메커니즘을 지정하기 위해 사용되는 헤더이다. <br>
웹 뷰에서, 일반 모바일 브라우저에서 304라는 응답 코드를 주면 이녀석을 의심 해보자.


1.5 Content-Type <br>
컨텐츠의 타입(MIME)과 문자열 인코딩(utf-8 등등)을 명시할 수 있다.<br>
ex) Content-Type: text/html; charset=utf-8 (현재 메시지 내용이 text/html 타입이고 문자열은 utf-8 문자열)
서버로 데이터를 보낼 때는 text/html 대신 www-url-form-encoded, multipart/form-data 등이 Content-Type이 된다.



1.6 Content-Language
사용자의 언어. <br>
ex) Content-Language=ja
(http://jpn.lottedfs.com/kr/display/category/third?dispShopNo1=1300091&dispShopNo2=1300092&dispShopNo3=1300093&treDpth=3)<br>
롯데 면세 일본어 버전으로 접속시 다음과 같은 헤더정보를 볼 수 있다.


1.7 Content-Encoding<br>
응답 컨텐츠를 br, gzip, deflate 등의 알고리즘으로 압축해서 보내면, 브라우저가 알아서 해제해서 사용한다.<br>
이 외에도 다양한 압축 알고리즘이 존재한다.<br>
요청이나 응답 전송 속도도 빨라지고, 데이터 소모량도 줄어들기 때문에 사용한다.<br>
ex) Content-Encoding: gzip, deflate
Content-Encoding은 컨텐츠 압축된 방식입니다. 


2. 요청헤더<br>
2.1 Host<br>
서버의 도메인 네임 <br>
Host 헤더는 반드시 하나가 존재해야 한다.<br>
EX) host: goddaehee.tistory.com


2.2 User-Agent<br>
가장 흔하게 보고 사용하는 헤더이다. 
현재 사용자가 어떤 클라이언트(운영체제, 앱, 브라우저 등)를 통해 요청을 보냈는지 알 수 있다.<br>
ex) User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36<br>
Window로 크롬 브라우저를 통해 접속 하였다. 다만 헤더는 변경 할 수 있기 때문에 무조건 신뢰할 수는 없다.<br>
그래도 매우 유용한 헤더이며 이를 활용하여 접속자 및 사용기기 통계를 내기도 한다.


2.3 Accept<br>
클라이언트가 허용할 수 있는 파일 형식(MIME TYPE)<br>
ex1) Accept: text/html (HTML 형식인 응답을 처리하겠다)<br>
ex2) Accept: image/png, image/gif<br>
ex3) Accept: text/*<br>
콤마로 여러 타입을 동시에 적어줄 수도 있고, *(와일드카드)로 텍스트면 ok라고 요청할 수 있다.<br>
위에 명시하였던 Content 헤더와 비교하여 보자.<br>
Accept로 원하는 형식을 보내고, 서버가 응답하면서 헤더의 Content를 설정하게된다.<br>
ex4)
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Accept-Encoding: gzip, deflate
Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7

2.4 Cookie<br>
웹서버가 클라이언트에 쿠키를 저장해 놓았다면 해당 쿠키의 정보를 이름-값 쌍으로 웹서버에 전송한다.

2.5 Origin<br>
POST같은 요청을 보낼 때, 요청이 어느 주소에서 시작되었는지를 나타내는데 이때 보낸 주소와 받는 주소가 다르면 CORS 문제가 발생하기도 한다.<br>
ex) Origin : http://goddaehee.tistory.com <br>
ex) Referer: http://goddaehee.tistory.com

2.6 If-Modified-Since<br>
페이지가 수정되었으면 최신 버전 페이지 요청을 위한 필드 <br>
만일 요청한 파일이 이 필드에 지정된 시간 이후로 변경되지 않았다면, 서버로부터 데이터를 전송받지 않는다.


2.7 Authorization <br>
Authorization 헤더는 인증 토큰을 서버로 보낼 때 사용하는 헤더.<br>
API 요청같은 것을 할 때 토큰이 없으면 거절당하기 때문에 이 때, Authorization을 사용한다.<br>
JWT(Json Web Token) 을 사용한 인증에서 주로 사용 한다.<br>
ex) Authorization: Bearer xxx.xxx.xxx


3. 응답헤더<br>
3.1 Server<br>
웹서버 정보를 나타낸다.

3.2 Access-Control-Allow-Origin<br>
요청 Host와 응답 Host가 다르면 CORS 에러가 발생하는데 서버에서 응답 메시지 Access-Control-Allow-Origin 헤더에 프론트 주소를 적어주면 에러가 발생하지 않는다.<br>
ex) Access-Control-Allow-Origin: goddaehee.tistory.com

관련 헤더
 - Access-Control-Request-Method : 실제로 보내려는 메서드
 - Access-Control-Request-Headers : 실제로 보내려는 헤더
 - Access-Control-Allow-Methods : 서버가 허용하는 메서드
 - Access-Control-Allow-Headers : 서버가 허용하는 헤더

3.3 Allow<br>
Allow 헤더는 CORS 요청 외에도 적용된다.<br>
ex) Allow: GET<br>
위와 같은 경우는 GET 요청만 받는다. POST 와 같은 경우는 405 Method Not Allowed 에러 Return 한다.

3.4 Content-Disposition<br>
응답 본문을 브라우저가 어떻게 표시해야 할지 알려주는 헤더.<br>
 - inline : 웹페이지 화면에 표시
 - attachment : 다운로드

ex) Content-Disposition: inline<br>
ex) Content-Disposition: attachment; filename='filename.json'<br>
filename 옵션으로 파일명 지정도 가능하다.

3.5 Location<br>
300번대 응답이나 201 Created 응답일 때 어느 페이지로 이동할지를 알려주는 헤더

ex ) HTTP/1.1 302 Found
     Location: /login<br>
위와 같은 응답시 브라우저는 /login 주소로 리다이렉트한다.

3.6 Content-Security-Policy<br>
다른 외부 파일들을 불러오는 경우, 차단할 소스와 불러올 소스를 여기에 명시할 수 있다.<br>
하나의 웹 페이지는 다양한 외부 소스들을 불러온다. (이미지, JS, CSS, 폰트, 아이프레임 등) 

ex) Content-Security-Policy: default-src 'self' (자신의 도메인의 파일들만 가져올 수 있다)<br>
Content-Security-Policy: default-src https: (https를 통해서만 파일을 가져올 수 있다)<br>
Content-Security-Policy: default-src 'none' (못가져 오게 만듬)<br>
https:로 지정하면 https를 통해서만 파일을 가져올 수 있게 됩니다. 'none'으로 지정하면 가져올 수 없습니다.

[출처](https://goddaehee.tistory.com/169) : https://goddaehee.tistory.com/169


## 발표
Request URL은 요청한 URL로 https://toss.im/ 으로 Request 하였다.<br>

Request Method를 통해, 사용된 HTTP 메소드는 GET이며,

Status Code에서 성공적으로 요청되었다는 의미의 200을 확인 가능. <br>

Remote Address는 클라이언트의 IP 주소이다. 211.115.96.201에 포트번호 443을 사용하는 것을 알 수 있다. <br>

http 요청은 옵셔널 헤더인 Referer를 가지고 있을 수 있다. 이 정보는 이 요청이 만들어진 origin 또는 웹페이지 URL을 가리킨다. Referrer-Policy헤더는 요청과 함께 얼마나 많은 referer 정보를 포함해야 하는지 알려준다. <br>
strict-origin-when-cross-origin은
동일 origin request일 때, origin, path, querystring을 보내주고, cross-origin request에서는 프로토콜 보안이 동일한 경우에만 origin을 송신한다.(HTTPS->HTTPS) <br>
보안이 취약한 프로토콜로 송신할 때는 referer 헤더를 보내지 않는다. <br><br>

Response Header
Connection: keep-alive, 최소 특정 시간동안(timeout) 최대 요청 request(max)의 수를 알려줄 수 있다.<br>
아래의 Keep-Alive: timeout=60과 같이 보면, 60초 동안 연결을 유지한다는 것을 알 수 있다.<br>

content-encoding: gzip, Content-Encoding 헤더 값에 gzip을 명시하여, 컨텐츠가 gzip 알고리즘에 의해 압축되었음을 브라우저에게 알리고 있다.

Content-Type: text/html; charset=utf-8
자원의 형식을 명시하기 위해 헤더에 실리는 정보이다. 해당 header는
타입 :text, 서브타입 : html, 문자셋 : UTF-8을 의미한다.<br>

Date: 해당 Response가 송신된 시간<br>
2022/01/27 목요일 03:17:53<br>

key-event-id: ? 해당 내용은 정확히 파악 안됨 <br>

Server: nginx <br>
해당 웹서버에 사용되는 소프트웨어 <br>

Transfer-Encoding: chunked <br>
청크 인코딩 (메시지를 일정 크기의 청크 여럿으로 쪼개며 서버는 각 청크를 순차적으로 보낸다).<br>

vary: Origin,Access-Control-Request-Method,Access-Control-Request-Headers 
- vary - 오리진 서버로부터 새로운 요청을 하는 대신 캐시된 응답을 사용할지를 결정하기 위한 
향후의 요청 헤더를 매칭할 방법을 정함.
- ACRM - 실제 요청이 일어나는 경우 어떤 HTTP 메서드가 사용 될 것인지 서버에 알리는 용도로 사용 (실제 요청시에는 위 헤더는 포함하지 않는다)
- ACRH - 실제 요청이 일어나는 경우 어떤 요청 헤드가 사용될 것인지 서버에 알리는 용도 (실제 요청시에는 위 헤더는 포함하지 않는다.)

x-envoy-upstream-service-time: 8<br> 
요청 서버에 대한 모든 재시도에 소요된 시간의 합계입니다. 8 밀리초<br>

X-Frame-Options: SAMEORIGIN <br>
도메인 내에서만 iframe (다른 HTML 페이지를 현재 페이지에 포함시키는 중첩된 브라우저로 iframe 요소를 이용하면 해당 웹 페이지 안에 어떠한 제한 없이 다른 페이지를 불러와서 삽입) 접근 가능


Request Header
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/ *;q=0.8,application/signed-exchange;v=b3;q=0.9 <br>
클라이언트가 처리가능한 미디어 타입으로 전달(돌려준다) => 여기서 우선순위를 주어 선호하는 미디어 타입을 보낼 수 있다<br>

Accept-Encoding: gzip, deflate, br  클라이언트가 디코딩 가능한 압축 방식

Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7 
클라이언트가 지원하는 자연 언어 우선순위는 1.한국어 2.영어

Cache-Control: no-cache <br>
클라이언트가 이전에 받은 데이터와 새로 요청한 데이터가 같다면 캐시를 통해 빠른 응답을 받을 수 있다.  Cache-Control은 이러한 캐시를 다루는 Header이다.<br>
no-cache : 캐시는 저장하지만 사용하려고 할 때마다 서버에 재검증 요청을 보내야 합니다.

Connection: 요청이 끝난 후 클라이언트와 서버간의 네트워크 커넥션을 유지할 것인지 끊을 것인지. Keep-alive이므로 유지된다.

Cookie: Set-Cookie 헤더와 함께 서버에 의해 전송되어 저장된 HTTP Cookies를 포함

Host: 서버의 도메인 명과 서버의 TCP 포트 번호

Pragma : 오래된 클라이언트에 대한 옵션 낮은 버전의 HTTP 1.0을 위함 <br>
no-cache : 캐시는 저장하지만 사용하려고 할 때마다 서버에 재검증 요청을 보내야 합니다.

Sec-CH-UA: 쉼표로 구분된 목록에서 브라우저와 관련된 각 브랜드에 대한 브랜드 및 중요한 버전을 제공한다.


Sec-CH-UA-Mobile: 브라우저가 모바일 디바이스에서 동작하는지 여부를 뜻한다. <br>
Sec-CH-UA-Mobile: <boolean>
?0이므로 모바일 환경이 아님을 뜻한다.
---
title:  "HTTP_API 디자인"
excerpt: "DevOps 부트캠프 Section 1 "

categories:
  - Blog
tags:
  - [Blog, DevOps]

toc: true
toc_sticky: true
 
date: 2023-03-23
last_modified_at: 2023-03-24
---
# API 디자인
REST API는 데이터나 자원(resource)을 HTTP URI로 표현하는 데에 그 목적이 있습니다. 따라서 API 작성에 앞서 다음 과정이 선행됩니다.

어떤 리소스를 요청/응답으로 주고 받을 것인가?
해당 리소스에는 어떤 내용을 포함하는가?

즉, 전달 과정에서 필요한 데이터를 디자인하는 과정은 큰 틀에서 데이터 모델링의 한 부분이며, 여러 개의 표의 형식으로 정의하는 것이 관계형 데이터 모델링이다.

- 관계형 데이터베이스<br>
관계형 데이터베이스(RDB)는 테이블, 행, 열의 정보를 구조화하는 방식입니다. RDB에는 테이블을 조인하여 정보 간 관계 또는 링크를 설정할 수 있는 기능이 있어, 여러 데이터 포인트 간의 관계를 쉽게 이해하고 정보를 얻을 수 있습니다. 

데이터가 HTTP 프로토콜을 통해 전달되려면, 표를 문자열로만 표현해야 합니다. HTTP 본문(body)은 문자열로만 이루어져 있기 때문입니다.  <br>
=> 이와 같이, 데이터가 어떠한 정해진 문자열 형태로 변환되는 과정을 직렬화(serialize)라고 합니다.


<br><br>

## 5가지 기본 REST API 설계 지침
견고하고 강력한 디자인은 API 성공의 핵심 요소이다.<br>
잘못 설계된 API는 오용으로 이어지거나, 전혀 사용되지 않을 수 있다.<br>
다음은 RESTful API를 만드는 5가지 기본 설계 지침이다.<br>
리소스 (URI) → 동사아닌 메소드 + 구체적 이름 (척추 케이스)
HTTP 메소드 → 메소드를 적절하게 사용
HTTP 헤더 → 정보를 올바르게 구성
쿼리 매개 변수
상태 코드 -> 올바르게 구성


1. 리소스 (URI)<br>
이름과 동사<br>
리소스 설명 시 동작 동사가 아닌 구체적인 이름을 사용한다.
- RPC 방식 : 지난 수십 년 동안 동작 동사를 사용했다.<br>
getUser(1234) createUser(user) deleteAddress(1234)

- RESTful 방식 : 동적 동사 대신 구체적인 이름을 사용한다.<br>
GET /users/1234 POST /user (body에 사용자를 설명하는 JSON 포함) DELETE /address/1234<br><br>
URL 케이스<br>
프로그램 ‘리소스 이름’을 만드는 ‘세 가지 문자 규칙 유형’이 있다.<br>
CamelCase (낙타 케이스)
  - Java 언어로 대중화되었다.<br>
    대소 문자로 구분하지 않는 컨텍스트에서 비효율적이다.

  snake_case (뱀 케이스)<br>
  - C 프로그래머가 수년 동안 널리 사용했다.<br>
  - 언더바(_)는 컴파일러나 인터프리터가 기호로 이해할 수 있다.<br>
  - 사용할 수 없는 컨텍스트는 거의 없다.

  spinal-case (척추 케이스)
  - 일부 언어에서 (변수,클래스,함수) 이름에 하이픈을 허용하지 않는다.
  - lisp-case라고도 한다.
  - UNIX, Linux 시스템에서 폴더 및 파일 이름을 지정하는 전통적인 방법이다.

  정리하자면
  - RFC3986에 따르면 “URL은 대소 문자를 구분한다”고 한다. (scheme과 host 제외)
  - 그러나 실제로 Windows 시스템에서 호스팅되는 API로 인해 장애를 일으킬 수 있다.

<br>

2. HTTP 메소드<br>
- REST 접근 방식의 주요 목표는 API로 직접 요청 방식을 만들지 않고 HTTP를 애플리케이션 프로토콜을 사용하는 것이다.
- HTTP 메소드를 사용해서 리소스에서 수행되는 작업을 설명해야한다.
- 이것은 반복적인 CRUD 작업을 처리하는 프론트 개발자에게 용의하다.
- 참고 : HTTP 요청 메서드 – HTTP | MDN

  GET<br>
  - GET은 주어진 URI를 사용하여 주어진 서버에서 정보를 검색하는데 사용된다.
  - GET 요청은 데이터만 검색해야하며, 데이터에 영향을 주지 않아야한다.<br>
  
  HEAD
  - GET과 동일하지만 상태 줄 및 헤더 섹션만 전송한다.

  POST
  - POST 요청은 데이터를 서버로 보내는데 사용된다.

  PUT
  - 대상 리소스의 모든 내용을 대체한다.

  PATCH
  - 대상 리소스의 일부 내용을 변경한다.

  DELETE
  - 대상 리소스의 모든 내용을 삭제한다.

  OPTIONS
  - 대상 리소스에 대한 통신 옵션을 설정한다.

<br>

3. HTTP 헤더<br>
HTTP_Request_Headers

- HTTP 헤더 필드는 요청 및 응답에 대한 필수 정보 또는 메세지 바디의 컨텐츠 정보를 표시한다.
- HTTP 메세지 헤더에는 4가지 유형이 있다.
- 참고 : 헤더 – 용어 사전 | MDN

  일반 헤더 (General Header)
  - 요청 및 응답 메세지 모두에서 일반적으로 적용 가능하다.
  - 바디에서 최종적으로 전송되는 데이터와는 관련이 없는 헤더이다.

  클라이언트 요청 헤더 (Client Request Header)
  - Only 요청 메세지에만 적용 가능하다.
  - 페치될 리소스나 클라이언트 자체에 대한 자세한 정보를 포함한다.

  서버 응답 헤서 (Server Response Header)
  - Only 응답 메세지에만 적용 가능하다.
  - 위치 또는 서버 자체에 대한 정보(이름, 버전 등)와 같이 응답에 대한 부가적인 정보를 갖는 헤더이다.
  엔티티 헤더 (Entity Header)
  - HTTP 요청 및 응답 모두에서 사용된다.
  - 메세지 바디의 컨텐츠 또는 요청 받은 리소스에 대한 정보를 포함한다.
  - Content-Length Content-Language Content-Encoding
  - MIME 유형 같은 정보를 포함한다. Content-Type
  - 참고 : 엔티티 헤더 – 용어 사전 | MDN

<br>

4. 쿼리 매개 변수<br>
페이징 (Paging)
  - API의 초기 디자인 단계에서 리소스 페이징을 예상해야한다.
  - 호출 클라이언트가 반환 값을 제공하지 않을 경우 기본 값으로 페이지를 매기는 것이 좋다. (0-25)
필터링 (Filtering)
- 필터링은 일부 속성과 예상 값을 지정하여 쿼리된 리소스 수를 제한하는 것으로 구성된다.
- 동시에 여러 속성에 대한 컬렉션을 필터링하고 하나의 필터링된 속성에 여러 값을 허용할 수 있다.
정렬 (Sorting)
- 리소스 모음에 대한 쿼리 결과를 정렬한다.
- 정렬 매개 변수에는 정렬이 수행되는 속성의 이름이 쉼표로 구분되어 있어야 한다.
검색 (Searching)
- 검색은 컬렉션의 하위 리소스이다.
- 따라서 결과는 리소스 및 컬렉션 자체와 다른 형식을 갖는다.
- 이를 통해 검색과 관련된 제안, 수정 및 정보를 추가 할 수 있다.
- 매개 변수는 쿼리 문자열을 통해 필터와 동일한 방식으로 제공되지만 반드시 정확한 값은 아니며 구문이 근사 일치를 허용한다.

<br><br>

5. 상태 코드<br>
응답할 때 적절한 HTTP 상태 코드를 사용하는 것은 REASTful API에서 매우 중요하다.<br>
참고 : HTTP 상태 코드 – HTTP | MDN<br>
다음은 가장 많이 사용되는 상태 코드이다.<br>

    200 – 확인
    - OK 모든 것이 정상 작동한다.

    201 – 생성됨
    - 새로운 리소스가 생성되었다.

    204 – 콘텐츠 없음
    - 리소스가 성공적으로 삭제되었다. (응답 본문에 없음)

    304 – 수정되지 않음
    - 반환 된 날짜는 캐시 된 데이터 이다. (데이터는 변경되지 않음)

    400 – 잘못된 요청
    - 요청이 잘못되었거나 처리 할 수 없다.
    - 정확한 오류는 오류 페이지에 설명되어야한다. (예: JSON이 유효하지 않습니다.)

    401 – 미확인
    - 요청에는 사용자 인증이 필요하다.

    403 – 금지
    - 서버가 요청을 이해했지만 거부하거나 액세스가 허용되지 않는다.

    404 – 찾을 수 없음
    - URI 뒤에는 리소스가 없다.

    500 – 내부 서버 오류
    - API 개발자는 이 오류를 피해야한다.
    - 이 오류가 발생하면 스택 추적이 기록되고 응답으로 반환되지 않아야한다.

    <br><br>

요약 <br>
URI는 정보의 자원을 표현한다.<br>
자원에 대한 상태 정의는 HTTP method(GET, POST, PUT, DELETE…)로 표현한다.<br>
예를 들어, GET /user/1122/post라는 URI는 REST API에 부합하지 않는다.<br>
POST /user/1122처럼 표현하는 것이 REST API 규칙에 부합한다.<br><br>

- REST는 새로운 것이 아니다.
- REST는 초기 인터넷 표준, URI 및 HTTP에 대한 강조를 통해
대형 애플리케이션 서버 시대 이전의 웹 방식으로 돌아오게 하는 것이다.
- 리소스 모델링은 API 디자인이 최고의 API 클라이언트 상호 작용 경험을 제공하도록 한다.
- 앞서 논의한 다양한 접근 방식을 기반으로
비즈니스 요구 사항, 기술적 고려 사항(깨끗한 디자인, 유지 관리 가능성 등) 및 비용-편익 분석을 고려해 신중한 개발이 필요하다.

Instagram 그래프 API<br>
GET /{ig-comment-id}?fields={fields}

요청구문: <br>
GET https://graph.facebook.com/{api-version}/{ig-comment-id}
  ?fields={fields}
  &access_token={access-token}

경로 매개변수<br>
{api-version}: API 버전<br>
{ig-comment-id}: 필수 항목. IG 댓글 ID.

쿼리 문자열 매개변수<br>
access_token: {access-token}, 필수 항목. 앱 사용자의 사용자 액세스 토큰.(required?)
fields: {fields}, 결과 세트에서 각 IG에 반환하고자 하는 IG 댓글 필드의 쉼표로 구분된 리스트.

cURL 예시 <br>
요청
```
curl -i -X GET \
 "https://graph.facebook.com/v16.0/17881770991003328?fields=hidden%2Cmedia%2Ctimestamp&access_token=EAAOc..."
```

응답
```
{
  "hidden": false,
  "media": {
    "id": "17856134461174448"
  },
  "timestamp": "2017-05-19T23:27:28+0000",
  "id": "17881770991003328"
}
```


블로그 글 좋아요: PUT
블로그 글 좋아요 취소: PUT
다른 글쓴이 팔로우: PUT

|bloglike|user|
|------|---|
|false|test|
|true|lee|
|false|Joo|


이런 단순한 데이터 모델이 있고, 테이블에서 bloglike 파라미터가 좋아요를 판별하는 값이라면, 이를 put으로 수정한다면 좋아요를 누르고, 취소할 수 있을 것이라고 생각된다.
팔로우 역시 단순한 기능적으로는 PUT을 사용해 데이터를 수정하는 방법이 가장 적합할 것 이라고 생각된다.

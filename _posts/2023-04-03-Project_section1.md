---
title:  "Secton1 Project"
excerpt: "DevOps 부트캠프 Section 1 "

categories:
  - Blog
tags:
  - [Blog, DevOps]

toc: true
toc_sticky: true
 
date: 2023-04-03
last_modified_at: 2023-04-05
---
# Project 개요
### [Project_link](https://github.com/cs-devops-bootcamp/Devops-04-S1-Team6-1): https://github.com/cs-devops-bootcamp/Devops-04-S1-Team6-1

## LMS
필요 기능 <br>
- 사용자는 모든 수업을 조회할 수 있다
- 사용자는 특정 분류의 수업을 조회할 수 있다(강의자, 수업명, 수업분류)
- 사용자는 수업을 수강신청 할 수 있다
- 사용자는 모든 수강중인 수업을 조회할 수 있다
- 사용자의 타입이 강의자일 경우 새로운 수업을 생성할 수 있다
- 사용자의 타입이 강의자일 경우 수업의 정보를 변경할 수 있다
- 사용자는 수업에 대한 수강신청을 취소 할 수 있다


## API 명세
## 선정 주제 : LMS
---
## API 명세
![alt text](/images/api_%EB%AA%85%EC%84%B8.png)
---
list-of-course
```
{
  "course": [
    {
      "id": 4,
      "course_name": "테니스",
      "instructor_id": 3,
      "course_type": "스포츠"
    },
    {
      "id": 5,
      "course_name": "축구"
      "instructor_id": 2.
      "course_type": "스포츠"
    }
  ]
}
```

list-of-registration
```
{
  "registration": [
    {
      "registration_id": 1,
      "student_id": 2,
      "course_id": 4
    }
  ]
}
```

<br><br>


## Project 회고
새로운 도전이였다. <br>
기존에 개인적으로 프론트엔드 공부를 했던 적이 있었는데, 이번 프로젝트는 백엔드의 관련된 작업이였다. <br>
fastify를 이용하여 서버를 만들고, javascript로 기능들을 코딩하는 프로젝트였다. <br><br>

이번 프로젝트는 거의 코딩이 대부분을 차지해서, 같이 협업하는 팀원들은 코딩 경험이 많지 않아서 어려움을 겪었다.<br>
실제로 코딩을 경험해본 사람들이 좀 더 편하게 접근할 수 있는 프로젝트라는 생각이 들긴 했다.<br><br>

프로젝트를 시간의 흐름에 따라 회고해 보며, 관련 내용을 작성하도록 한다.<br><br>

### 주제 분석 및 ERD 제작
가장 먼저 해당 주제에 대한 분석을 시작하였다.<br>
주제 분석은 시작이며, 데이터베이스 설계에 꼭 필요한 부분이다.
주제 분석을 통하여 만든 데이터베이스의 ERD는
![alt text](/images/ERD.png)
이러한 구조를 가지고 있다.<br>

ERD 구조를 제작하면서 논의점은
1. 원래는 registration 테이블에는 name 데이터를 넣으려 하였으나, registration은 수강신청 정보가 들어가는 테이블인데 여기에는 이름은 필요없다고 판단되어 제거하였다.

2. 수강 중, 수강 중이 아닌것의 판단은 어떻게 할것인가?<br>
이것은 판단 근거가 명확하게 존재하지 않는다고 판단하였고, 수강 신청 시에 해당 강좌를 수강하는 상태이고, 신청 취소 혹은 신청이 안되어있을 시에는 해당 강좌를 미수강하는 상태라고 상황을 설정하였다.

3. 처음 설계시에는 User 테이블에 Student와 instructor 정보를 모두 담으려고 하였으나, 두 테이블을 이분화하여 분리하였다. <br>
이유는 학사 정보 시스템 특성 상, 강사 정보가 다수의 수강생 정보와 같은 User Resource Pool 에 존재할 시 강사 정보 검색에 비효율 존재, 새로운 Resource Table로 나누어 검색하는 것이 성능적으로 우수하다고 판단하였기 때문이다.


4. Course 테이블을 처음에 설계할 때, instructor_name도 instructor 테이블에서 외래키로 가져오게 데이터에 넣어놓았으나, NoSQL적 성격을 띄게 될것이라는 조언을 듣고, 해당 데이터필드를 instructor_id를 외래키로 가져오게 설정하였다. <br>
이로써 SQL의 특징을 가지고 설계하게 되었다.

위의 논의점들을 토대로 수정을 거쳐 위의 그림의 ERD를 완성하게 된다. 이를 토대로 API 명세를 작성하게 된다.<br><br>


### API 명세 작성
명세를 작성하기 이전에 해당 API의 기능들이 요구하는 부분들을 좀 더 분석하였다.

- 사용자는 모든 수업을 조회할 수 있다. **(R)**: <br>
사용자 (Student,Instructor)는 Get  Method를 통해 수업(Course) Data를 불러올 수 있다.
- 사용자는 특정 분류의 수업을 조회할 수 있다. **(R)** **강의자, 수업명, 수업분류로 조회.*:
<br>
사용자 (Student, Instructor)는 GET Method로 특정 값(강의자, 수업명, 수업분류)을 기준으로 해당 값과 연관된 Data를 불러올 수 있다.

- 사용자는 수업을 수강신청할 수 있다. **(C)**: <br> 사용자 (Student) 는 Post Method로 Registration에 Data를 생성할 수 있다.
- 사용자는 모든 수강중인 수업을 조회할 수 있다. **(R)**:<br> 
사용자 (Student)는 GET Method로 본인이 수강한 수업 정보를 조회할 수 있다.(/student_id , /registration)
- 사용자의 타입이 강의자일 경우 새로운 수업을 생성할 수 있다. **(C)**:<br> 
사용자 (Instructor)는 Post Method로 Course Table에 Data를 생성할 수 있다.
- 사용자의 타입이 강의자일 경우 수업의 정보를 변경할 수 있다. **(U)**:<br>
사용자 (Instructor)는 Put Method로 본인이 생성한 수업 내용을 변경할 수 있다.
- 사용자는 수업에 대한 수강신청을 취소할 수 있다. **(D)**:<br> 
사용자 (Student)는 Delete Method로 본인이 신청한 수업을 취소할 수 있다.

<br><br> 

API 명세서를 작성하면서의 논의점은
1. 큰 메소드들은 어떻게 쓸지 이미 분석하면서 설정을 하였기에 세부적인 기능들을 설정하는 논의를 진행

2. 그 중에서 아무나 데이터들을 CRUD하면 안되기 때문에 이를 방지하기 위한 Request Header Authorization을 추가하기로 하였다.

3. 수강정보를 받아오는 메소드 GET에서 특정 조건들의 경우 어떤 방식으로 가져오게 할 것인가에 대해 논의하였다.<br> 
처음에는 params 방식으로 하고자 하였으나, params 보다는 다른 방식으로 path에 querystring을 사용하고, request body에서 search 키워드를 통해 조건에 맞는 내용들을 검색하기로 하였다.

4. 수강정보를 조회할때, 모든 수강정보를 받아오는게 아닌, 해당 정보를 조회하는 학생의 수강정보만 받아오는것이 기능적으로 알맞다고 생각하여, request header에 사용자 정의 header로 student_id를 넣기로 하였다.

이렇게 명세작성을 완료하고 명세에 따라 실제 기능들을 구현하기 시작하였다.
<br> <br> 

## 실제 구현
실제 구현에 대한 회고에서는 구현하면서 발생한 변경점이나, 어려운 점 등에 대해서 다루어보려한다.

#### 데이터베이스 실제 생성
테이블을 생성, 수정, 삭제할때 실제 사용했던 query들.<br> 
해당 쿼리들은 ERD의 구조에 따라 생성, 수정, 삭제된다.<br><br>
내용을 다 쓰기에는 매우 길기에 링크로 대체
[link](https://github.com/cs-devops-bootcamp/Devops-04-S1-Team6-1/blob/main/used_query.md) : https://github.com/cs-devops-bootcamp/Devops-04-S1-Team6-1/blob/main/used_query.md

#### WAS와 데이터베이스 연결
.env에 저장된 데이터베이스의 API key 값들을 WAS에서 plugin에서 해당 .env를 가지고 데이터베이스에 연결하는 code를 작성하여 연결하게 된다.
```
'use strict'

const fp = require('fastify-plugin')

const {
    DATABASE_USERS,
    DATABASE_PASSWORD,
    DATABASE_HOSTS,
    DATABASE_NAMES
} = process.env

module.exports = fp(async function (fastify, opts) {
    fastify.register(require('@fastify/postgres'), {
        connectionString: `postgres://${DATABASE_USERS}:${DATABASE_PASSWORD}@${DATABASE_HOSTS}/${DATABASE_NAMES}`
      })
})

```
데이터베이스에 연결하는 모듈을 exports 하여 다른 파일에서도 사용할 수 있도록 한다.

<br>

#### Search 키워드 제거
API 명세상에는 course의 GET 함수에서 search 키워드로 해당 내용들을 검색한다고 작성하였는데, 실제 querystring의 정보를 저장하니까 해당 키워드가 없어도 기능적인 구현이 충분하다고 생각되어, search 키워드를 사용하지 않고, path에서 주어지는 querystring의 내용들을 이용하여 원하는 기능을 구현하려고 하였다.

<br>

#### Querystring의 활용
이번 프로젝트에서 애먹은 부분 #1.<br>
API 명세에서 보면 /course 이후 3개의 GET 메소드가 querystring을 엔드포인트로 가진다.<br>
PATH에서 뒤에 ?로 이어지는 querystring을 어떻게 활용해야하는가.. 에 대한 고민을 길게 가졌다.<br>
이는 몇개의 레퍼런스를 토대로 해결했다.<br>
[reference](https://dar0m.tistory.com/222): https://dar0m.tistory.com/222
해당 레퍼런스에는 req.query가 주소 바깥, ? 이후의 변수를 담는다고 설명을 한다.<br>

해당 방식이라면 원하는 방향으로 사용할 수 있을 것이라고 판단했고, 실제 사용은
```
let str = Object.keys(req.query)
let queryparams=req.query.instructor_id
```
와 같이 사용하였다.<br>

req.query를 하면 반환값이 ? 뒤의 값이 반환되므로 API 명세의 /course?instructor_id=instructor_id 같은 경우 instructor_id=instructor_id이 객체로 반환된다. <br>

예시로 http://localhost:3000/course?instructor_id=2 으로 GET하면 req.query는
``` 
{
    "instructor_id": "2"
}
```
으로 나온다. 

이 값을 사용하기 위해 Object.keys로 객체의 키값을 추출한 str에는 instructor_id가 저장되고, queryparams에는 instructor_id 키의 value인 2가 들어간다.<br>

str을 통해 해당 쿼리가 어떤 조건의 쿼리인지 if문을 통해 판별하고, 조건에 맞는 경우 그에 맞는 쿼리를 작성하며, queryparams에 저장된 value로 해당 조건에 알맞는 데이터를 가져올 수 있게된다.



#### Authorization
이번 프로젝트에서 애먹은 부분 #2.<br>

해당 고민은 꽤 여러 단계에 걸쳐 진행되었다.

1. 일단 Authorization header가 무엇인가?
Authorization 헤더는 인증 토큰(JWT든, Bearer 토큰이든)을 서버로 보낼 때 사용하는 헤더입니다. API 요청같은 것을 할 때 토큰이 없으면 거절당하기 때문에 이 때, Authorization을 사용하면 됩니다.<br>
[reference](https://archijude.tistory.com/457): https://archijude.tistory.com/457 <br><br>
Authorization을 어떻게 사용되고, 왜 사용되는지 알기위해 레퍼런스들을 보면서 이 헤더에 대해 이해를 했다. <br>
쉽게 이해한다면 Server의 사용자임을 증명하는 신분증 같은 것이라고 이해했다.

2. Authorization의 구현(JWT)
Authorization을 어떻게 구현할까라는 고민으로 상당한 시간을 보내다가, authorization header에는 bearer라는 타입이 있고, 이 중 인증방식 중에 하나인 JWT라는 레퍼런스에 도달하였고, 이를 fastify에서 사용하는 것을 도와주는 @fastify/jwt util를 발견한다. <br>
이를 또다시 어떻게 fastify에서 jwt를 사용할까를 
fastify-jwt 공식 문서를 보면서 고민하였다.<br>
[reference](https://github.com/fastify/fastify-jwt): https://github.com/fastify/fastify-jwt
<br>
해당 공식문서 Usage 파트에서 가장 첫 문장이.
"플러그인으로 register하라"였다. <br>
그렇기에 플러그인으로 어떻게 사용하지 찾아보다가 인상깊은 문구를 발견하게된다. <br><br>
However, most of the time we want to protect only some of the routes in our application. To achieve this you can wrap your authentication logic into a plugin like<br><br>
일부 경로만을 보호할때, 해당 플러그인으로 래핑할 수 있다.<br>
해당 파트에서 착안하여 만든것이,<br>
[link](https://github.com/cs-devops-bootcamp/Devops-04-S1-Team6-1/blob/main/my-lms/plugins/jwt.js) : https://github.com/cs-devops-bootcamp/Devops-04-S1-Team6-1/blob/main/my-lms/plugins/jwt.js
이다. 해당 플러그인으로 래핑하는 예시로 보여주는 것이<br><br>
```
 fastify.get("/",{onRequest: [fastify.authenticate]},async function(request, reply) {
      return request.user
    }
  )
```

- 익숙한 형태의 fastify.get을 사용하는 것을 볼 수 있고, {onRequest: [fastify.authenticate]} 추가하면 토큰이 있어야 통신이 가능한, 즉 authorization header가 필요한 GET을 만들 수 있었다.

3. Authorization을 request header에 넣는 방법?
Authorization header를 사용하려면, 일단 request header에 넣어야 할 것이다.<br>
여기서 부딪힌 문제는 그걸 어떻게 넣지?<br>
이 시점까지 파이어폭스로 localhost로 접속하여 직접 테스트해보고 있었다.<br><br>
그전까지 작성된 endpoint들은 authorization이 필요없었기에 별 문제가 없었으나, jwt 플러그인의 기능으로 래핑해놓으니까 401 에러로 unauthorized가 뜨면서 접속이 불가했다.<br>
이를 해결하려면 authorization header가 필요했고, 그렇다면 이것을 어떻게 구현하지라는 어려움에 빠졌다.
처음에는 fastify.get 안에서 request.headers.authorization = ~~~ 하면서 값을 넣어보려고 했는데, 일단 firefox 상에서는 request header에 새로운 authorization 항목이 생성되지 않는 것을 볼 수 있었다.<br><br>
좀 더 헤메이다가 사실 이미 request는 보내졌는데, 거기에 header를 추가하려 해봐야 될 수가 없다라는 생각에 도달하였다.<br>
해당 부분에 대해서 엔지니어 분께 여쭤보았고, Postman을 통해서 authorization에서 값들을 추가하면서 할 수 있다는 사실을 알게 되었고, 이후로는 Postman으로 test들을 진행하였다.<br>
Postman 상에서 authorization tab에서 jwt bearer를 선택하고, jwt 플러그인에서 최초로 발행한 토큰인 supersecert 키로 접속하니 성공적으로 접속되었다.


4. 통신할 때, 토큰이 필요하다 그렇다면 사용자마다 토큰을 줘서 사용자가 구분되게 할 수는 없을까?
방금 전 과정에서 supersecret 토큰으로만 통신을 진행하다가 든 생각이다.<br>
모든 사용자가 supersecret 토큰을 사용하게 해야하는가? 라는 질문에서 이것 좀 아닌거 같다라는 생각이 들어 jwt의 토큰 생성에 대해 찾아보았다.<br>
그러다가 이에 대해 명확한 방법이 담긴 레퍼런스를 찾는것에 성공했다.<br>
[reference](https://velog.io/@byron1st/Fastify-JWT-%EC%82%AC%EC%9A%A9): https://velog.io/@byron1st/Fastify-JWT-%EC%82%AC%EC%9A%A9 <br>
해당 레퍼런스에서는 jwt의 2개의 함수에 대해 다루어져있는데, decode와 sign이다.<br>
레퍼런스에서의 예시를 참고하여 이를 활용하는 코드를 짜고자 하였고, <br>
jwt.sign(payload) 로 토큰을 만들 수 있고, payload에는 키 쌍으로 값을 지정할 수 있다는 사실을 파악하였다. <br>
이 사실에서 그렇다면 payload에 student_id: 1 같은 사용자 정보의 json 키쌍을 payload로써, token을 생성하고<br>
해당 token을 jwt.decode(token)으로써 decode하면, 사용자 정보의 구분이 가능하다라는 추론과 함께 새로운 endpoint를 하나 만들게 된다.

5. /getToken Path
기능 구현을 하다가 새로 생성한 Path이다. 해당 Path에서는 params 값으로 입력되는 데이터를 payload로써 토큰을 생성하고, 생성한 토큰정보를 반환한다.<br>
생성한 token으로 authorization이 필요한 페이지에 authorization header의 value로써 토큰을 보내게 되면, 이를 추출하여 decoding해서 해당 사용자가 student인지, instructor인지, 사용자의 id를 추출할 수 있을 것이다.<br>
왜냐면 payload에 "student_id"= 1 이나 "instructor_id"= 2 같은 내용을 payload로써 token을 생성했기에, decode하면 같은 내용들이 나올 것 이기 때문이다.
예시로 "student_id"= 1 생성한 token을 가지고 /registration GET을 하게 되면, <br>
해당 student_id와 registration 테이블의 student_id가 일치하는 데이터들만 보여주게된다.<br><br>


#### 방금 삽입하거나 수정한 내용을 리턴
POST 메소드를 사용하면, 테이블에 새로운 내용이 삽입되게 된다.(INSERT를 사용하니깐)<br>
그런데 일반적으로 사용하는 쿼리에서는 아무런 값도 리턴되지 않아 이것이 정상적으로 작동됬는지 모르기에 이를 구분할 수 있는 기능을 구현하고자 하였다.<br>
레퍼런스를 찾아본 결과<br>
[reference](https://stackoverflow.com/questions/2944297/postgresql-function-for-last-inserted-id): https://stackoverflow.com/questions/2944297/postgresql-function-for-last-inserted-id
```
let query = 'INSERT INTO public.registration (student_id, course_id) VALUES (' + str + ',' +bodystr+ ') RETURNING registration_id'
```
로 새로 삽입된 registration_id를 반환하는 query를 완성하였고, 차후 다른 레퍼런스에서 추가적인 내용을 찾게되어 RETURNING *로 수정한 결과, 방금 처리된 내용들이 전부 return되어 출력할 수 있게 되었다.


#### 이후 내용
/course POST에서도 authorization header를 decoding하여 instructor_id일 경우 POST가 가능하고, 아닐 경우 404에러가 뜨게 기능을 작성하였다.<br>
동일하게 /course PATCH에서도 해당 기능을 사용하여 instructor만이 course에서 수정, 추가할 수 있도록 하였다.<br>
DELETE는 팀원들이 작성하여 주었기 때문에 해당 부분에 대한 설명들과 논의가 있었다.<br>

이 시점에서 필수 기능들로써 주어진 기능들은 다 작성되었다고 판단하였기 때문에, 추가적인 기능들을 구현하고자 하였다.


#### 추가적인 기능
1. 유효성 검증
논의 중에 가장 처음 떠오른 기능은 'registration에서 실제로 없는 수업을 수강신청할 수 있다 그러므로 실제로 없는 수업이면 안되도록 해야한다'라는 기능이였다.<br>
유효성의 검증이라고 할 수 있으므로, 해당 부분을 구현하기 위한 고민들을 하였다.<br>
위의 기능은 꽤 단순하게 해결할 수 있었는데 쿼리를 한번 더 보내서 해결을 했다.
```
  let validate = 'SELECT * FROM public.course WHERE id=' + bodystr

    let validateresult = await client.query(validate)

    if(!validateresult.rowCount){
        reply.code(404).send('해당 강좌 ID는 존재하지 않습니다')
    }else{
        try {
            const { rows } = await client.query(
                query
            )
    
            reply.code(201).send(rows)
          } finally {
            client.release()
          }
    }
```
validate는 body로 받은 id가 실제로 course에 있는지 확인하는 쿼리를 보낸다.<br> 
있다면 validate.rowCount=1이 되고, 없다면 validate.rowCount=0이 된다는 점에 착안하여
if문을 통해 값이 있을 때는 진행, 없을 경우 404가 리턴된다.<br>

이 기능을 구현하고 생각해보니, 
1. course에서 post할 때, instructor_id가 없는 id일 수 있다.
2. getToken에서 입력된 student_id나 instructor_id가 없는 id일 수 있다.
<br>
위의 2가지 경우도 유사한 문제가 발생될 수 있다고 판단하여, 같은 방식으로 유효성 검증을 하도록 기능을 만들었다.

2. Join을 통해 id만 보이는 결과들 name도 볼 수 있게
기존에는 registration GET을 하면 registration_id, student_id, course_id가 나왔다.<br>
해당 id만 뜨는 것보다는 관련된 name들도 뜨면 보기에 좋을것 같다는 의견으로 해당 부분에 query를 수정하였다.
```
let querytest = 
'SELECT r.registration_id, r.student_id, r.course_id, s.student_name, c.course_name, c.course_type, i.instructor_name 
FROM public.registration r 
LEFT OUTER JOIN public.student s on r.student_id=s.id 
LEFT OUTER JOIN public.course c on r.course_id=c.id 
LEFT OUTER JOIN public.instructor i on c.instructor_id=i.id 
WHERE student_id=' + str
```
위처럼 쿼리를 수정하면, select에서 지정된 내용들이 조건에 맞는다면 on 조건들이 맞는다면 출력되게된다.



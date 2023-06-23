---
title:  "YAML과 Docker"
excerpt: "DevOps 부트캠프 Section 2 "

categories:
  - Blog
tags:
  - [Blog, DevOps]

toc: true
toc_sticky: true
 
date: 2023-04-11
last_modified_at: 2023-04-11
---
# YAML
YAML이란 데이터 표현 양식의 한 종류이다.<br>
데이터는 다양한 포멧으로 표현될 수 있는데 우리가 일반적으로 사용하는 JSON이나 XML도 표현 양식의 한 종류이다.

데이터를 포멧에 맞게 사용하는 이유는 타 시스템과 연동할때 생기는 문제를 미연에 방지하기 위해서이다.<br>
예를들어 데이터를 자기만의 규칙으로 "김철수-163-62kg" 이렇게 표현하면 작성한 당사자는 "이름-키-몸무게"라고 바로 이해할 수 있지만 규칙을 모르는 상태로 해당 데이터를 전달받은 누군가는 쉽게 이해할 수 없기 때문이다.

위와같이 개인적으로 정의된 데이터를 서로 전달받을때마다 규칙을 함께 전달하기엔 너무 번거롭고, 데이터마다 규칙을 따로 관리하기에는 문서가 너무 많아질 수 있기에 사람들은 가독성이 높고 많은 사람들이 쓰는 데이터 형식으로 저장하곤 한다.

<br><br>

## 사용하는 이유
YAML은 최근들어 많이 활용되는 데이터 포멧인데 그 이유는 인간이 보고 이해하기 쉬운 형태를 가지고 있기 때문이다.<br>
아래는 서버정보를 YAML 포멧으로 작성한 예시이다.
```
#YAML
Servers:
  - name: Server1
    administrator: Kim
    created: 20050103132749
    status: active
  - name: Server2
    administrator: Lee
    created: 20210101000000
    status: active
```
YAML은 기본적으로 들여쓰기(indent)를 원칙으로하며 데이터는 Map(key-value)형식으로 작성되어야 한다.<br>
작성할때부터 들여쓰기를 잘 구분해서 써야하기 때문에 데이터 포멧이 망가질 염려가 없고 그에따라 가독성도 올라갈 수 있어서 최근들어 개발자들에게 많이 사용되고 있다.

YAML과 이전에 많이 사용되던 JSON을 한번 비교해본다면? <br>
아래는 같은 서버정보를 JSON포멧으로 작성한 예시이다.
```
#JSON
{
  Servers:[
    {
      name: Server1,
      administrator: Kim,
      created: 20050103132749,
      status: active
    },
    {
      name: Server2,
      administrator: Lee,
      created: 20210101000000,
      status: active
    }
  ]
}
```
사실 JSON도 많이 사용되는 데이터 포멧이며 보기에 가독성도 나쁘지 않아 보인다.<br>
하지만 JSON은 YAML과 달리 문법상 줄바꿈이나 띄어쓰기를 크게 신경쓰지 않기 때문에 위와같이 읽기 편하게 작성된 JSON이 아닌 경우 정말 가독성이 떨어질 수 있다.
```
#JSON
{Servers:[{name:Server1,
administrator:Kim,created: 20050103132749,
status: active},{name:Server2,administrator:Lee,
created:20210101000000,
status:active}]}
```
위와같이 작성해도 문법상으론 아무런 문제가 없으며 데이터가 늘어날경우 가독성은 더욱 떨어질 수 밖에 없다.


## YAML 사용법
### 1. 데이터 정의
YAML은 기본적으로 key-value 형태로 데이터를 정의한다.
``` 
name: Server1
administrator: Kim
created: 20050103132749
```

### 2. 들여쓰기(indent)
YAML은 들여쓰기로 계층 구조를 표현한다.<br>
들여쓰기는 기본적으로 2칸 혹은 4칸을 지원합니다.
```
Server:
  name: Server1
  administrator: Kim
  created: 20050103132749
```


### 3. 배열 정의
배열로 여러 데이터를 표현하고 싶을경우 - 기호를 사용해서 표현 가능하다.
```
#YAML
Servers:
  - name: Server1
    administrator: Kim
    created: 20050103132749
    status: active
  - name: Server2
    administrator: Lee
    created: 20210101000000
    status: active
```

### 4. 기타(주석, 띄어쓰기)
주석은 # 기호를 사용하여 작성할 수 있다.
```
# Server Info
Server:
  name: Server1
  administrator: Kim
  created: 20050103132749
```
YAML을 key-value형태로 작성할 때 반드시 사이에 띄어쓰기가 들어가야 한다.<br>
그렇지 않은경우 error가 발생할 수 있다.
```
#OK
name: Server1

#Error (not key-value, string)
name:Server1
```

YAML 공식 홈페이지: https://yaml.org/

[reference](https://velog.io/@jnine/YAML%EC%9D%B4%EB%9E%80): https://velog.io/@jnine/YAML%EC%9D%B4%EB%9E%80

<br><br><br>

# JSON
JavaScript Object Notation라는 의미의 축약어로 데이터를 저장하거나 전송할 때 많이 사용되는 경량의 DATA 교환 형식<br>
Javascript에서 객체를 만들 때 사용하는 표현식을 의미한다.<br>
JSON 표현식은 사람과 기계 모두 이해하기 쉬우며 용량이 작아서, 최근에는 JSON이 XML을 대체해서 데이터 전송 등에 많이 사용한다.<br>
JSON은 데이터 포맷일 뿐이며 어떠한 통신 방법도, 프로그래밍 문법도 아닌 단순히 데이터를 표시하는 표현 방법일 뿐이다.<br>


## 특징
- 서버와 클라이언트 간의 교류에서 일반적으로 많이 사용된다.
- 자바스크립트 객체 표기법과 아주 유사하다.
- 자바스크립트를 이용하여 JSON 형식의 문서를 쉽게 자바스크립트 객체로 변환할 수 있는 이점이 있다.
- JSON 문서 형식은 자바스크립트 객체의 형식을 기반으로 만들어졌다.
- 자바스크립트의 문법과 굉장히 유사하지만 텍스트 형식일 뿐이다.
- 다른 프로그래밍 언어를 이용해서도 쉽게 만들 수 있다.
- 특정 언어에 종속되지 않으며, 대부분의 프로그래밍 언어에서 JSON 포맷의 데이터를 핸들링 할 수 있는 라이브러리를 제공한다.


## XML과 JSON
데이터를 나타낼 수 있는 방식은 여러가지가 있지만, 대표적인 것이 XML이고, 이후 가장 많이 사용되는 것이 아마도 JSON일 것이다.

### XML
데이터 값 양쪽으로 태그가 있다. <br>
(HTML을 근본으로 했기에 태그라는 것이 없을 수가 없는데, 그 태그를 줄인다 해도 최소한 표현하려면 양쪽에 몇글자씩이 있어야 한다.)


### JSON
태그로 표현하기 보다는 중괄호({}) 같은 형식으로 하고, 값을 ','로 나열하기에 그 표현이 간단하다.


## JSON 문법
```
{
  "employees": [
    {
      "name": "Surim",
      "lastName": "Son"
    },
    {
      "name": "Someone",
      "lastName": "Huh"
    },
    {
      "name": "Someone else",
      "lastName": "Kim"
    } 
  ]
}
```
- JSON 형식은 자바스크립트 객체와 마찬가지로 key / value가 존재할 수 있으며 key값이나 문자열은 항상 쌍따옴표를 이용하여 표기해야한다.
- 객체, 배열 등의 표기를 사용할 수 있다.
- 일반 자바스크립트의 객체처럼 원하는 만큼 중첩시켜서 사용할 수도 있다.
- JSON형식에서는 null, number, string, array, object, boolean을 사용할 수 있다.


## JSON의 형식
1. name-value형식의 쌍
- 여러가지 언어들에서 object등으로 실현되었다.
- { String key : String value }
```
{
  "firstName": "Kwon",
  "lastName": "YoungJae",
  "email": "kyoje11@gmail.com"
}
```

<br><br>

2. 값들의 순서화된 리스트 형식
- 여러가지 언어들에서 배열(Array) 등으로 실현되었다.
- [value1, value2, ...]
```
{
  "firstName": "Kwon",
  "lastName": "YoungJae",
  "email": "kyoje11@gmail.com",
  "hobby": ["puzzles","swimming"]
}
```

<br><br>

## JSON의 문제점
AJAX 는 단순히 데이터만이 아니라 JavaScript 그 자체도 전달할 수 있다. <br>
이 말은 JSON데이터라고 해서 받았는데 단순 데이터가 아니라 JavaScript가 될 수도 있고, 그게 실행 될 수 있다는 것이다. <br>
(데이터인 줄 알고 받았는데 악성 스크립트가 될 수 있다.)

위와 같은 이유로 받은 내용에서 순수하게 데이터만 추출하기 위한 JSON 관련 라이브러리를 따로 사용하기도 한다.



## JSON이 가져올 수 있는 데이터
JSON으로 가져올 수 있는 데이터는 해당 자바스크립트가 로드된 서버의 데이터에 한정된다.

예를 들어, http://kwz.kr/json.js에서 불러올 수 있는 데이터는 kwz.kr 서버에 존재하는 것만 가능하다. <br>
(구글 데이터를 불러온다거나 네이버 데이터를 불러온다거나 할 수 없다.)

JSON은 단순히 데이터 포맷일 뿐이며 그 데이터를 불러오기 위해선 XMLHttpRequest()라는 JavaScript 함수를 사용해야 하는데 이 함수가 동일 서버에 대한 것만 지원하기 때문이다. <br>
( JSONP 또는 프락시 역할을 하는 서버쪽 Script 파일로 가능하게도 할 수 있다.)

<br><br>

### JSON 형식 텍스트를 JavaScript Object로 변환하기
```
var jsonText = '{ "name": "Someone else", "lastName": "Kim" }';  // JSON 형식의 문자열
var realObject = JSON.parse(jsonText);
var jsonText2 = JSON.stringify(realObject);

console.log(realObject);
console.log(jsonText2);
```
- JSON.parse( JSON 형식의 문자열 ) : JSON 형식의 텍스트를 자바스크립트 객체로 변환한다.
- JSON.stringify( JSON 형식의 문자열로 변환할 값 ) : 자바스크립트 객체를 JSON 텍스트로 변환한다.

<br><br><br><br>

# Docker
Docker는 Container형 Application의 빌드, 배치, 관리 기능을 제공하는 오픈 소스 플랫폼이다.

Docker는 LinuXContainer를 생성 및 사용할 수 있도록 하는 Container화 기술로,
Docker는 개발자가 개발한 Source Code(Application)를 실행하는 표준 방식을 제공한다.

쉽게 말해 Docker는 Container 를 위한 운영 체제(O/S)이다.
Docker를 사용하면 Container를 아주 가벼운 Module 형식 VM 처럼 다룰 수 있다.

Docker를 통해 개발자는 Application을, Container로 Packaging할 수 있다.

아래 그림은 Container와 가상 머신(Virtual Machine)의 차이점을 보여준다.
![alt text](/images/dockervm.png)

VM 환경에서는 Server 위 Hypervisor를 통해 여러개의 O/S를 구동시켜,
다수의 Application을 구동 시키는 형식이지만,


Container의 경우 하나의 O/S위에,
Docker Engine내에서 여러개의 Container를 구동시켜,
다수의 Application을 구동 시킬 수 있다.


## Container?
Docker에서 사용하는 Container 개념 이전에,
Linux O/S에는 LinuXContainer(LXC)라는 Container 기능이 2008년 부터 제공되고 있었다.

이 LinuXContainer(LXC)는 단일 Instance에 대해 가상화 기능을, Linux 커널 단에서 제공한다.

### Container In Docker
Docker에서 Container의 개념은,
Application Source Code를 임의의 환경에서 실행하기 위해 필요한,
O/S Library 및 종속 항목과 결합하여 실행 가능한 표준 Component이다.

이러한 Container는 분산형으로 분화되는 Application의 실행 환경 Delivery를 간소화해주며, Cloud 환경으로 이전되는 개발환경 상황에서 유용하게 사용된다.


## Docker VS LXC
Docker는 LinuXContainer(LXC)에 아래와 같은 기능을 추가하였다.

- 완벽한 이식성 <br>
LXC가 간간히 System의 특정 구성을 참조하는데에 반해,
Docker의 Container는 DeskTop, Data Center, Cloud 환경에서 별도의 수정없이도 실행된다.

- 경량성<br>
LXC를 사용할 경우 다수의 Process를 하나의 단일 Container 안에서 결합할 수 있다.<br>
Docker의 Container를 사용할 경우,
각 Container안에서는 하나의 Process만 실행할 수 있다.<br>
이를 통해서 Update나 수정사항 반영을 위해 하나의 Process를 중단하는 동안에도,
계속해서 실행 가능한 Application을 Build 할 수 있다.

- 자동 Build<br>
Docker는 Application Souce Code를 기반으로,
Container를 자동으로 Build할 수 있다.

- Container Version 관리<br>
Docker는 Container 이미지의 버전에 대해,
Version Rollback이나 Build한 사용자 및 방법 등을 Tracking할 수 있다.<br>
더나아가 이전 버전과 신규 버전 사이의 변경점만 Upload할 수 있다.

- 재사용성<br>
새로운 Container를 생성할 때,
기존에 사용하던 Container를 Template 형식으로 재사용하여,
기본 이미지로 사용하며 생성할 수 있다.

- 공유성<br>
자신이 생성한 Container 이미지나 다른 사람이 생성한 Container 이미지를,
Open Source Registry에 등록 및 참조하여 사용할 수 있다.



## Docker 구조
Docker의 구조를 살펴보면 아래 그림과 같다.
![alt text](/images/docker구조.png)

Docker가 실행될 때 구성 요소는 크게 5가지로,

Docker Daemon, Docker Client, Doker Host, Docker Object, Docker Registry가 있다.

위 그림을 토대로 구성 요소들을 살펴보면 아래와 같다.

### Docker Daemon
Docker Daemon은 dockerd로 지칭되며,
Docker API 요청을 수신하고 Docker Object인,
Image, Container, Network & Volume를 관리한다.

Docker Daemon은 Docker Service관리를 위해 다른 Docker Daemon과 통신할 수 있다.

### Docker Client
Docker Client는 docker로 지칭되며,
Docker를 사용하는 사용자, 개발자가 Docker와 상호 작용하는 기본 방법이다. <br>
Docker Client(docker)는 docker run와 같은 명령을,
Docker Daemon(dockerd)에게 보냄으로 써 Docker API를 요청한다.

Docker Client는 하나 이상의 Docker Daemon과 통신할 수 있다.

### Docker Host
Docker Host는 Docker Engine을 구동시키는 Server를 생각하면 된다.<br>
하나 이상의 Docker Daemon을 구동하는 Server로,<br>
해당 Server 내부에서 Docker Client(docker)의 명령을 토대로,<br>
자신이 구동중인 Docker Daemon(dockerd)로 명령을 전달한다.

### Docker Object
Docker를 사용하면 Image, Container, Network & Volume 등을 만들어 사용하게 되는데,
이들을 Docker Object라 지칭한다.

<br><br><br>

## Image
Image는 Docker Container를 생성할 수 있는 Template 형식이다.

자신만의 Docker Container 생성을 위한 Image를 만들거나,
Registry에 등록된 다른 사용자의 Image를 토대로 사용 및 수정하여,
Docker Container를 생성할 수 있게 해준다.

고유한 Image Build를 위해서는 Image 생성 및 실행을 위한 단계를,
정의하기 위한 간단한 구문을 사용해 Dockerfile을 만든다.

이 Dockerfile의 각 명령은 Image에 계층(Layer)을 생성하게 된다.

이 Dockerfile을 변경해 Image를 다시 Build 하면,
변경된 계층(Layer)만 다시 Build 되어,
다른 가상화 기술과 비교해 Docker를 사용하는 이유인,
경량성을 충족시키는 부분이다.

### Container
Cotainer은 앞서 살펴본 Image를 통해 실제로 메모리 영역에 Onload 된 Instance이다.

Docker API나 CLI를 사용해 Container를 생성, 시작, 중지, 삭제 등 관리할 수 있다.

[출처](https://velog.io/@gillog/Docker%EB%9E%80): https://velog.io/@gillog/Docker%EB%9E%80

<br><br>

# 회고 TIL
이번에는 YAML과 Docker를 배웠다.
YAML은 데이터를 전송할 때, 데이터를 표현하는 양식 중의 하나이다.<br>
JSON과 비슷한 점이 있지만, 작성하는 방식에서 들여쓰기로 계층을 구분하는 등의 차이가 존재한다.<br>
YAML을 배운 이유는 Docker image와 연관짓기 위함이라고 생각된다.

Docker는 여러 컨테이너들을 다루는 엔진이라고 이해하였다. (실제로는 오픈소스 플랫폼이라고 한다.)

image에는 해당 컨테이너가 사용할 어플리케이션과 해당 어플리케이션이 작동하기 위한 제반사항, 환경이 포함되어 있고, 어플리케이션의 dependency가 작성되어 있다.

image를 통해 컨테이너를 생성하면, image에 작성되어 있던 내용들을 토대로, 환경과 dependency가 구성되어 어플리케이션을 실행하는 환경을 구성한다.

해당 컨테이너 내부안에서 환경을 구성하기 때문에 실제 이 컨테이너를 돌리는 환경에 구애받지 않는다. <br>
Mac, windox, Linux 등등 OS가 다르면 환경이 달라지지만, 컨테이너는 그 내부에서 작동되니 어플리케이션은 문제 없이 작동한다.

이러한 image는 레지스트리에 저정된다. 이미지 레지스트리는 도커 CLI에서 image를 통해 컨테이너를 생성할 때, 호스트에 해당 이미지가 없다면, 레지스트리에서 다운받게 된다.<br>
image를 저장하고 공유하는 목적을 사용된다.

---
title:  "Section3 Project"
excerpt: "DevOps 부트캠프 Section 3"

categories:
  - Blog
tags:
  - [Blog, DevOps]

toc: true
toc_sticky: true
 
date: 2023-05-26
last_modified_at: 2023-05-30
---
# Project 개요
### [project_link](https://github.com/wngud9646/project3-msa) : https://github.com/wngud9646/project3-msa


## Project 목표
- 메시지 큐의 Pub/Sub 패턴과 Producer/Consumer 패턴의 차이를 이해한다
- DB와 서버와의 통신이 가능하도록 연결한다
- 특정 상황에서 SNS, SQS로 메세지가 전달되도록 시스템을 구성한다
- SQS에 들어온 메세지를 레거시 시스템(Factory API)으로 전달하는 시스템을 구성한다
- 레거시 시스템(Factory API)의 콜백 대상이되는 리소스를 생성해 데이터베이스에 접근 할 수 있게 한다




## tutorial
이번 프로젝트에 앞서, iac 도구인 serverless framework에 대한 이해를 위한 튜토리얼을 진행하였다. 

### Step 1
[reference](https://www.serverless.com/framework/docs/tutorial) : https://www.serverless.com/framework/docs/tutorial<br>
해당 레퍼런스를 참고하여, serverless를 설치하고 생성한다.

```
npm install -g serverless
```

명령어를 통해 설치하고,

```
serverless
```

로 템플릿을 생성할 수 있었다.

템플릿은 기본적으로 region이 설정되어있지 않으므로, 생성된 serverless project 디렉토리에서 serverless.yml을 수정한다.

위와 같이 수정하면 aws region이 ap-northeast-2로 설정된다.

이제 실질적으로 project에서 동작하는 index.js를 수정한다.

#### 트러블 슈팅
위의 작업에서 트러블 슈팅한 내용

- 템플릿대로 serverless를 구성후에 curl을 보냈더니 <br>
message: not found가 돌아옴 => http 404 에러로 판별되었다.
=> serverless.yml에서 api event가 get만 명시되어 있었다. <br>
그렇기에 lambda의 api gateway는 post가 동작이 안됨<br>
api event 부분을 post로 수정하였다.

- 500 Internal Server Error 발생 => handler를 인식하지 못하는 에러로 판별되었다.<br>
팀원의 조언으로 해결을 하였다.<br>
serverless.yml fuction handler 부분에서 index.handler를 참조하는데
index.js에서는 export를 hello를 하고 있었다.<br>
index.handler를 index.hello로 수정하였더니 정상적으로 함수가 작동된다.


해당 코드를 작성 후에 serverless deploy를 실행하면 aws 상에 serverless.yml에서 작성된 리소스들이 생성된다.<br>
해당 템플릿에서는 index.js를 handler로 하는 lambda와 lambda의 트리거인 api gateway가 생성된다.

해당 게이트웨이로 '{ "input": 1 }' 를 body로써 보낸다면<br>
'메시지를 받았습니다. 입력값: 1, 결과: 2' 가 출력된다.

<br><br>

### Step 2
SQS와 DLQ<br>
해당 step에서는 SQS와 DLQ를 생성하여 연결하고, lambda로 SQS에 메세지를 보내고, SQS의 메세지를 소비하는 lambda를 생성한다. 

#### SQS의 사용 이유
내결함성과 확장성이 뛰어난 스토리지에 메시지를 안정적으로 전송 및 저장하기 위함.<br>
SQS는 메시지를 버퍼링하고, 메시지 소비자가 처리할 준비가 될 때까지 메시지를 보존한다.


#### SQS와 DLQ를 사용하는 이유
- 재시도 및 오류 처리: SQS는 메시지 큐 시스템으로, 메시지를 비동기적으로 처리한다. <br>
처리 중에 오류가 발생하거나 처리가 실패한 경우, DLQ를 통해 해당 메시지를 보관하고 재시도할 수 있다. <br>
DLQ는 실패한 메시지를 안전하게 보관하고, 재처리 및 디버깅에 활용된다.

- 에러 처리 및 로깅: DLQ는 재시도를 위해 실패한 메시지를 저장하는 곳이다. <br>
실패한 메시지를 DLQ로 전송하면, 해당 메시지에 대한 자세한 정보와 오류 내용을 확인할 수 있다. <br>
이를 통해 에러 처리 및 디버깅에 도움을 줄 수 있다.

- 예외 상황 처리: SQS와 DLQ를 연결함으로써 예외 상황에 대비할 수 있다. <br>
예를 들어, 네트워크 문제, 시스템 장애, 처리 오류 등의 이유로 메시지 처리가 실패하는 경우, 해당 메시지를 DLQ로 이동시켜 안전하게 보존한다. <br>
이후 재시도나 대체 처리를 통해 문제를 해결할 수 있다.

- 메시지 검증 및 중복 제거: DLQ를 사용하여 메시지의 일관성과 중복 제거를 관리할 수 있다.<br> 
잘못된 형식의 메시지나 중복된 메시지를 DLQ로 보내어, 유효성 검사 및 중복 제거 작업을 수행할 수 있다.

- 장애 내구성: SQS와 DLQ를 연결하면 장애 내구성이 향상된다. <br>
SQS는 여러 가용 영역에 걸쳐 메시지를 복제하고 저장하기 때문에, 메시지 손실을 방지하고 시스템의 안정성을 보장할 수 있다.

consumer를 위와 같이 구성하면, 아까와 같이 body에 input이라는 key를 넣고, sqs에 메세지를 전달하는 lambda의 엔드포인트로 보낸다면, consumer에서 위와 같이 소비하고, cloudwatch에 로그를 남기게 된다.



## Project
실제 프로젝트의 진행이다.

## Step 1 : Lambda 서버(Sales API) - DB 연결
주어진 API를 가지고 serverless를 통해 배포하게한다.

실제 코드상으로는 환경변수를 대하여 다루게 되는데 로컬 상에서는 .env를 가지고 환경변수를 관리하였다.<br>
하지만 실제로는 .env를 사용하면 정보들이 노출되기 때문에 다른 방식을 생각해보았고,
AWS Secret Manager를 사용하여 환경변수들을 관리한다.

여기서 기존에 없던 새로운 파일은 만들어
저장된 Secret들을 가져오는 모듈을 작성한다.

해당 내용을 토대로, 환경변수들을 드러나지 않게 사용하게 할 수 있었다.

해당 단계에서 나타낸 에러로는 lambda의 실행 역활에서 Secret Manager를 참고할 수 있는 권한이 없어, 이를 직접 설정해 주어야했다. <br>
이 부분은 serverless.yml에서 내용을 수정할 수도 있었다.

<br><br>

## Step 2 : “재고 없음” 메시지 전달 시스템 구성
아키텍처 상으로 재고가 없는 경우, 해당 이벤트에 대한 메세지가 AWS SNS로 발행되고, SNS에 구독되어 있는 AWS SQS가 해당 메세지를 Queue에 담는다.

해당 아키텍처의 구현을 위하여 serverless.yml을 수정하였다.

해당 과정을 통해 resource들을 IaC로 생성할 수 있었는데, 위와 같이 생성된 리소스 중에서 SNS와 SQS의 구독이 제대로 작동하지 않는다.

원인은 SQS 접근 권한이 설정되지 않기 때문인데, 이는 SNS나 SQS에서 새롭게 구독을 생성하면 SQS의 접근권한이 그에 맞게 수정되면 해결할 수 있으나, IaC 상으로 해결할 수 있는지 연구가 필요하다.

위와 같이 리소스들을 생성했다면, SNS에 메세지를 발행해야하는데,
이는 코드상에서 Database 쿼리로 재고가 0인 경우의 조건문에 메세지를 발행하는 코드를 추가한다.

<br><br>

## Step 3 : 메시지를 Factory API로 전송하는 Lambda 구성 및 DLQ 추가

Dlq를 resource에서 만들어주고, SQS에서 deadLetterTargetArn을 작성해주면, Dlq와 연결이 된다.

여기에 SNS에서 발행된 메세지가 SQS에 쌓일텐데, 이러한 메세지를 소비하는 lambda인 stock-lambda를 생성한다.

초기에 작성할 때, 공장에 생산 메세지를 보내는 코드인 stock-lambda도 sales-api 디렉토리에서 serverless.yml을 통해 작성하였다. <br>
해당 시점까지는 sales-api에서 serverless.yml을 수정하였지만,
loosely-couple를 위해서 해당 lambda들은 따로 관리하는 것이 좋다고 판단되어, 새로운 stock-lambda라는 디렉토리에서 관리하도록 변경하였다.

또한 sales-api/serverless.yml에서 모든 리소스를 생성하였지만,
stock-lambda에서 SNS와 SQS를 생성하도록 수정하였다. <br>
Sales-api는 메세지를 SNS로 발행하면 역할을 다하는것이고,
stock-lambda는 SNS에 메세지가 발행되면서 시작되고, SQS의 메세지를 소비하면서 끝난다.

<br><br>

## Step 4 : 데이터베이스의 재고를 증가시키는 Lambda 함수 생성
주어진 api 명세에 따라 메세지를 보내는 함수를 작성한다. <br>
추가적인 함수 생성보다는 현재 메세지를 소비하고 종료되는 stock-lambda에서 메세지를 소비하며, 명세에 따른 post request를 보내는 함수로 변경한다.

해당 request를 보내면 api에서 response가 오는데, 현재 api에 request에 url을 작성한다면, api에서 response를 body로 url로 request를 보낸다.<br>
즉, response api 명세를 사용하여, Database의 재고를 늘리는 stock-increase-lambda 함수를 작성한다.

<br><br><br>

## 프로젝트 전체 회고
전에 진행했던 project1이나 2보단 좀 더 쉽게 진행할 수 있었다.<br>
프로젝트와 세션들을 진행하면서 확실히 여러가지를 배웠고, 그것들을 내것으로 만들었다는 점을 느낄 수 있었다.

project 1에서 사용하였던 express나 axio 같은 모듈들에 대한 이해가 없었다면, 기능들에 공부를 하면서 사용했어야겠지만, 이미 사용해본 경험이 있기에 좀 더 능숙하게 다룰 수 있었다.<br>

Session 2에서 진행했던 AWS에 대한 내용들을 토대로 이번 프로젝트에서는 AWS 관련 오류가 많이 발생한 편도 아니였고, Cloudwatch log를 통해 트러블슈팅도 자연스럽게 진행되었다.<br>

이러한 점들을 보았을때, 확실히 처음 세션을 시작할 때와 비교한다면 많은 발전이 있었다고 돌아볼 수 있을것이다.<br>
하지만 이에 안주하지 않고, 공부할 영역은 아직도 넓고 내용을 많으니 노력해야겠다는 생각을 가졌다.

현재 공부해볼 부분은 Terraform을 사용해서 완전한 IaC 관리를 해야겠다는 생각이 든다.<br>
현재로써는 약간의 메뉴얼한 작업이 필요한 상태이기에 완전한 자동화를 위해 좀 더 수정할 점이 느껴졌다.<br>
Terraform이 api gateway에 여러가지 문제점이 생겨서 좀 더 연구가 필요하다.

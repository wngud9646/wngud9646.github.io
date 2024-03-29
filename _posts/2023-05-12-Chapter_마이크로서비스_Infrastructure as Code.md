---
title:  "Infrastructure as Code"
excerpt: "DevOps 부트캠프 Section 3"

categories:
  - Blog
tags:
  - [Blog, DevOps]

toc: true
toc_sticky: true
 
date: 2023-05-12
last_modified_at: 2023-05-12
---
# IaC
## 프로비저닝이란?
IaC를 공부하기 이전 프로비저닝에 대해 알아볼 필요가 있다.

Provisioning(프로비저닝)이란 사용자의 요구에 맞도록 시스템 자원을 할당, 배치, 배포해 두었다가 필요한 상황에 시스템을 즉시 사용할 수 있는 상태로 미리 준비해 두는 것을 말한다.

더 쉽게 말하자면 Infra 자원, 서비스, 장비 등의 IT 자원을 사용자의 요구사항에 딱 맞도록 제공해주는 것을 말한다.

프로비저닝의 종류에는 5가지가 존재한다.

- Server Resource Provisioning<br>
CPU, Memory, IO 같은 실제 서버 자원을 할당 및 운영할 수 있게 제공
- OP Provisioning<br>
OS를 서버에 설치하여 사용할 수 있게 제공해줌
- Software Provisioning<br>
WAS, DBMS 등의 SW를 즉시 사용할 수 있도록 세팅
- Account Provisioning<br>
접근 권한을 가진 계정을 제공해주는 것
Cloud Infra 쪽에서는 업무 관리자가 변경된 경우 Account Provisioning을 통해 권한 인계를 하는 경우가 많다
- Storage Provisioning<br>
데이터를 저장하고 관리할 수 있는 Storage를 제공하는 것
Provisioning이 중요한 개념이 된 것은 Cloud의 인기 때문이다.

Cloud는 서버를 직접 구축할 수 있기 때문에 제공하는 서비스들 각각의 용도에 맞게 환경을 구축하는 것이 중요하며, Provisioning을 통해 Cloud Server를 내가 원하는 대로 Setting 할 수 있게 된 것이다.

용도에 맞는 서버 환경 구축 작업을 자동화하는 것을 자동화 Provsioning이라고 말하며 이는 CI/CD의 가장 큰 핵심인 "자동화"에 있어서도 매우 중요해진 개념이라고 볼 수 있다.

<br><br>

## IaC란
수동 프로세스가 아닌 코드를 통해 Infra를 관리하고 Provisioning하는 것을 말한다.

베어 메탈 서버나 IT Infrastructure 같은 물리 장비 및 가상 머신에 내가 원하는 환경 설정을 Script를 통해 적용하는 것이다.

(베어 메탈 서버 : HW에 어떤 SW도 설치되어 있지 않은 순수한 서버)

IaC를 활용할 경우 인프라 사양 및 설정을 담은 구성 파일(Script)을 생성하고 이를 통해 서버를 구축하므로 서버 구축이 쉬워지며 서버에 대한 일괄적인 설정 변경이 쉬워진다.

또한 IaC는 모든 서버가 동일한 환경을 가짐을 보장할 수 있고 Infra 구축 과정이 코드화 및 문서화됨으로 인해 관리가 더욱 수월해진다.


### 장점
- 일관성<br>
IaC는 코드를 통해 서버 환경을 설정하므로 Script를 통해 설정된 모든 서버가 동일한 설정임을 보장할 수 있다.<br>
서버를 확장해야 할 때도 Script를 활용하면 새로운 서버 환경이 기존 서버 환경과 동일함이 보장되므로 환경 설정 차이로 인해 문제가 발생할 확률이 줄어든다.

- 효율성<br>
IaC를 활용하면 프로그래밍 방식을 통해 SW를 일괄적으로 관리할 수 있으므로 운영자 1명이 동일한 코드를 통해 여러 개의 시스템을 한 번에 구축 및 관리할 수 있다.<br>
또한 Infra 설정에 어려움이 없으므로 개발자가 빠르게 개발 환경을 구축하고 개발을 진행할 수 있어 개발 속도를 증가시킬 수 있다.<br>
즉, 필요한 인력이 줄어 비용을 절감할 수 있으며 개발의 속도도 증가시킬 수 있게 되는 것이다.

- 위험 감소<br>
IaC는 Script를 통해 서버 환경을 설정하므로 특정 서버에 문제가 발생하더라도 Script를 재실행시킴으로써 빠르게 환경 복구가 가능해진다.<br>
또한 운영자가 직접 서버에 접속하여 수동으로 서버 환경을 수정 할 때 발생할 수 있는 Human Error를 줄일 수 있다.<br>
대표적인 Human Error는 서버 설정 값을 잘못 입력했다거나, 여러 개의 서버가 동작하는 상황에서 몇 개의 서버에 환경 설정 적용을 누락하는 것이 대표적일 것이다.

<br><br>

## IaC 접근 방식
![alt text](/images/Iac.jpg)

### 선언형 접근 방식(Declarative)
문제를 달성하는 방법을 정의하지 않고 최적의 시스템 상태만 정의하는 것을 말한다.

How를 버리고 What을 중요시한 접근 방식이다.

선언적 IaC는 운영자가 서버 환경 설정을 어떻게 수행해야하는지는 알 필요가 없으며 단지 Script에 필요한 시스템 상태를 기입만 한다면 IaC Tool이 알아서 Script에 기입된 상태의 서버를 생성해주는 것이다.

선언형 접근 방식으로 Infra를 할당받은 시스템은 현재 상태 목록을 유지하므로 인프라가 중단되더라고 쉽게 복구가 가능하다.

대부분의 IaC는 선언적 접근 방식을 활용한다.

### 명령형 접근 방식(Imperative)
Infra 구성 방법 뿐만 아닌 달성하는 과정을 정확하게 정의해야 한다. 달성 방법이 올바른 순서로 실행되어야 원하는 Infra 구성의 서버가 생성된다. What과 How를 모두 명시해야 하는 접근 방식이다.

명령형 방식은 사양에 대한 지식과 Infra 구동 순서, 코드 작성까지 사람이 모두 입력해야 하므로 난이도가 높은 접근 방식이다.

또한 서버 환경에 변경이 필요한 경우 운영자는 변경사항이 어떻게 어떤 단계에서 적용되어야 하는지 해독해야 하므로 불안정성이 높은 IaC 접근 방식이라 할 수 있다.

<br><br>

## IaC가 중요해진 이유
이전에 운영자가 수행했던 Provisioning 작업을 개발자가 IaC Tool과 Script를 활용함으로써 빠르게 Infra 준비가 가능해졌다. 개발자는 Infra 준비를 기다리는 동안 Application 배포를 중지해야 할 필요가 없어졌으며 개발자가 서버 환경을 설정하기도 편해져 DevOps 도입에도 큰 도움이 되었다.

DevOps의 가장 중요한 부분인 CI/CD 부분에도 IaC는 큰 도움을 준다.

CI/CD 과정의 가장 중요한 개념은 자동화일 것이다. 이는 CI/CD 과정에 포함된 테스트도 마찬가지이다.

문제는 테스트를 진행하는 서버와 운영 서버 환경이 다르다면 차이로 인한 새로운 에러가 발생할 수도 있으며 이를 방지하기 위해 테스트를 진행하기 이전 테스트 서버와 실제 운영 서버의 환경 설정이 차이가 있는지를 먼저 검수해야 한다.

CI/CD 과정을 수행하기 전 환경 설정을 비교하는 것은 자동화와는 거리가 있는 작업이다.

하지만 위에서 말했듯 IaC를 통해 서버 환경을 구축한다면 모든 서버가 일관성 있는 환경임이 보장되며 이는 테스트 서버와 운영 서버에도 마찬가지로 적용된다.

따라서 CI/CD 과정에서 테스트가 정상적으로 수행되었다면 IaC Script를 그대로 활용하여 운영 서버를 설정한다면 개발 서버 환경을 그대로 이식할 수 있으며 Application의 안정성을 보장할 수 있게 된다.

또한 QA 전문가도 Infra의 복사본으로 Application의 동작 및 성능을 테스트할 수 있으므로 운영 환경에서 직접 테스트를 진행하지 않더라도 운영 서버에서 테스트를 진행하는 것과 동일한 효과를 내 안정성을 가질 수도 있다.


## IaC Tool
### Terraform
Terraform은 Provisioning 도구이다.

프로비저닝 도구는 환경이 계속해서 원하는 상태에 있게 하는 것을 목표로 둔다.

Terraform은 원하는 환경 상태를 지속적으로 저장하며 만약 원하는 환경 설정에서 벗어난 부분이 존재한다면 환경 자체를 다시 로드하여 시스템을 자동으로 계산하고 복원한다.

Terraform은 설정과 실제 환경이 다를 경우 환경 자체를 다시 복원하다보니 환경이 일정하고 변하지 않는 상태가 필요한 작업에 적합한 IaC Tool이다.

이 때문에 Terrafrom은 변동이 거의 없는 운영서버 관리나 Load Balancing, Computing, VPCs, Storage 같은 클라우드 리소스 관리 용도로 활용된다.

이렇듯 Terraform은 어느정도 서버 환경 설정이 되어 있는 상태에서 많이 활용되다 보니 기본적으로 Terraform은 Bare Metal 서버에서의 Provisioning을 지원하지 않는다.

Terraform은 선언형 접근 방식의 IaC Tool로써 자체 Script(DSL)를 통해 서버 환경을 설정한다는 특징을 가진다.


### Ansible
Ansible은 CM(Configuration Management; 구성 관리) 도구이다.

Provisioning 도구가 시스템을 새로 로드하는 것과는 다르게 CM 도구는 시스템을 재설정하거나 교체하지 않고 문제를 해결한다. 이 때문에 CM 도구는 수명이 짧고 변동이 많은 환경에서 많이 활용된다.

Ansible은 변동이 많은 환경에서 주로 활용되며 이 때문에 CI/CD Tool로써 자주 활용된다. 특히 Cloud 환경에 개발한 App이나 서비스를 배포하는 데에 많이 활용된다.

이렇다 보니 CI/CD 과정에서 많이 활용되는 Tool인 Kubernetes나 AWS와 연동이 매우 잘 되는 IaC Tool이다.

또한 새로운 환경에 App이나 서비스를 배포할 때 많이 활용되므로 Bare Metal 서버에서의 Provisioning을 지원하며 Terraform보다 확장성이 좋다.(물론 Terraform도 확장성이 좋은 편이지만 Ansible이 더 좋은 확장성을 가진다.)

Ansible은 명령형과 선언형 접근 방식을 동시에 활용하는 Hybrid라고 할 수 있으나, 명령형 접근 방식을 활용하는 쪽에 조금 더 가깝다.

추가로 Ansible은 자체 DSL Script 대신 YAML 파일을 통해 환경 설정을 수행하므로 다른 IaC Tool보다는 Script를 통해 시스템을 저장하고 구성하는 것이 쉽다고 할 수 있다.

Ansible은 매우 특수한 특징을 가지는데 바로 Agent가 필요 없다는 것이다.

다른 IaC Tool은 Client(위 사진에서 Ansible-node 컴퓨터)측에 Agent가 설치되어 있어야 한다.

이후 IaC Tool의 Server 측에서 Agent에게 요청을 보내면 Agent가 받은 요청(Script)을 통해 환경 설정을 수행하는 것이다.

하지만 Ansible은 Agent가 Node 측에 필요가 없고 단지 Node에 Python만 설치되어 있으면 된다.

Ansble은 Python이 자체적으로 가지고 있는 네트워크 모듈을 통해 Server-Node 사이 통신을 수행한다.

Linux에서는 Python이 기본적으로 설치되어 있고 대부분의 서버는 Linux 환경으로 구성한다는 점을 생각해본다면 사실상 Server 측에 Ansible만 설치되어 있다면 즉시 활용할 수 있다고 생각할 수 있다.



## 불변한(Immutable) 인프라스트럭처
불변한(Immutable) 인프라스트럭처는 인프라 리소스를 변경할 때 기존 리소스를 직접 수정하는 대신 새로운 리소스를 생성하고 이전 상태를 유지하는 개념이다. <br>
이는 기존 인프라 리소스를 수정하는 대신 새로운 버전을 생성하여 변경 사항을 반영하는 방식으로 작동한다.

불변한 인프라스트럭처의 핵심 원칙은 다음과 같다:

- 변경 대신 교체: 기존 리소스를 직접 수정하는 대신 새로운 리소스를 생성하고 이전 리소스를 제거한다.<br> 
이를 통해 변경 사항을 반영하고 이전 상태를 보존합니다.

- 코드로 관리: 인프라스트럭처는 코드로 정의되며, 변경 사항은 코드의 수정을 통해 이루어진다.<br> 
코드 형상 관리 시스템을 통해 변경 이력을 추적하고 협업이 용이하다.

- 인프라스트럭처의 상태를 유지: 변경 사항이 발생하더라도 기존 인프라스트럭처의 상태는 유지된다.<br> 
이전 버전의 리소스는 변경되지 않으며, 이를 통해 롤백이나 재현이 가능해진다.<br><br>


불변한 인프라스트럭처의 장점은 다음과 같다:

- 예측 가능성: 인프라 리소스의 변경으로 인한 예기치 않은 부작용을 최소화한다. <br>
변경 사항을 사전에 테스트하고 검증할 수 있다.
- 롤백 및 복구 용이성: 이전 상태의 리소스를 유지하기 때문에 문제가 발생하면 이전 상태로 쉽게 롤백하거나 복구할 수 있다.
- 확장성: 새로운 리소스를 생성하므로 수평 확장이 용이하다. <br>
필요한 만큼의 리소스를 추가로 생성하여 처리량을 증가시킬 수 있다.
- 협업과 버전 관리: 코드로 정의되고 변경 이력이 관리되므로 팀 간 협업이 용이하고 변경 사항을 추적할 수 있다.

불변한 인프라스트럭처를 구현하기 위해서는 IaC 도구를 사용하고, 인프라 리소스를 코드로 정의하고 관리해야한다. <br>
이를 통해 인프라스트럭처를 일관되고 예측 가능한 상태로 유지할 수 있다.



### 질문
### 가변적(mutable) 인프라와 불변적(immutable) 인프라의 차이는 무엇인가요?
가변 인프라와 불변 인프라 사이의 가장 근본적인 차이는 central policy에 있다.

Mutable Infrastructure:<br>
서버를 수동으로 수정할 수 있다. <br>
서버에 로그인하여 구성을 변경하고 패키지를 설치/수정할 수 있다.

Immutable Infrastructure:<br>
일단 배포되면 서버를 수정할 수 없다. <br>
구성을 변경해야 하는 경우 기존 이미지를 업데이트하고 새 서버를 가동하여 이전 이미지를 교체할 수 있다. <br>
서버가 실행되는 동안에는 구성 또는 코드 변경이 허용되지 않는다.

좀 더 자세히 설명하자면, 서버 기반의 가변 인프라와 불변 인프라 사이에는 practical 및 conceptual 적인 차이가 있다.

- conceptual<br>
두 종류의 인프라는 서버를 어떻게 다루어야 하는지에 대한 접근 방식(예: 생성, 유지 관리, 업데이트, 제거)이 크게 다릅니다. 이것은 일반적으로 "pets vs cattle”의 비유로 설명된다.

- practical<br>
Mutable 인프라는 가상화 및 클라우드 컴퓨팅과 같은 불변의 인프라를 실현하고 실용적으로 만드는 핵심 기술보다 훨씬 오래된 인프라 패러다임이다.


가변적(mutable) 인프라:
- 가변적 인프라는 변경 가능한 상태를 가지며, 직접 수정이 가능하다.
- 변경이 필요한 경우 기존 인프라 리소스를 직접 수정하고 업데이트할 수 있다.
- 예를 들어, 서버에 패치를 적용하거나 구성을 변경하는 경우 서버에 SSH로 접속하여 직접 수정한다.
- 가변적 인프라는 즉시 변경 사항이 반영되며, 유연하게 수정할 수 있다.
- 하지만 수정 작업이 직접적으로 수행되므로 운영 환경에서 예기치 않은 문제가 발생할 수 있다.

불변적(immutable) 인프라:
- 불변적 인프라는 변경할 수 없는 상태를 유지하며, 변경이 필요한 경우 새로운 인프라스트럭처를 배포한다.
- 변경이 필요한 경우 이전 인프라스트럭처를 완전히 제거하고 새로운 인프라스트럭처를 생성한다.
- 예를 들어, 서버의 구성이 변경되는 경우 이전 서버를 삭제하고 새로운 서버를 배포한다.
- 불변적 인프라는 변경 사항을 적용하기 위해 인프라를 재생성하는 접근 방식을 사용한다.
- 변경 작업이 복제된 인프라스트럭처에서 수행되므로 이전 상태로 롤백할 수 있으며, 예측 가능하고 일관된 환경을 제공한다.
- 불변적 인프라는 코드 기반으로 인프라를 관리하므로 자동화와 인프라스트럭처 버전 관리가 용이하다.
- 불변적 인프라는 가변적 인프라에 비해 일관성, 안정성, 확장성 등의 이점을 제공하지만, 새로운 인프라스트럭처를 배포하는 데 시간이 걸릴 수 있다. <br>
또한, 불변적 인프라를 구축하기 위해서는 인프라스트럭처를 코드로 관리하는 도구(예: Terraform)를 사용하고, 자동화된 프로세스를 수립해야한다.



### Terraform의 선언적 방식으로 작성된 코드는 항상 인프라의 최신 상태를 의미합니다. Terraform은 어떤 방식으로 인프라를 최신 상태로 유지할 수 있는 걸까요?

테라폼은 선언적(Declarative) 방식으로 인프라스트럭처를 관리한다. <br>
선언적 방식은 원하는 상태를 정의하고, 테라폼이 해당 상태를 달성하도록 요구하는 방식이다. <br>

상태 정의: 테라폼에서는 인프라스트럭처의 원하는 상태를 정의하는 "Terraform 코드"를 작성한다. <br>
이 코드는 인프라 리소스(예: 가상머신, 네트워크, 스토리지 등)와 그들 간의 관계를 표현하는 구성 요소로 구성된다.

변경 검출: 테라폼은 실행할 때마다 현재 인프라스트럭처와 정의된 상태 간의 차이를 검출한다. <br>
이를 통해 테라폼은 어떤 리소스를 생성, 수정 또는 삭제해야 하는지를 판단한다.

계획(Plan): 테라폼은 변경 사항을 적용하기 전에 "계획" 단계를 수행한다. <br>
이 단계에서는 변경 사항의 요약 정보를 제공하고, 어떤 리소스가 생성, 수정 또는 삭제될 것인지를 표시한다. <br>
계획을 검토하여 변경 사항을 미리 확인하고 승인할 수 있다.

변경 사항 적용: 계획이 승인되면 테라폼은 변경 사항을 적용한다. <br>
이 단계에서는 실제로 인프라스트럭처를 생성, 수정 또는 삭제하여 정의된 상태와 일치시킨다.

테라폼의 선언적 방식은 원하는 상태를 정의하고 테라폼이 상태를 달성하도록 책임을 맡긴다. <br>
이는 테라폼 코드가 중복성을 최소화하고 일관성을 유지할 수 있도록 도와주며, 변경 사항을 관리하고 롤백할 수 있는 기능을 제공한다. <br>
또한, 테라폼 코드는 버전 관리 시스템에 저장하여 변경 이력을 추적할 수 있다.

---
title:  "AWS 확장, 노출, 보안"
excerpt: "DevOps 부트캠프 Section 2 "

categories:
  - Blog
tags:
  - [Blog, DevOps]

toc: true
toc_sticky: true
 
date: 2023-04-18
last_modified_at: 2023-04-18
---
# Auto Scaling
클라우드 컴퓨팅의 대표적인 장점으로는 필요에 따라 서비스를 빠르게 확장하거나 축소할 수 있는 유연성을 들 수 있다.

그중, 오토스케일링(Auto Scaling)은 클라우드의 유연성을 돋보이게 하는 핵심기술로 CPU, 메모리, 디스크, 네트워크 트래픽과 같은 시스템 자원들의 메트릭(Metric) 값을 모니터링하여 서버 사이즈를 자동으로 조절 하는 서비스를 말한다.

이를 통해 사용자는 예상치 못한 서비스 부하에 효과적으로 대응하고, 최대한 저렴한 비용으로 안정적이고 예측 가능한 성능을 유지 할수 있다.

<br>

## 스케일링
스케일링 이란 인스턴스 혹은 컴퓨팅 파워를 늘리는 것을 말한다.
스케일링의 방법에는 대표적으로 2가지가 있다.

<br>

### 스케일 업(Scale Up)
cpu가 하나이고 memory 1기가인 인스턴스가 있다고 가정하자.<br>
만일 인스턴스의 성능을 16배로 늘려야할 상황이 온다면, 말 그대로 16배 큰 인스턴스를 사용함으로서 성능을 올리는 것을 스케일 업이라고 한다. <br>
하지만 이는 성능과 비용이 비례하지 않는다는 단점이 있다.

주변에서 간단히 예를 들면 그래픽카드를 볼 수 있다. <br>
GTX 1080과 RTX 2080의 성능 차이는 30%밖에 안되지만 가격 차이는 두배가 넘는다.<br>
거기다 만일 1,000,000배 높은 성능을 사용하고 싶다면 비용은 어마어마 할것이고, 물리적으로 가능한지도 고민 해봐야 한다.

<br>

### 스케일 아웃(Scale Out)
위의 물리적인 문제를 해결하는것이 바로 스케일 아웃 이다.

스케일 아웃은 규모를 늘리는 것이다.<br>
즉, 10만배 성능이 좋은 그래픽카드는 현실에서도 존재하지도 않으니 여러개 장착 해서 성능을 올려보자는 개념이다.<br>
하지만 이는 물리적으로 불가능하다. 메인보드에는 그래픽카드를 최대 2개밖에 장착하지 못하며, 만일 수십개를 장착할 수 있다고 해도 공간을 고려해야 되지 때문이다.

그러나 클라우드는 상황이 다르다. <br>
성능을 16배 올리고 싶으면 그냥 인스턴스를 16개 가져다가 쓰면 된다.<br>
공간의 제약이나 하드웨어적인 제약이 없다.<br>
그리고 성능과 비용이 비례하다는 특징도 가지고 있다. (그래픽카드 크로스파이어(글카 2개 장착) 같은경우 30%밖에 성능 향상폭이 미미하다)

따라서, 클라우드 환경에서는 Scale Out을 항상 염두하며 설계를 해야 한다.<br>
수요에 따라 인스턴스를 덜 쓸 수도, 더 쓸 수도 있음으로서 유연성을 가질 수 있기 때문이다.

<br>

### 스케일 인(Scale In)
scale out으로 늘린 인스턴스를 다시 줄이는 행위로 이해하면 된다.

이처럼, 오토 스케일링(Auto Scaling)은 바로 Scale Out을 자동화(Auto) 하기 위해 나온 서비스라고 보면 된다.<br>
애플리케이션을 모니터링 하고 용량을 자동으로 조정하는 역할을 하며, 최대한 저렴한 비용으로 안정적이고 예측 가능한 성능을 유지 한다.

그래서 몇 분만에 손쉽게 여러 서비스 전체에서 여러 리소스에 대해 애플리케이션 규모 조정을 설정 할 수 있다.

<br>

## 오토 스케일링의 목표
1. 정확한 수의 EC2 인스턴스를 보유하도록 보장
- 그룹의 최소 인스턴스 숫자와 최대 인스턴스 숫자를 관리
- 만약 애플리케이션을 실행하기 위해 인스턴스가 3개가 필요하다면, 3대 이상의 인스턴스가 항상 떠있을 수 있게 보장한다.
- 최소 숫자 이하로 내려가지 않도록 인스턴스 숫자를 유지 (인스턴스 추가)
- 최대 숫자 이상 늘어자니 않도록 인스턴스 숫자 유지 (인스턴스 삭제)
2. 다양한 스케일링 정책 적용 가능
- CPU의 부하에 따라 인스턴스 크기 늘리기/줄이기 (오후 2시가 되면 게임 접속 트래픽이 많으니 인스턴스 확 올리고, 새벽 2시가 되면 접속 트래픽이 낮으니 인스턴스를 내린다.)
3. 가용영역에 인스턴스가 골고루 분산될 수 있도록 인스턴스를 분배
- 서비스 장애가 발생하더라도 문제없이 서비스 이용

<br>

## 오토 스케일링의 장점
높은 가용성: Auto Scaling을 사용하면 애플리케이션의 인프라를 일정 수준 이상으로 자동으로 복제하므로 시스템 장애 발생 시 애플리케이션의 가용성이 높아진다. <br>
\예를 들어, 인스턴스 중 하나가 다운되면, Auto Scaling은 해당 인스턴스를 자동으로 종료하고 대신 새로운 인스턴스를 시작하여 시스템의 가용성을 유지한다.

비용 절감: Auto Scaling은 필요한 용량만큼 자동으로 조절하므로 인프라의 불필요한 사용을 줄일 수 있다.<br>
이렇게 하면 사용하지 않는 용량을 더 이상 지불하지 않아도 되므로 비용이 절감된다.

효율성 향상: Auto Scaling은 트래픽이 변동되는 상황에서도 시스템의 성능과 가용성을 유지할 수 있도록 자동으로 인스턴스를 스케일링한다. <br>
이렇게 하면 시스템 성능이 유지되면서 리소스의 효율성도 높아진다.

운영 간소화: Auto Scaling은 수동으로 운영해야 하는 부분을 자동화한다. <br>
인스턴스 시작, 중지, 종료, 스케일링 등의 작업을 자동으로 처리하여 운영 작업을 간소화하고 운영자의 작업 부담을 줄일 수 있다.

높은 탄력성: Auto Scaling은 인프라의 용량을 자동으로 조정하기 때문에 대규모 트래픽이 발생하더라도 시스템이 강력하게 대응할 수 있다. <br>
이렇게 하면 사용자의 요청에 대한 응답 시간이 늦어지는 것을 방지할 수 있다.

<br>

### 오토 스케일링 그룹(Groups)
EC2 인스턴스를 조정 및 관리 목적의 논리 단위로 취급될 수 있도록 그룹으로 구성<br>
그룹을 생성할 때 EC2 인스턴스의 최소(minimum size) 및 최대(maximum size) 인스턴스 수와 원하는(desired capacity) 인스턴스 수를 지정하고 이 범위안에서 Scale in/out 이 일어난다.<br>
인스턴스 증감은 이 그룹안에서 이루어 지게 되며, 사용자가 지정한 조건을 통해 실행된다.<br>
- 증설 시 어떤 인스턴스 템플릿을 이용할 것인지
- 얼마나 많은 서버를 필요로 하는지
- 어떤 값을 기반으로 모니터링해서, 인스턴스를 증설 또는 감축 할 것인지

추가적으로 증설된 인스턴스가 로드밸런서의 멤버로 연결되어야 한다면, 로드밸런서를 지정할 수도 있다.

<br>

## 오토 스케일링 동작 원리
1. 모니터링: Auto Scaling 그룹은 지정된 Amazon CloudWatch 지표를 주기적으로 평가하여 현재 인스턴스 용량을 모니터링한다. <br>
예를 들어, CPU 사용률, 네트워크 사용률, 요청 수 등을 모니터링할 수 있다.
2. 트리거: Auto Scaling 그룹은 CloudWatch 지표를 기반으로 트리거를 설정한다. <br>
이 트리거는 일정 지표 값 이상이거나 이하일 때, 스케일 업 또는 스케일 다운 작업을 수행하도록 설정할 수 있다.
3. 스케일링 정책: Auto Scaling 그룹은 스케일링 정책을 사용하여 어떻게 인스턴스를 추가하거나 제거할지를 결정한다. <br>
예를 들어, CPU 사용률이 일정 값 이상일 때 인스턴스를 추가하고, CPU 사용률이 일정 값 이하일 때 인스턴스를 제거하는 정책을 설정할 수 있다.
4. 스케일링: Auto Scaling 그룹은 스케일링 작업을 수행한다. <br>
스케일 업 작업을 수행할 때, Auto Scaling은 AMI(Amazon Machine Image)에서 새 인스턴스를 시작하고, 로드 밸런서에 등록한다. <br>
스케일 다운 작업을 수행할 때, Auto Scaling은 인스턴스를 종료하고, 로드 밸런서에서 제거한다.
5. 로드 밸런서: Auto Scaling 그룹은 로드 밸런서를 사용하여 트래픽을 인스턴스로 분산시킨다. <br>
이렇게 하면 트래픽이 증가하더라도 각 인스턴스에 대한 부하를 분산시켜 시스템의 가용성과 성능을 유지할 수 있다.

<br>

### 시작 템플릿
Auto Scaling 그룹에서 인스턴스를 시작하는데 사용하는 템플릿으로, AMI(이미지)라고 생각해도 된다.<br>
템플릿이라 함은 똑같은 OS환경의 인스턴스를 간편하게 복제하기 위해서 구성하는 것이다.<br>
따라서 템플릿을 오토스케일링 그룹에 지정시킴으로서, 오토스케일링을 통해 인스턴스 늘리면 그 인스턴스의 환경 구성이 템플릿에 설정된 환경에 따라 복제됨으로서 서비스를 늘린다는 개념 원리인 것이다.<br>
인스턴스의 AMI ID, 인스턴스 유형, 키 페어, 보안 그룹, 블록 디바이스 매핑 등의 정보를 세팅해서 템플릿을 구성하게 된다.<br>
템플릿을 구성하면 수정이 불가능하다. <br>
템플릿은 버젼으로 구분되는데, 만일 옵션을 바꿀 필요가 있으면 새로 생성을 하고 버젼을 새로 생성한 버젼으로 바꾸는 식으로 해야한다.

<br>

### 오토 스케일링 조정 옵션
Auto Scaling 그룹을 조정하는 다양한 조건 방법을 설정하는 옵션이다.<br>
예를들어, CPU 점유율이 일정 %를 넘었을 때 추가로 늘리거나 아니면 2개 이상이 필요한 스택에서 EC2 하나가 죽었을 때 실행하거나 등<br>
현재 인스턴스 수준 유지 관리, 수동 조정, 일정을 기반으로 조정, 온디맨드 기반 조정 등등 이 있다.<br>
CloudWatch이나 ELB(부하 분산)와 연계가 가능하다.<br>

<br>

### 스케일링 유형
- 인스턴스 레벨 유지<br>
기본 스케일링 계획으로도 부르며, 항상 실행 상태를 유지하고자 하는 인스턴스의 수를 지정할 수 있다.<br> 일정한 수의 인스턴스가 필요한 경우 최소, 최대 및 원하는 용량에 동일한 값을 설정하면 된다.<br>

- 수동 스케일링<br>
기존 Auto Scaling 그룹의 크기를 수동으로 변경할 수 있다. <br>
수동 스케일링을 선택하면 사용자가 직접 콘솔이나, API, CLI 등을 이용해 수동으로 인스턴스를 추가 또는 삭제 해야한다. <br>
해당 방식은 추천되지 않는다.

- 예측 스케일링<br>
트래픽의 변화를 예측할 수 있고, 특정 시간대에 어느 정도의 트래픽이 증가하는지 패턴을 파악하고 있다면 일정별 스케일링을 사용하는 것이 좋다. <br>
예를 들어 낮 시간대에는 트래픽이 최고치에 이르고 밤 시간대에는 트래픽이 거의 없다면 낮 시간대에 서버를 증설하고 밤 시간대에 스케일 다운 하는 규칙을 추가하면 된다.

- 동적 스케일링<br>
수요 변화에 대응하여 Auto Scaling 그룹의 용량을 조정하는 방법을 정의한다. <br>
이 방식은 CloudWatch가 모니터링 하는 지표를 추적하여 경보 상태일 때 수행할 스케일링 규칙을 정한다. <br>
예를 들어 CPU 처리 용량의 80% 수준까지 급등한 상태가 5분 이상 지속될 경우 Auto Scaling이 작동돼 새 서버를 추가하는 방식다. <br>
이와 같은 스케일링 정책을 정의 할 때는 항상 스케일 업과 스케일 다운 두 가지의 정책을 작성해야 한다.

<br>

### 동적 스케일링 정책
Target Tracking Scaling Policy: 이 정책은 지표(metric)를 기반으로 대상(target) 수준을 유지하는 것을 목표로 한다. <br>
예를 들어, CPU 사용률이 70%를 초과하면 인스턴스 수를 자동으로 늘리고, 30% 미만으로 떨어지면 인스턴스 수를 자동으로 줄인다. <br>
이 정책은 대상 지표를 설정하고, 대상 값(target value)과 대상 지표(target metric) 간의 관계를 정의한다.

Step Scaling Policy: 이 정책은 지표의 변화에 따라 일정한 단계(step)로 인스턴스 수를 늘리거나 줄인다. <br>
예를 들어, CPU 사용률이 70%를 초과하면 2개의 인스턴스를 추가하고, 30% 미만으로 떨어지면 1개의 인스턴스를 제거한다. 이러한 작업은 정의된 단계(step)에 따라 수행된다.

Simple Scaling Policy: 이 정책은 지표가 특정 임계값(threshold)을 초과하는 경우 인스턴스 수를 늘리고, 그렇지 않은 경우 인스턴스 수를 줄인다. <br>
예를 들어, CPU 사용률이 70%를 초과하면 1개의 인스턴스를 추가하고, 30% 미만으로 떨어지면 1개의 인스턴스를 제거한다. <br>
이러한 작업은 임계값(threshold)에 따라 수행된다.

<br>

### 인스턴스 삭제 정책
인스턴스 삭제 정책은 Auto Scaling 그룹에서 인스턴스를 어떻게 제거할 것인지 결정하는 방법이다. <br>
Auto Scaling 그룹에서 인스턴스를 제거하는 데에는 두 가지 방법이 있다.

- Oldest Instance Termination Policy: 이 정책은 가장 오래된 인스턴스부터 제거하는 방법이다. <br>
이 방법은 가장 이상적인 방법은 아니지만, 가장 단순하고 쉽게 구현할 수 있다.

- Balanced Instance Termination Policy: 이 정책은 여러 가지 요소를 고려하여 인스턴스를 제거한다.<br>
이 방법은 오래된 인스턴스보다는 다양한 요소를 고려하기 때문에 더욱 정확한 인스턴스 제거를 가능하게 한다.<br> 
이 방법은 다음과 같은 요소를 고려한다.

  - 가용 영역(Availability Zone)별로 인스턴스 수를 조정하여 해당 영역에 장애가 발생해도 인스턴스를 유지할 수 있도록 한다.
  - Auto Scaling 그룹 내에서 인스턴스가 어떤 역할을 하는지 고려하여 인스턴스를 제거한다.
  - 인스턴스의 상태를 고려하여 인스턴스를 제거한다.<br> 
  예를 들어, 이벤트 로그를 분석하여 가장 많이 오류가 발생한 인스턴스를 우선적으로 제거할 수 있다.

이러한 정책을 통해 Auto Scaling 그룹에서 인스턴스를 효과적으로 관리하고, 애플리케이션의 가용성을 유지할 수 있습니다.

[reference](https://inpa.tistory.com/entry/AWS-%F0%9F%93%9A-EC2-%EC%98%A4%ED%86%A0-%EC%8A%A4%EC%BC%80%EC%9D%BC%EB%A7%81-ELB-%EB%A1%9C%EB%93%9C-%EB%B0%B8%EB%9F%B0%EC%84%9C-%EA%B0%9C%EB%85%90-%EA%B5%AC%EC%B6%95-%EC%84%B8%ED%8C%85-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC): https://inpa.tistory.com/entry/AWS-%F0%9F%93%9A-EC2-%EC%98%A4%ED%86%A0-%EC%8A%A4%EC%BC%80%EC%9D%BC%EB%A7%81-ELB-%EB%A1%9C%EB%93%9C-%EB%B0%B8%EB%9F%B0%EC%84%9C-%EA%B0%9C%EB%85%90-%EA%B5%AC%EC%B6%95-%EC%84%B8%ED%8C%85-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC <br>
[reference](https://chat.openai.com): Chat GPT

<br><br><br>



# Elastic Load Balancer(ELB)
애플리케이션 트래픽을 Amazon EC2 인스턴스, 컨테이너, IP 주소, Lambda 함수, 가상 어플라이언스와 같은 여러 대상에 자동으로 분산시켜 안정적인 AWS서버 환경을 운용하는데에 도움을 주는 서비스 입니다.


## Elastic Load Balancer 기능
보안
- VPC를 사용할 경우 ELB와 관련된 보안 그룹을 생성 및 관리하여 ALB 및 CLB를 위한 추가 네트워킹 및 보안 옵션을 제공할 수 있다.
- 원하는 Load Balancer를 인터넷과 연결되도록 구성하거나 퍼블릭 IP 주소 없이 로드 밸런서를 생성하여 인터넷에 연결되지 않은 내부 로드 밸런서로 사용할 수 있다.

고가용성 및 탄력성
- 수신되는 트래픽을 단일 가용 영역 또는 여러 가용 영역의 Amazon EC2 인스턴스 전체에 걸쳐 배포할 수 있다.
- 수신되는 애플리케이션 트래픽에 대응하여 요청 처리 용량을 자동으로 조정한다.
- 대상이 사용 가능하고 정상 상태인지 확인하기 위해 구성 가능한 케이던스로 대상에 대해 상태 확인을 실행한다.
- 기본 애플리케이션 서버의 사용률에 따라 자동으로 추가되거나 제거된다.

높은 처리량
- 트래픽 증가를 처리할 수 있도록 설계되었으며 초당 수백만 개의 요청을 로드 밸런싱할 수 있다.
- 갑작스럽고 변동이 심한 트래픽 패턴을 처리할 수 있다.

상태 확인
- EC2 인스턴스, 컨테이너, IP 주소, 마이크로서비스, Lambda 함수, 어플라이언스 등 정상 상태인 대상으로만 트래픽을 라우팅한다.
- 두 가지 방식으로 애플리케이션 상태에 대한 통찰력을 개선할 수 있다.
  - 상태 확인 개선을 통해 상세한 오류 코드를 구성할 수 있다.
  - 새로운 지표로 EC2 인스턴스에서 실행되는 각 서비스의 트래픽을 파악할 수 있다.

고정 세션
- 요청을 동일한 클라이언트에서 동일한 대상으로 라우팅하는 메커니즘을 바탕으로 고정성은 대상 그룹 수준에서 정의된다.

운영 모니터링 및 로깅
- Amazon CloudWatch가 ALB와 CLB에 대해 요청 횟수, 오류 횟수, 오류 유형, 요청 지연 시간 등의 지표를 보고한다.
- NLB와 GLBa에 대해 활성 플로우 수, 새 플로우 수, 처리된 바이트 등의 지표를 추적합니다.

삭제 방지
- Elastic Load Balancer에서 삭제 방지를 활성화하여 우발적 삭제를 방지할 수 있다.

AWS로 마이그레이션
- 자동 조정 기능이 포함되어 있어 용량을 예측할 필요가 없고, 기존 애플리케이션과 클라우드 네이티브 애플리케이션 모두의 로드 밸런싱에 적합하다.
- Terraform, Ansible과 같이 이미 익숙한 일반적인 관리 도구와도 잘 통합된다.

하이브리드 클라우드 구축
- AWS와 온프레미스 리소스 전체에서 로드 밸런싱할 수 있는 기능을 제공한다.
- 모든 리소스를 동일한 대상 그룹에 등록하고 이 대상 그룹을 로드 밸런서와 연결하면 이를 달성할 수 있다.
- AWS와 온프레미스 리소스에 DNS 기반 가중치 적용 방식의 로드 밸런싱을 사용할 수도 있다.

서버리스 운영 및 컨테이너로 애플리케이션 현대화
- Lambda를 사용하는 경우 복잡한 구성을 거치거나 API 게이트웨이를 사용하지 않고도 ALB를 활용하여 네이티브 HTTP 기반 엔드포인트를 제공한다.
- Kubernetes를 사용한 컨테이너 및 컨테이너 오케스트레이션도 지원하여 클라이언트와 애플리케이션 간의 로드 밸런싱과 서비스 간 통신을 제공한다.

타사 가상 어플라이언스 확장
- GLB를 사용하면 클라우드에서 실행할 때의 확장성과 유연성을 활용하면서 원하는 공급업체의 어플라이언스를 배포할 수 있다.

  * AWS GLB(Global Load Balancer)는 여러 지역의 애플리케이션을 고가용성과 안정성을 보장하며 전 세계적으로 빠른 응답 시간을 제공하는 서비스이다.

통합 및 글로벌 접근성
- EC2, ECS/EKS, Global Accelerator 및 운영 도구 등의 다른 AWS 서비스와 긴밀하게 통합된다.

## 알고리즘
라운드 로빈 방식<br>
라운드 로빈은 클라이언트로부터 받은 요청을 로드밸런싱 대상 서버에 순서대로 할당받는 방식이다. <br>
첫 번째 요청은 첫 번째 서버, 두 번째 요청은 두 번째 서버, 세 번째 요청은 세 번째 서버에 할당한다. <br>
서버의 성능이 동일하고 처리 시간이 짧은 애플리케이션의 경우, 균등하게 분산이 이루어진다.

가중 라운드 로빈 방식<br>
가중 라운드 로빈 방식은 실제 서버에 서로 다른 처리 용량을 지정할 수 있다. <br>
각 서버에 가중치를 부여할 수 있으며, 여기서 지정한 정숫값을 통해 처리 용량을 정한다.

최소 연결 방식<br>
최소 연결 방식은 연결 수가 가장 적은 서버에 네트워크 연결방향을 정한다. <br>
동적인 분산 알고리즘으로 각 서버에 대한 현재 연결 수를 동적으로 카운트할 수 있고, 동적으로 변하는 요청에 대한 부하를 분산시킬 수 있다.

## Elastic Load Balancer 종류
Application Load Balancer (ALB)
- HTTP 및 HTTPS프로토콜과 1~65535의 TCP 포트, Lambda 호출, ECS를 사용하는 애플리케이션의 로드 밸런싱을 지원한다.
- 리디렉션, 고정 응답, 인증 및 전달을 위해 호스트 헤더, 경로, HTTP 헤더, 방법, 쿼리 매개 변수 및 소스 IP CIDR와 같은 규칙을 정할 수 있으며 규칙을 사용하여 특정 요청을 라우팅할 방법을 결정할 수 있다.
- ACM과 통합되면서 아주 간단하게 인증서를 로드 밸런서에 연결할 수 있어 SSL/TLS 인증서를 구매, 업로드 및 갱신하는 작업이 매우 간편하다.
- ALB가 실행된 시간 또는 부분 시간 그리고 시간당 사용된 로드 밸런서 용량 단위(LCU)에 대해 요금이 부과된다.

Network Load Balancer (NLB)
- IP 프로토콜 데이터를 기반으로 Amazon VPC 내의 대상(Amazon EC2 인스턴스, 마이크로서비스 및 컨테이너)으로 연결을 라우팅한다.
- TCP 및 UDP 트래픽을 이상적인 매우 짧은 대기 시간을 유지할 수 있다.
- 가용 영역당 단일 고정 IP 주소를 사용하여 갑작스럽고 변동성이 큰 트래픽 패턴을 처리하도록 최적화되어 있다.
- Auto Scaling, ECS, Amazon CloudFormation 및 ACM와 같은 다른 인기 있는 AWS 서비스와 통합가능하다.
- WebSocket 유형의 애플리케이션에 매우 유용한 장기간 연결도 지원한다.
- 단일 (공용/인터넷 IP, 프라이빗 IP)만 지원합니다.
- 컨테이너 내에서는 서로 다른 보안 그룹을 가질 수 있고, 여러 컨테이너에서 같은 포트를 사용할 수도 있다.
- NLB가 실행된 시간 또는 부분 시간 그리고 시간당 NLB에서 사용된 NLB용량 단위(NLCU)에 대해 요금이 부과된다.

Gateway Load Balancer (GWLB)
- 타사 가상 어플라이언스를 쉽게 배포, 확장 및 관리할 수 있다.
- 흐름을 소스 IP, 대상 IP, 프로토콜, 소스 포트 및 대상 포트로 구성된 5-튜플의 조합으로 여러 가상 어플라이언스에 트래픽을 분산하는 동시에 수요에 따라 확장하거나 축소할 수 있는 하나의 게이트웨이를 제공하여 네트워크의 잠재적인 실패 지점이 제거되고 가용성이 높아진다.
- 배포 프로세스를 간소화하여 현재와 동일한 공급업체와 협력하든 새로운 시도를 하든 관계없이 가상 어플라이언스의 가치를 더 빨리 확인할 수 있다.
- 서드 파티 가상 어플라이언스를 통한 모든 계층 3 트래픽을 투명하게 전달할 수 있다.
- GLB가 실행된 시간 또는 부분 시간 그리고 시간당 GLB에서 사용된 GLB 용량 단위(GLCU)에 대해 요금이 부과된다.
 
GLB는 AWS PrivateLink 기술을 기반으로 한 새로운 유형의 VPC 종단점인 GLB 엔드포인트(GWLBE)를 사용하며, 이 엔드포인트는 애플리케이션이 VPC 경계에서 GWLB와 트래픽을 안전하게 교환하는 방법을 간소화한다.<br> GWLBE에는 별도의 요금이 책정 및 청구됩니다

Classic Load Balancer (CLB)
- 여러 EC2 인스턴스에서 기본 로드 밸런싱을 제공하고 요청 수준과 연결 수준 모두에서 작동한다.
- EC2 네트워크 내에 구축된 애플리케이션을 HTTP 포트 80과 HTTPS 포트 443을 단일로 매핑 할 수 있다.
- HTTP, HTTPS(보안 HTTP), SSL(보안 TCP) 및 TCP 프로토콜을 사용하여 애플리케이션의 로드 밸런싱을 지원해준다.
- ACM과 통합되면서 아주 간단하게 인증서를 로드 밸런서에 연결할 수 있어 SSL/TLS 인증서를 구매, 업로드 및 갱신하는 작업이 매우 간편하다.
- CLB가 실행된 각 시간 또는 한 시간 미만, 그리고 로드 밸런서를 통해 전송된 각 GB 단위 데이터에 대해 비용이 청구된다.


## Cross Zone Load Balancing
AWS Load Balancer에서 제공하는 기능 중 하나로, 각각의 인스턴스가 속한 가용 영역에 상관 없이, 모든 가용 영역에 걸쳐서 트래픽을 분산시키는 기능이다.

일반적으로 각 가용 영역에는 로드 밸런서에 대한 리소스가 있으며, 이를 통해 트래픽이 분산된다. <br>
그러나 Cross Zone Load Balancing을 사용하면, 각 인스턴스는 서로 다른 가용 영역에 있더라도 로드 밸런서의 모든 리소스를 사용할 수 있으므로 더욱 효율적인 트래픽 분산이 가능하다.

Cross Zone Load Balancing을 사용하면, 가용 영역 간에 불균형한 트래픽 분산 문제를 해결할 수 있으며, 특정 가용 영역에서 장애가 발생하더라도 다른 가용 영역에서 트래픽을 처리할 수 있어 애플리케이션 가용성을 높일 수 있다.

단점으로는, Cross Zone Load Balancing을 사용하면 로드 밸런서의 성능에 영향을 미칠 수 있다는 것이다. <br>
특히 로드 밸런서와 인스턴스 간에 네트워크 대역폭이 충분하지 않은 경우, Cross Zone Load Balancing을 사용하면 인스턴스 간 트래픽이 늘어나기 때문이다. <br>
따라서 특정 상황에서는 Cross Zone Load Balancing을 사용하지 않는 것이 좋을 수 있다.


## Sticky Session
AWS Load Balancer에서 제공하는 기능 중 하나로, 로드 밸런서에서 클라이언트 요청을 처리할 때, 특정한 클라이언트의 요청을 항상 같은 인스턴스로 보내는 기능이다.

일반적으로 클라이언트가 로드 밸런서에 요청을 보내면, 로드 밸런서는 요청을 받은 후 특정 인스턴스에게 전달한다.<br> 
이때 Sticky Session을 사용하면, 로드 밸런서는 클라이언트가 처음 요청을 보낼 때 할당한 쿠키 정보를 사용하여, 이후에 같은 클라이언트의 요청이 들어올 경우 항상 같은 인스턴스로 보내게 된다.

Sticky Session은 세션 기반 애플리케이션에서 사용되는 경우가 많다. <br> 
예를 들어, 사용자가 로그인한 후에 다음 페이지로 이동하거나 다른 요청을 보낼 때, 세션 정보를 유지해야 하는 경우가 있다. <br> 
이때 Sticky Session을 사용하면, 같은 사용자의 요청은 항상 같은 인스턴스로 전달되므로, 세션 정보가 유지되어 사용자가 로그아웃하기 전까지 계속해서 같은 인스턴스로 요청을 처리할 수 있다.

Sticky Session은 인스턴스 간의 부하 분산을 최적화하지 못할 수 있다. <br>
예를 들어, Sticky Session을 사용하면 일부 인스턴스는 다른 인스턴스보다 더 많은 요청을 받게 되므로, 전체적인 부하 분산이 고르게 이루어지지 않을 수 있다.<br>
따라서 Sticky Session을 사용할 때는, 이러한 문제를 고려하여 적절한 인스턴스 크기와 수를 선택하는 것이 중요하다.

<br><br><br>

# AWS 서비스 노출 - CloudFront
클라우드프론트는 개발자 친화적 환경에서 짧은 지연 시간과 빠른 전송 속도로 데이터, 동영상, 애플리케이션 및 API를 전세계 고객에게 안전하게 전송하는 고속 콘텐츠 전송 네트워크(CDN) 서비스이다.

엣지로케이션에 내 서비스를 등록하는 AWS 서비스가 바로 CloudFront이다.

CloudFront는 CDN 서비스와 이외에도 기본 보안 기능(Anti-DDoS)을 제공한다.

<br>

## CDN 이란?
CDN(Content Delivery Network or Content Distribution Network, 콘텐츠 전송 네트워크) 은 콘텐츠를 효율적으로 전달하기 위해 여러 노드를 가진 네트워크에 데이터를 저장하여 제공하는 시스템이다.

인터넷 서비스 제공자(ISP,Internet Service Provider)에 직접 연결되어 데이터를 전송하므로, 콘텐츠 병목을 피할 수 있는 장점이 있다.

우리가 프론트 공부할때 제이쿼리 같은 라이브러리를 링크 src로 불러온적이 있을텐데, 이게 바로 CDN 서비스를 이용하는 것이라고 보면 된다

<br>

### 특징
- 웹 페이지, 이미지, 동영상 등의 컨텐츠를 본래 서버에서 받아와 캐싱
- 해당 컨텐츠에 대한 요청이 들어오면 캐싱해 둔 컨텐츠를 제공
- 컨텐츠를 제공하는 서버와 실제 요청 지점 간의 지리적 거리가 매우 먼 경우 or 통신 환경이 안좋은 경우
→ 요청지점의 CDN을 통해 빠르게 컨텐츠 제공 가능
- 서버의 요청이 필요 없기 때문에 서버의 부하를 낮추는 효과

<br>

## 엣지 로케이션
- 컨텐츠가 캐싱되고 유저에게 제공되는 지점
- AWS가 CDN 을 제공하기 위해서 만든 서비스인 CloudFront의 캐시 서버 (데이터 센터의 전 세계 네트워크)
- cloudFront 서비스는 엣지 로케이션을 통해 콘텐츠를 제공
- CloudFront를 통해 서비스하는 콘텐츠를 사용자가 요청하면 지연 시간이 가장 낮은 엣지 로케이션으로 라우팅되므로 콘텐츠 전송 성능이 뛰어나다.
- 콘텐츠가 이미 지연 시간이 가장 낮은 엣지 로케이션에 있는 경우 CloudFront가 콘텐츠를 즉시 제공

<br>

## CloudFront 동작방식
CloudFront는 AWS 백본 네트워크를 통해 콘텐츠를 가장 효과적으로 서비스할 수 있는 엣지로 각 사용자 요청을 라우팅하여 콘텐츠 배포 속도를 높인다.

일반적으로 CloudFront 엣지가 최종 사용자에게 가장 빨리 제공한다.

1. 콘텐츠가 엣지 로케이션에 없는 경우
- CloudFront는 콘텐츠의 최종 버전에 대한 소스로 지정된 오리진(Amazon S3 버킷, MediaPackge 채널, HTTP 서버(예 : 웹 서버)등) 에서 콘텐츠를 검색
- 컨텐츠를 제공하는 근원에서 제공받아 전달
2. 콘텐츠가 엣지 로케이션에 있는 경우
- 바로 전달

<br>

## CloudFront 데이터 전달 과정
1. 클라이언트로부터 Edge Server로의 요청이 발생한다.
2. Edge Server는 요청이 발생한 데이터에 대하여 캐싱 여부를 확인
3. 사용자의 근거리에 위치한 Edge Server 중 캐싱 데이터가 존재한다면 사용자의 요청에 맞는 데이터를 응답<br>
사용자의 요청에 적합한 데이터가 캐싱되어 있지 않은 경우 Origin Server로 요청이 포워딩
4. 요청받은 데이터에 대해 Origin Server에서 획득한 후 Edge Server에 캐싱 데이터를 생성하고, 클라이언트로 응답이 발생

<br>

## CloudFront 장점
- AWS 네트워크를 사용하면 사용자의 요청이 반드시 통과해야 하는 네트워크의 수가 줄어들어 성능이 향상
- 파일의 첫 바이트를 로드하는 데 걸리는 지연 시간이 줄어들고 데이터 전송 속도가 빨라진다.
- 파일(객체)의 사본이 전 세계 여러 엣지 로케이션에 유지(또는 캐시)되므로 안정성과 가용성이 향상
- 보안성 향상
  - 오리진 서버에 대한 종단 간 연결의 보안이 보장됨(https)
  - 서명된 URL 및 쿠키 사용 옵션으로 자체 사용자 지정 오리진에서 프라이빗 콘텐츠를 제공하도록 할 수 있음


## CloudFront 기능
### 정적 & 동적 컨텐츠 분별 제공
#### 정적(Static) 컨텐츠
- 서버를 거치지 않고 클라이언트에서 직접 보여주는 내용<br>
ex) 이미지, CSS, 기타 서버가 필요없는 내용들
캐싱으로 접근속도 최적화

#### 동적(Dynamic) 컨텐츠
- 서버 계산, DB조회 등이 필요한 내용<br>
ex) 로그인, 게시판 등
- 네트워크 최적화, 연결 유지, Gzip 압축 등을 사용
- 서버랑 통신을 할 때 전처리 작업이 있는데, 주소가 어디로 전달되는지(DNS Lookup), TCP Connection, Titm to First Byte 등을 CloudFront에서 네트워크를 최적화한다.
- 실제로 내용을 최적화 해서 보내는 것이 아니라, 통신을 최적화 해서 속도를 최적화 시키는 것

### 동적 / 정적 컨텐츠 처리
경로 패턴으로 URL에 따라 정적/동적 컨텐츠 분기 처리 한다.

### HTTPS 지원
Origin에서 HTTPS를 지원하지 않더라도 클라우드 프론트내에서 HTTPS 통신을 지원할 수 있도록 구성 가능

예를 들어, S3 정적 웹 호스팅 URL 같은 경우 SSL설정이 쉽지 않은데, CloudFront를 통해서 HTTPS 통신을 지원할수 있게끔 할 수 있다.


### 지리적 제한 설정
특정 지역의 컨텐츠 접근을 제한 가능 하다.

예를들어 아프키라 티비 스트리밍 서비스를 하는데 라이센스나 계약에 따라 일본권에서는 볼 수 있지만, 아프리카권은 볼수 없게 설정 가능하다.


### 다른 서비스와 연계
AWS WAF, Lambda@Edge 등과 연동 가능

Lambda@Edge
- 엣지 로케이션에서 돌아가는 람다
- 람다엣지 사용 사례 :
  - 한국에서 요청이 올 경우 한국 웹서버, 미국에서 요청이 올 경우 미국 웹서버로 분산
  - 커스텀 에러 페이지
  - Cookie를 검사해 다른 페이지로 리다이렉팅 → A/B 테스팅
  - CloudFront에서 Origin 도착 이전에 인증 ..등


### CloudFront 리포팅
- 주요 CloudFront 이용 지표 확인 가능<br>
ex) 캐시 상태, 가장 많이 요청 받은 컨텐츠, Top Referrer(어디를 타고 들어와서 어느 웹사이트를 보고있는지..)
- 구글 애널리틱스 같은거라고 보면 된다.


### CloudFront 뷰어 정보
CloudFront에서 뷰어의 정보를 헤더에 더해 Origin에 전송한다.

- 디바이스 타입 (Android / IOS / SmartTV / Desktop / Tablet)
- IP Address
- Country / 도시 / 위도 / 경도 / 타입존 등

<br><br>

## CloudFront 정책과 보안
### CloudFront 정책
CloudFront는 총 3가지 정책 설정 가능하다.

1. 캐시 정책 (Cache Control) : 캐싱 방법 및 압축
- TTL 및 Cache Key 정책
- CloudFront가 어떻게 캐싱을 할지를 결정
2. 원본 요청 정책 (Origin Request) : Origin으로 어떤 내용을 보낼 것인가
- Origin에 쿠키, 헤더, 쿼리스트링 중 어떤 것을 보낼 것인가
3. 응답 헤더 정책
- CloudFront가 응답과 함께 실어 보낼 HTTP Header

<br>

### CloudFront 보안
#### Signed URL
어플리케이션에서 CloudFront의 컨텐츠에 접근 할 수 있는 URL을 제공하여 컨텐츠 제공을 제어하는 방법이다.

- URL에는 시작시간, 종료시간, IP, 파일명, URL의 유효기간 등의 정보를 담을 수 있음
- 이 URL 접근 이외의 접근을 막고 허용된 유저에게만 URL을 전달하여 컨텐츠 제공을 제어 가능
- 단 하나의 파일 또는 컨텐츠에 대한 허용만 가능
- S3 Signed URL과 비슷한 방식


#### Signed Cookie
Signed URL은 하나의 컨텐츠만 제공 제어를 한다고 하면, Signed Cookie는 다수의 컨텐츠의 제공방식을 제어하고 싶을 때 사용된다.

- Signed URL과 마찬가지로 여러 제약사항 설정 가능
- 다수의 파일 및 스트리밍 접근 허용 가능
- 사용사례 : 정기 구독 프리미엄 유저만 볼 수 있는 강의 동영상 등


#### Origin Access Identity (OAI)
S3의 컨텐츠를 CloudFront를 사용해서만 볼 수 있도록 제한하는 방법이다.

즉, S3의 정적인 컨텐츠 URL로 바로 접근하는게 아니라 CDN을 통해서 접근하는 것이다. 

왜냐하면 S3로 직접 접속을 하면 캐싱을 못해 속도 측면에서 마이너스가 될수 있고, 국가별 라우팅, 인증 등이 CloudFront에 구현 되어있다면 유저가 직접 S3로 접속하면 안되기 때문이다.

- S3는 CloudFront와 잘 맞는다 → 정적인 컨텐츠를 호스팅 하기 때문에 CDN과 찰떡궁합
- CloudFront만 권한을 가지고 S3에 접근하고 나머지 접근권한은 막음 → CloudFront는 유저와 S3사이에서 중개하는 역할
- S3 Bucket Policy로 CloudFront의 접근을 허용해야 사용 가능


#### Field Level Encryption
- CloudFront로부터 Origin 사이의 통신을 암호화
- 최대 10개의 필드까지 가능
- 공개키 방식으로 암호화 → CloudFront에 공개키를 제공 후 Origin에서 Private Key로 해독

[reference](https://inpa.tistory.com/entry/AWS-%F0%9F%93%9A-CloudFront-%EA%B0%9C%EB%85%90-%EC%9B%90%EB%A6%AC-%EC%82%AC%EC%9A%A9-%EC%84%B8%ED%8C%85-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC): https://inpa.tistory.com/entry/AWS-%F0%9F%93%9A-CloudFront-%EA%B0%9C%EB%85%90-%EC%9B%90%EB%A6%AC-%EC%82%AC%EC%9A%A9-%EC%84%B8%ED%8C%85-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC


<br><br>

# Route 53
Amazon Route 53 은 가용성과 확장성이 뛰어난 클라우드 Domain Name System (DNS) 웹 서비스이다.

Route 53는 도메인 구입부터 네임서버 등록까지 dns에 필요한 모든 기능이 있고, aws 답게 추가로 모니터링 기능까지 제공한다. 

다른 도메인 등록 기관(가비아, 후이즈 등) 에 비해 가격이 비슷하거나 저렴하고, 등록 외에 부가적인 기능 제공 및 안정성, GUI를 제공해 관리가 수월하다.

또한, Route 53 은 사용자의 요청을 Amazon EC2 인스턴스, Elastic Load Balancing 로드 밸런서, Amazon S3 버킷 등 AWS 에서 실행되는 인프라에 효과적으로 연결하고, AWS 외부의 인프라로 라우팅하는데도 Route 53 사용이 가능하다.


## Route 53 장점
- 가용성과 확장성이 뛰어난 클라우드 기반 DNS 웹 서비스
- 동적으로 사용자에게 노출될 DNS 레코드 타입과 값 조정
- 각종 다양한 로드밸런싱 기능 지원
- 사용자의 요청을 EC2, ELB, S3 버킷 등 인프라로 직접 연결 가능
- 외부의 인프라로 라우팅하는데 사용 가능
- Route 53 트래픽 흐름을 사용하염ㄴ 지연시간 기반 라우팅 가능
- Route 53에서는 도메인 이름 등록도 지원 가능

[reference](https://inpa.tistory.com/entry/AWS-%F0%9F%93%9A-Route-53-%EA%B0%9C%EB%85%90-%EC%9B%90%EB%A6%AC-%EC%82%AC%EC%9A%A9-%EC%84%B8%ED%8C%85-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC): https://inpa.tistory.com/entry/AWS-%F0%9F%93%9A-Route-53-%EA%B0%9C%EB%85%90-%EC%9B%90%EB%A6%AC-%EC%82%AC%EC%9A%A9-%EC%84%B8%ED%8C%85-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC

<br><br>

## Route 53 주요 DNS Record 유형
- A 레코드: 도메인 이름과 IPv4 주소를 연결합니다. 
```
example.com.    300     IN      A       192.0.2.1
```
- AAAA 레코드: 도메인 이름과 IPv6 주소를 연결합니다.
```
example.com.    300     IN      AAAA    2001:0db8:85a3:0000:0000:8a2e:0370:7334
```
- CNAME 레코드: 도메인 이름과 다른 도메인 이름을 연결합니다.
```
www.example.com. 300     IN      CNAME   example.com.
```
- MX 레코드: 이메일을 처리하는 서버의 우선순위와 호스트 이름을 지정합니다.
```
example.com.    300     IN      MX      10 mail.example.com.
```
- NS 레코드: 도메인 이름에 대한 권한 있는 이름 서버를 지정합니다.
```
example.com.    172800  IN      NS      ns-1234.awsdns-12.co.uk.
```
- PTR 레코드: IP 주소와 호스트 이름을 연결합니다.
```
1.2.0.192.in-addr.arpa. 300 IN    PTR     mail.example.com.
```
- SOA 레코드: DNS 존에 대한 기본 정보를 지정합니다.
```
example.com.    900     IN      SOA     ns-1234.awsdns-12.co.uk. hostmaster.example.com. 1 7200 900 1209600 86400
```
- SRV 레코드: 도메인 이름에서 서비스 인스턴스의 위치를 지정합니다.
```
_ldap._tcp.example.com.  300     IN      SRV     0 100 389 ldap.example.com.
```
- TXT 레코드: 도메인 이름에 대한 추가 정보를 지정합니다.
```
example.com.    300     IN      TXT     "v=spf1 include:_spf.example.com ~all"
```


## Route 53 라우팅 정책
단순 라우팅 정책(Simple routing policy) - 도메인에 대해 특정 기능을 수행하는 하나의 리소스만 있는 경우(예: example.com 웹 사이트의 콘텐츠를 제공하는 하나의 웹 서버)에 사용합니다. 단순 라우팅을 사용하여 프라이빗 호스팅 영역에서 레코드를 생성할 수 있습니다.

장애 조치 라우팅 정책(Failover routing policy) - 액티브-패시브 장애 조치를 구성하려는 경우에 사용합니다. 장애 조치 라우팅을 사용하여 프라이빗 호스팅 영역에서 레코드를 생성할 수 있습니다.

지리 위치 라우팅 정책(Geolocation routing policy) - 사용자의 위치에 기반하여 트래픽을 라우팅하려는 경우에 사용합니다. 지리적 위치 라우팅을 사용하여 프라이빗 호스팅 영역에서 레코드를 생성할 수 있습니다.

지리 근접 라우팅 정책(Geoproximity routing policy) - 리소스의 위치를 기반으로 트래픽을 라우팅하고 필요에 따라 한 위치의 리소스에서 다른 위치의 리소스로 트래픽을 보내려는 경우에 사용합니다.

지연 시간 라우팅 정책 - 여러 AWS 리전에 리소스가 있고 최상의 지연 시간을 제공하는 리전으로 트래픽을 라우팅하려는 경우에 사용합니다. 지연 시간 라우팅을 사용하여 프라이빗 호스팅 영역에서 레코드를 생성할 수 있습니다.

Certificate ManagerIP 기반 라우팅 정책 - 사용자의 위치에 기반하여 트래픽을 라우팅하고 트래픽이 시작되는 IP 주소가 있는 경우에 사용합니다.

다중 응답 라우팅 정책(Multivalue answer routing policy) - Route 53이 DNS 쿼리에 무작위로 선택된 최대 8개의 정상 레코드로 응답하게 하려는 경우에 사용합니다. 다중 값 응답 라우팅을 사용하여 프라이빗 호스팅 영역에서 레코드를 생성할 수 있습니다.

가중치 기반 라우팅 정책(Weighted routing policy) - 사용자가 지정하는 비율에 따라 여러 리소스로 트래픽을 라우팅하려는 경우에 사용합니다. 가중치 라우팅을 사용하여 프라이빗 호스팅 영역에서 레코드를 생성할 수 있습니다.

[reference](https://docs.aws.amazon.com/ko_kr/Route53/latest/DeveloperGuide/routing-policy.html): https://docs.aws.amazon.com/ko_kr/Route53/latest/DeveloperGuide/routing-policy.html

<br><br>

# 보안- AWS Certificate Manager
AWS Certificate Manager(ACM)는 AWS에서 제공하는 SSL/TLS 인증서 관리 서비스이다.<br> 
ACM을 사용하면 인증서 발급, 관리 및 자동 갱신을 쉽게 처리할 수 있으며, AWS에서 호스팅하는 애플리케이션에 쉽게 적용할 수 있다.

ACM은 다음과 같은 기능을 제공한다.

무료 공인 SSL/TLS 인증서 발급: ACM을 사용하면 무료로 공인 SSL/TLS 인증서를 발급받을 수 있다. <br>
이 인증서는 AWS에서 호스팅하는 애플리케이션에 사용할 수 있으며, 이를 통해 데이터 보안성을 향상시킬 수 있다.

간편한 관리 및 자동 갱신: ACM을 사용하면 발급받은 인증서를 간편하게 관리할 수 있다.<br> 
ACM은 인증서의 만료일을 자동으로 감지하고, 인증서를 자동으로 갱신하여 유효성을 유지한다. <br>
이를 통해 인증서 관리에 필요한 시간과 노력을 줄일 수 있다.

다양한 통합 지원: ACM은 AWS CloudFront, Elastic Load Balancing, API Gateway 등 AWS 서비스와 쉽게 통합된다. <br>
이를 통해 HTTPS를 사용하는 모든 애플리케이션에서 쉽게 인증서를 사용할 수 있다.

고객 데이터 보호: ACM은 고객 데이터 보호를 위해 안전한 저장소에 인증서를 저장한다. <br>
이를 통해 인증서 관리와 보안에 대한 우려를 덜 수 있다.

ACM은 AWS에서 호스팅하는 애플리케이션에서 SSL/TLS 인증서를 사용하는 데 있어서 매우 유용한 서비스이다. <br>
ACM을 사용하면 간편하게 인증서를 발급하고, 관리하며, 애플리케이션의 보안성을 향상시킬 수 있다.

<br>

## Identity and Access Management(IAM)
AWS 리소스에 대한 액세스를 안전하게 제어할 수 있는 웹 서비스이다. <br>
IAM을 사용하면 사용자가 액세스할 수 있는 AWS 리소스를 제어하는 권한을 중앙에서 관리할 수 있다. <br>
IAM을 사용하여 리소스를 사용하도록 인증(로그인) 및 권한 부여(권한 있음)된 대상을 제어한다.

AWS 계정을 생성할 때는 해당 계정의 모든 AWS 서비스 및 리소스에 대한 완전한 액세스 권한이 있는 단일 로그인 ID로 시작한다. <br>
이 자격 증명은 AWS 계정 루트 사용자라고 하며, 계정을 생성할 때 사용한 이메일 주소와 암호로 로그인하여 액세스한다. <br>
일상적인 태스크에 루트 사용자를 사용하지 않을 것을 강력히 권장하며, 루트 사용자 보안 인증 정보를 보호하고 루트 사용자만 수행할 수 있는 작업을 수행하는 데 사용한다. <br>

## IAM 기능
- AWS 계정에 대한 공유 액세스<br>
암호나 액세스 키를 공유하지 않고도 AWS 계정의 리소스를 관리하고 사용할 수 있는 권한을 다른 사람에게 부여할 수 있다.

- 세분화된 권한<br>
리소스에 따라 여러 사람에게 다양한 권한을 부여할 수 있다. <br>
예를 들어 일부 사용자에게 Amazon Elastic Compute Cloud(Amazon EC2), Amazon Simple Storage Service(Amazon S3), Amazon DynamoDB, Amazon Redshift 및 기타 AWS 서비스에 대한 완전한 액세스를 허용할 수 있다. <br>
다른 사용자에게는 일부 S3 버킷에 대한 읽기 전용 권한, 일부 EC2 인스턴스를 관리할 수 있는 권한 또는 결제 정보에만 액세스할 수 있는 권한을 허용할 수 있다.

- Amazon EC2에서 실행되는 애플리케이션을 위한 보안 AWS 리소스 액세스<br>
EC2 인스턴스에서 실행되는 애플리케이션의 경우 IAM 기능을 사용하여 자격 증명을 안전하게 제공할 수 있다. <br>
이러한 자격 증명은 애플리케이션에 다른 AWS 리소스에 액세스할 수 있는 권한을 제공합니다. 예를 들면 이러한 리소스에는 S3 버킷 및 DynamoDB 테이블이 있다.

- 멀티 팩터 인증(MFA)<br>
보안 강화를 위해 계정과 개별 사용자에게 2팩터 인증을 추가할 수 있다. <br>
MFA를 사용할 경우 계정 소유자나 사용자가 계정 작업을 위해 암호나 액세스 키뿐 아니라 특별히 구성된 디바이스의 코드도 제공해야 한다. <br>
이미 다른 서비스와 함께 FIDO 보안 키를 사용하고 있으며 FIDO 보안 키의 구성에 AWS가 지원되는 경우 MFA 보안을 위해 WebAutn을 사용할 수 있다. 

- 아이덴티티 페더레이션<br>
기업 네트워크나 인터넷 자격 증명 공급자와 같은 다른 곳에 이미 암호가 있는 사용자에게 AWS 계정에 대한 임시 액세스 권한을 부여할 수 있다.

- 보장을 위한 자격 증명 정보<br>
AWS CloudTrail을 사용하는 경우 계정의 리소스를 요청한 사람에 대한 정보가 포함된 로그 레코드를 받게된다. <br>
이 정보는 IAM 자격 증명을 기반으로 한다.

- PCI DSS 준수<br>
IAM에서는 판매자 또는 서비스 공급자에 의한 신용카드 데이터의 처리, 저장 및 전송을 지원하며, Payment Card Industry(PCI) Data Security Standard(DSS) 준수를 검증받았다.

- 많은 AWS 서비스와의 통합<br>
IAM과 함께 사용할 수 있는 AWS 서비스의 목록은 AWS IAM으로 작업하는 서비스 섹션을 참조하세요.

- 최종 일관성
IAM은 다른 많은 AWS 서비스처럼 최종 일관성이 있다. <br>
IAM은 전 세계 Amazon 데이터 센터 내의 여러 서버로 데이터를 복제함으로써 고가용성을 구현한다. <br>
일부 데이터를 변경하겠다는 요청이 성공하면 변경이 실행되고 그 결과는 안전하게 저장된다.<br> 
그러나 변경 사항은 IAM 전체에 복제되어야 하고, 이 작업에는 어느 정도 시간이 소요된다.<br>
그러한 변경 사항에는 사용자, 그룹, 역할 또는 정책을 만들거나 업데이트한 것이 포함된다.<br>
그러한 IAM 변경 사항을 애플리케이션의 중요한 고가용성 코드 경로에 포함시키지 않는 것이 좋다. <br>
대신 자주 실행하지 않는 별도의 초기화 루틴이나 설정 루틴에서 IAM을 변경해야하며 또한 프로덕션 워크플로우에서 변경 사항을 적용하기 전에 변경 사항이 전파되었는지 확인해야 한다. <br>
자세한 내용은 변경 사항이 매번 즉시 표시되는 것은 아닙니다 섹션을 참조하세요.


### IAM 자격 증명(사용자, 사용자 그룹 및 역할)
AWS 계정 루트 사용자 또는 계정의 관리 사용자가 IAM 자격 증명을 생성할 수 있다.<br>
IAM 자격 증명을 통해 AWS 계정에 액세스할 수 있다. <br>
IAM 사용자 그룹은 하나의 단위로 관리되는 IAM 사용자 집합이다. <br>
IAM 자격 증명은 인간 사용자 또는 프로그래밍 워크로드를 대표하며, 인증된 후 AWS에서 작업을 수행할 수 있는 권한을 부여받는다.<br>
각 IAM 자격 증명은 하나 이상의 정책과 연결될 수 있다. <br>
정책은 사용자, 역할 또는 사용자 그룹 멤버가 수행할 수 있는 작업, 작업의 대상 AWS 리소스 및 작업 수행 조건을 결정한다.

### IAM 역할
- AWS 어카운트 관리 및 리소스/유저/서비스의 권한제어
  - 임시 권한 부여
  - 서비스 사용을 위한 인증 정보 부여
- 유저의 생성 및 관리 및 계정의 보안
  - Multi-factor Authentication(유저가 로그인 할 때 일반적으로 사용하는 id, pwd 뿐만 아니라 디바이스로부터 토큰형식의 임시 pwd를 받아 로그인 하는 방식으로 은행에서 많이 사용)
  - 유저의 패스워드 정책 관리
- 다른 계정과의 리소스 공유
- Identity Federation(Facebook 로그인, 구글 로그인 등 다른 인증수단 설정)

### IAM 구성
- 유저: 실제 AWS 서비스를 이용하는 사람
  - Access Key/ Secret Access key: 유저가 AWS의 서비스를 사용하기 위한 인증정보(유저명, 패스워드랑은 다른 개념)(Secret Access Key는 발급시 한번 보여주기 때문에 잘 적어둬야 한다.)
  - 유저명 / 패스워드: 유저가 AWS 콘솔을 사용하기 위한 인증정보
- 그룹: 유저의 집합. 그룹에 속한 유저는 그룹에 부여된 권한을 행사할 수 있다.
- 정책(policy): JSON 형식으로 정의되며, 유저와 그룹, 자격이 무엇을 할 수 있는지에 나와있는 문서(태그를 통해서 나누는 것도 가능하다)
- 자격(Role): AWS 리소스에 부여하여 AWS 리소스가 무엇을 할 수 있는지 정의(EC2가 S3에 있는 이미지를 가져오고 싶을 때 EC2에 부여하는 것이 자격이다. 자격을 갖추면 S3 파일에 접근 할 권한을 얻는다. 대부분의 문제는 자격 문제로 알맞은 권한이 부여됐는지 확인해야 한다.)
  - 다른 자격에 대해서 신뢰관계 구축 가능
  - 역할을 바꾸어가며 서비스 사용 가능

#### AWS 계정 루트 사용자
AWS 계정을 생성할 때는 해당 계정의 모든 AWS 서비스 및 리소스에 대한 완전한 액세스 권한이 있는 단일 로그인 ID로 시작한다. <br>
이 자격 증명은 AWS 계정 루트 사용자라고 하며, 계정을 생성할 때 사용한 이메일 주소와 암호로 로그인하여 액세스한다.

#### IAM 사용자
IAM 사용자는 단일 개인 또는 애플리케이션에 대한 특정 권한을 가지고 있는 AWS 계정 내 자격 증명이다. <br>
모범 사례로서, 가능하면 암호 및 액세스 키와 같은 장기 보안 인증 정보가 있는 IAM 사용자를 생성하는 대신 임시 보안 인증을 사용하는 것이 좋다. <br>
하지만 IAM 사용자의 장기 보안 인증이 필요한 특정 사용 사례가 있는 경우 액세스 키를 교체하는 것이 좋다. <br>
AWS 계정에 IAM 사용자를 추가하려면 AWS 계정에서 IAM 사용자 생성 섹션을 참조하세요.


#### IAM 사용자 그룹
IAM 그룹은 IAM 사용자 컬렉션을 지정하는 자격 증명이다. <br>
귀하는 그룹을 사용하여 로그인할 수 없다. <br>
그룹을 사용하여 여러 사용자의 권한을 한 번에 지정할 수 있다. <br>
그룹을 사용하면 대규모 사용자 집합의 권한을 더 쉽게 관리할 수 있다. <br>
예를 들어 IAMPublishers라는 그룹을 만들어 게시 워크로드에 일반적으로 필요한 유형의 권한을 해당 그룹에 부여한다.


#### IAM 역할
IAM 역할은 특정 권한을 가지고 있는 AWS 계정 계정 내 ID이다. <br>
IAM 사용자와 유사하지만, 특정 개인과 연결되지 않는다. <br>
역할을 전환하여 AWS Management Console에서 IAM 역할을 임시로 수임할 수 있다. <br>
AWS CLI 또는 AWS API 태스크를 호출하거나 사용자 지정 URL을 사용하여 역할을 수임할 수 있다. 
---
title:  "지속적 배포"
excerpt: "DevOps 부트캠프 Section 2 "

categories:
  - Blog
tags:
  - [Blog, DevOps]

toc: true
toc_sticky: true
 
date: 2023-04-21
last_modified_at: 2023-04-21
---
# 배포 자동화
배포 자동화가 필요한 이유
- 수동적이고 반복적인 배포 과정을 자동화함으로써 시간이 절약된다.
- 휴먼 에러(Human Error)를 방지할 수 있다.<br>
휴먼 에러란 사람이 수동적으로 배포 과정을 진행하는 중에 생기는 실수들을 뜻한다. <br>
그 전에 했던 배포 과정과 비교하여 특정 과정을 생략하거나 다르게 진행하여 오류가 발생하는 것이 휴먼 에러의 예로 볼 수 있다.

배포 자동화 파이프라인
- Source 단계 : 원격 저장소에 관리되고 있는 소스 코드에 변경 사항이 일어날 경우, 이를 감지하고 다음 단계로 전달하는 작업을 수행한다.
- Build 단계 : Source 단계에서 전달받은 코드를 컴파일, 빌드, 테스트하여 가공한다. <br>
또한 Build 단계를 거쳐 생성된 결과물을 다음 단계로 전달하는 작업을 수행한다.
- Deploy 단계 : Build 단계로부터 전달받은 결과물을 실제 서비스에 반영하는 작업을 수행한다.



## AWS 개발자 도구
AWS 개발자 도구는 AWS에서 제공하는 클라우드 컴퓨팅 플랫폼(AWS)을 이용하는 개발자들을 위한 다양한 도구와 서비스를 제공한다.

AWS 개발자 도구에는 다음과 같은 서비스들이 있다.

- AWS CodeCommit: 프라이빗 Git 리포지토리를 호스팅하는 서비스로, AWS에서 관리되는 VPC에서 프라이빗 네트워크에서 코드를 안전하게 저장할 수 있다.

- AWS CodeBuild: 빌드 및 테스트 프로세스를 자동화하여, 개발자가 더욱 빠르게 애플리케이션을 개발, 테스트 및 배포할 수 있도록 도와주는 서비스이다.

- AWS CodeDeploy: 애플리케이션 배포 프로세스를 자동화하여, 개발자가 손쉽게 애플리케이션을 배포하고 스케일링할 수 있도록 도와주는 서비스이다.

- AWS CodePipeline: CodeCommit, CodeBuild, CodeDeploy 및 다른 AWS 및 외부 서비스를 자동화하여, 전체 소프트웨어 배포 프로세스를 통합하는 서비스이다.

- AWS Cloud9: 클라우드 기반의 IDE(Integrated Development Environment)로, 개발자가 웹 브라우저만으로 AWS 리소스에 액세스하여 코드 작성, 디버깅 및 실행을 수행할 수 있다.

- AWS X-Ray: 분산된 애플리케이션의 성능 문제 해결을 위한 디버깅 및 분석 도구이다.

- AWS Elastic Beanstalk: 개발자가 코드를 업로드하면, 인프라 구성과 배포를 자동으로 처리하는 PaaS(Platform as a Service)이다.

- AWS SAM(Serverless Application Model): AWS Lambda, Amazon API Gateway 및 Amazon DynamoDB 등 AWS 서비스에서 서버리스 애플리케이션을 빠르게 작성, 테스트 및 배포하는 데 사용되는 오픈 소스 프레임워크이다.

이러한 도구들을 통해 개발자들은 빠르고 안정적인 애플리케이션을 쉽게 개발, 테스트 및 배포할 수 있다.


## AWS CodeBuild
AWS CodeBuild는 AWS에서 제공하는 완전 관리형 빌드 서비스이다. <br>
소스 코드에서 실행 가능한 바이너리 파일을 생성하고 테스트, 패키징, 배포하는 데 사용된다.

AWS CodeBuild를 사용하면 소스 코드의 버전 관리 시스템에서 빌드 프로젝트를 가져와서 빌드 환경에서 실행할 수 있다. <br>
이러한 환경은 AWS에서 미리 구성된 도커 이미지를 사용하거나 사용자 정의 빌드 이미지를 사용할 수 있다. <br>
AWS CodeBuild는 프로젝트 빌드와 관련된 모든 자원을 자동으로 관리하므로 더 이상 인프라를 직접 관리할 필요가 없다.

또한 AWS CodeBuild는 여러 AWS 서비스와 통합되어 있다. <br>
예를 들어, AWS CodePipeline에서 AWS CodeBuild를 사용하여 지속적인 통합/배포 파이프라인을 구성할 수 있다. <br>
또한 AWS CodeBuild는 AWS CodeCommit, AWS CodeDeploy, AWS CodeArtifact, Amazon S3 등 다른 AWS 서비스와 함께 사용할 수 있다.

AWS CodeBuild 프리 티어에는 general1.small 또는 arm1.small 인스턴스 유형을 사용한 매월 총 100분의 빌드 시간이 포함된다. <br>
CodeBuild 프리 티어는 12개월의 AWS 프리 티어 기간이 끝나도 자동으로 종료되지 않는다. 

build.general1.small 컴퓨팅 유형의 월별 빌드 시간 100분까지 무료

## AWS CodeDeploy
AWS CodeDeploy는 AWS에서 제공하는 애플리케이션 배포 서비스이다. <br>
CodeDeploy를 사용하면 Amazon EC2 인스턴스, AWS Lambda 함수, 온프레미스 서버, ECS 컨테이너 등 다양한 대상 환경에 코드 변경 사항을 자동으로 배포할 수 있다.

AWS CodeDeploy는 배포 과정을 자동화하여 안전하고 빠르게 애플리케이션 배포를 수행할 수 있도록 지원한다. <br>
배포 단계에서 CodeDeploy는 Amazon EC2 인스턴스 또는 다른 대상 환경에서 구동되는 애플리케이션을 중단시키지 않고 새로운 코드 버전을 배포하고, 이전 버전과의 차이를 자동으로 검증하여 배포 중 오류를 찾아내고 롤백할 수 있도록 지원한다.

또한 CodeDeploy는 여러 배포 전략을 제공하여 다양한 배포 요구사항을 지원한다. <br>
블루/그린 배포, 롤링 배포, 점진적 배포 등 다양한 전략을 선택하여 배포할 수 있다. <br>
또한 AWS CodeDeploy는 AWS CodePipeline, AWS CodeCommit, AWS CodeBuild, Amazon S3 등 다른 AWS 서비스와 함께 사용할 수 있다.

EC2, Lambda, ECS에서 CodeDeploy를 사용하는 경우: AWS CodeDeploy를 통해 Amazon EC2, AWS Lambda 또는 Amazon ECS에 코드를 배포하는 데는 추가 비용이 부과되지 않습니다.

온프레미스에 CodeDeploy를 사용하는 경우: AWS CodeDeploy를 사용해 온프레미스 인스턴스를 업데이트하는 경우에는 업데이트당 0.02 USD의 요금이 부과됩니다. 최소 요금 및 사전 약정은 없습니다. 예를 들어 인스턴스 3개에 대한 배포는 인스턴스 업데이트 3회와 동일합니다. CodeDeploy에서 인스턴스를 업데이트하는 경우에만 요금이 부과됩니다. 배포가 진행되지 않은 인스턴스에 대해서는 요금이 부과되지 않습니다.

애플리케이션을 저장하고 실행하기 위해 CodeDeploy와 함께 사용할 수 있는 다른 AWS 리소스(예: S3 버킷)에 대해서는 비용을 지불해야 합니다. 사용한 만큼만 비용을 지불하고 최소 요금 및 사전 약정은 없습니다.

## AWS CodePipeline
AWS CodePipeline은 소프트웨어 개발 및 배포를 자동화하기 위한 완전 관리형 서비스이다. <br>
AWS CodePipeline은 여러 단계의 소프트웨어 배포 프로세스를 자동화하는 데 사용된다. <br>
이 서비스는 코드의 컴파일, 테스트 및 배포와 같은 모든 단계를 실행하고, 각 단계에서 발생하는 문제를 확인하고 이를 처리할 수 있는 자동화된 방법을 제공한다.

AWS CodePipeline은 연결된 서비스와 함께 사용할 수 있다. <br>
예를 들어, AWS CodeBuild, AWS CodeDeploy, AWS CloudFormation 등이 연결될 수 있다. <br>
AWS CodePipeline은 이러한 서비스를 사용하여 소프트웨어를 배포하고, 배포 단계에서 발생하는 문제를 처리할 수 있다.

AWS CodePipeline은 다음과 같은 장점을 제공한다.

- 간편한 설정: AWS CodePipeline은 단계별로 코드를 실행하며, 이를 처리하는 간단한 설정을 제공한다.
- 다양한 연결 옵션: AWS CodePipeline은 다양한 AWS 서비스와의 연결을 제공한다.
- 보안: AWS CodePipeline은 AWS에서 제공하는 다양한 보안 기능을 제공하며, 안전한 소프트웨어 배포를 지원한다.
- 편리한 모니터링: AWS CodePipeline은 단계별로 실행되는 코드의 실행 상태를 실시간으로 모니터링할 수 있다.

월별 활성 파이프라인 1개까지 무료
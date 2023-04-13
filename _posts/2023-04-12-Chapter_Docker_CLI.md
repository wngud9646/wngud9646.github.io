---
title:  "Docker CLI"
excerpt: "DevOps 부트캠프 Section 2 "

categories:
  - Blog
tags:
  - [Blog, DevOps]

toc: true
toc_sticky: true
 
date: 2023-04-12
last_modified_at: 2023-04-13
---
# Docker Registry, Repository, Tag
## 레지스트리(Registry)
- 도커 이미지를 관리하는 공간
- 도커 허브를 기본 레지스트리로 설정되어 있다.
- 레지스트리는 Docker hub, Private Docker Hub, 회사 내부용 등으로 나뉜다.


## 레포지토리(Repository)
- 레지스트리 내에 도커 이미지가 저장되는 공간
- 이미지 이름이 사용되기도 한다.
- Github의 레포지토리와 비슷한 역할

## 태그(Tag)
- 같은 이미지지만 버전 별로 내용이 다르다
- 해당 이미지를 설명하는 버전 정보를 주로 입력
- latest 태그를 붙인 최신 버전을 사용하는 것이 좋다.


# Docker CLI
## 명령어 옵션
- -d: detached mode, 흔히 데몬이라 부르는 백그라운드 포워딩
- -p: 호스트와 컨테니어의 포트 연결(포워딩)
- -v: 호스트 컨테이너의 디렉토리를 연결(마운트)
- -e: 컨테이너 내에서 사용할 환경변수 설정
- --name: 컨테이너 이름 설정
- -it: -i 와 -t를 합한 것으로 터미널 입력을 위한 옵션 (주로 exec -it로 많이 사용)
- --rm: 프로세스 종료시 컨테이너 자동제거
- ---link: 컨테이너 연결


## CLI reference
- docker --version<br>
이 명령어는 현재 다운로드된 도커의 버전을 알려준다.
```
$ docker --verion
```
- docker pull<br>
이 명령어는 Docker Hub에 있는 repository의 이미지를 다운받을수 있는 명령어다.
```
sudo docker pull ubuntu
```
- docker run<br>
이것은 이미지로 부터 콘테이너를 만드는 명령어이다.
```
sudo docker run -it -d ubuntu
```
- docker ps<br>
이것은 실행중인 컨테이너의 목록을 볼때 사용하는 명령어
```
sudo docker ps
```
- docker ps -a<br>
이것은 실행중과 중지된 컨테이너 까지의 목록을 보여주는 명령어 이다.
```
sudo docker ps -a
```
- docker exec<br>
이것은 실행중인 컨테이너를 사용할수 있는 명령어이다. 컨테이너 CLI를 이용할때 주로 사용함.
```
sudo docker exec -it [컨테이너 이름] bash
```
다시 호스트 CLI로 돌아오고 싶다면 exit를 입력하면 된다.

- docker stop<br>
이것은 실행중인 컨테이너를 멈추는 명령어이다.
```
sudo docker stop [컨테이너 이름]
```
- docker kill<br>
이 명령어는 컨테이너 실행을 즉시 중지시킨다. stop과 kill의 차이점은 정상종료 되는 시간의 차이 이다. <br>
만약에 stop을 했지만 종료되는 시간이 너무 길면 kill을 사용한다.
```
sudo kill [컨테이너 이름]
```

- docker commit<br>
이 명령어는 로컬에 있는 수정된 컨테이너를 새로운 이미지로 만든다.
```
sudo docker commit {컨테이너 ID} {username/imagename}
```

- docker login<br>
이 명령어는 Docker Hub repository에 로그인 할때 사용된다.
```
sudo docker login
```

- docker push<br>
이 명령어는 Docker Hub repository에 이미지를 올릴때 사용한다.
```
sudo docker push {username/imagename}
```

- docker images<br>
이 명령어는 로컬에 저장된 도커 이미지들의 목록을 보여준다.
```
sudo docker images
```

- docker rm<br>
이 명령어는 정지된 컨테이너를 지울 때 사용한다.
```
sudo docker rm [컨테이너 ID]

# 강제로 중지하고 삭제하고 싶으면 다음을 입력하면 된다.
sudo docker rm --force [컨테이너 ID]
```
```
# 명령줄 하나로 실행중 + 중지된 도커 컨테이너들을 지우고 싶으면
sudo docker rm -f $(docker ps -aq)
```

- docker rmi<br>
이 명령어는 로컬에 있는 도커 이미지를 삭제할 때 사용된다.
```
sudo docker rmi [이미지 이름]

# 명령줄 하나로 모든 이미지를 삭제할 수 있는 것
sudo docker rmi $(docker images -q)
```

- docker build
이 명령어는 지정된 도커 파일에서 이미지를 빌드하는 데 사용된다.
```
sudo docker build {psth to Dockerfile}
```

<br><br><br>

# Dockerfile
Dockerfile은 DockerImage를 생성하기 위한 스크립트(설정파일)이다.<br>
여러가지 명령어를 토대로 Dockerfile을 작성한 후 빌드하면
Docker는 Dockerfile에 나열된 명령문을 차례대로 수행하며 DockerImage를 생성해준다.<br>
Dockerfile을 읽을 줄 안다는 것은 해당 이미지가 어떻게 구성되어 있는지 알 수 있다는 의미이다.<br>

## Dockerfile의 장점
(1) 이미지가 어떻게 만들어졌는지를 기록한다.<br>
보통 사람들은 완성된 이미지를 가져다 쓰기 때문에 이미지가 어떻게 만들어졌는지에 대해서는 알 필요가 없다.<br>
그러나 개발자의 경우라면 조금 다르다. 어떠한 애플리케이션을 담고 있는 이미지가 설치 되기 위한 과정은 어떠한지, 중간에 어떠한 과정을 수정해야 하는지 등을 알아야 하는 경우가 있다.<br>
예를 들어 raw한 상태의 우분투 이미지에서 자신이 원하는 애플리케이션을 담은 이미지를 만들어내기까지, 그 과정들을 기록하고 수정하며 바꿔나갈 수 있다는 것이다.<br>
이는 Dockerfile이 자동화된 스크립트 형태이기 때문이다.

(2) 배포에 용이하다.<br>
어떠한 이미지를 배포할 때, 몇 기가씩이나 되는 이미지 파일 자체를 배포하기보다는 그 이미지를 만들 수 있는 스크립트인 Dockerfile만을 배포한다면 매우 편리할 것이다.<br>
사용자는 그 스크립트를 실행시키기만 하면 스스로가 그 Dockerfile에 해당하는 이미지를 얻을 수 있기 때문이다.<br>
실제로 Docker Hub에 가면, Dockerfile로 이미지를 배포하고 있는 사람들을 심심찮게 볼 수 있다. 

(3) 컨테이너(이미지)가 특정 행동을 수행하도록 한다.<br>
컨테이너 환경에서 애플리케이션을 개발하다 보면, 특정 행동을 취하도록 하는 컨테이너를(이미지를) 만들어야 할 때가 있다.<br>
이는 사실 말로서 설명하기는 좀 어렵고, 실제 개발을 하다보면 '아, 이거 Dockerfile 쓰면 좀 간단해 지겠구나...' 라는 생각이 머릿속에서 불현듯 번개처럼 스칠때가 있다.<br>


## Dockerfile 작성 및 명령어
Dockerfile을 작성 할 땐 실제 파일의 이름을 'Dockerfile'로 해야한다.<br>
ubuntu에 아파치 서버를 설치하는 Dockerfile을 작성해보도록 하겠다.<br>
Dockerfile을 담을 디렉토리를 생성 한 후 Dockerfile을 생성한다.<br>
```
  mkdir apache-dockerfile && cd apache-dockerfile
  vi Dockerfile
```
Dockerfile의 내용은 아래와 같습니다.
```
  # server image는 ubunutu 18.04를 사용
  FROM ubuntu:18.04 
  # Dockerfile 작성자
  MAINTAINER Wimes <yms04089@kookmin.ac.kr> 

  # image가 올라갔을 때 수행되는 명령어들
  # -y 옵션을 넣어서 무조건 설치가 가능하도록 한다.
  RUN \
      apt-get update && \
      apt-get install -y apache2

  # apache가 기본적으로 80포트를 사용하기 때문에 expose를 이용해 apache server로 접근이 가능하도록 한다.
  EXPOSE 80 

  # 컨테이너가 생성 된 이후에 내부의 아파치 서버는 항상 실행중인 상태로 만들어준다.
  # apachectl을 foreground(즉, deamon)상태로 돌아가도록 한다.
  CMD ["apachectl", "-D", "FOREGROUND"]
```

- FROM : 베이스 이미지
  - 어느 이미지에서 시작할건지를 의미한다.
- MAINTAINER : 이미지를 생성한 개발자의 정보 (1.13.0 이후 사용 X)
- LABEL : 이미지에 메타데이터를 추가 (key-value 형태)
- RUN : 새로운 레이어에서 명령어를 실행하고, 새로운 이미지를 생성한다.
  - RUN 명령을 실행할 때 마다 레이어가 생성되고 캐시된다.
  - 따라서 RUN 명령을 따로 실행하면 apt-get update는 다시 실행되지 않아서 최신 패키지를 설치할 수 없다.
  - 위 처럼 RUN 명령 하나에 apt-get update와 install을 함께 실행 해주자.
- WORKDIR : 작업 디렉토리를 지정한다. 해당 디렉토리가 없으면 새로 생성한다.
  - 작업 디렉토리를 지정하면 그 이후 명령어는 해당 디렉토리를 기준으로 동작한다.
  - cd 명령어와 동일하다.
- EXPOSE : Dockerfile의 빌드로 생성된 이미지에서 열어줄 포트를 의미한다.
  - 호스트 머신과 컨테이너의 포트 매핑시에 사용된다.
  - 컨테이너 생성 시 -p 옵션의 컨테이너 포트 값으로 EXPOSE 값을 적어야한다.
- USER : 이미지를 어떤 계정에서 실행 하는지 지정
  - 기본적으로 root에서 해준다.
- COPY / ADD : build 명령 중간에 호스트의 파일 또는 폴더를 이미지에 가져오는 것
  - ADD 명령문은 좀 더 파워풀한 COPY 명령문이라고 생각할 수 있다.
  - ADD 명령문은 일반 파일 뿐만 아니라 압축 파일이나 네트워크 상의 파일도 사용할 수 있다.
  - 이렇게 특수한 파일을 다루는 게 아니라면 COPY 명령문을 사용하는 것이 권장된다.
- ENV : 이미지에서 사용할 환경 변수 값을 지정한다.
  - path 등
- CMD / ENTRYPOINT : 컨테이너를 생성 및 실행 할 때 실행할 명령어
  - 보통 컨테이너 내부에서 항상 돌아가야하는 서버를 띄울 때 사용한다.
- CMD
  - 컨테이너를 생성할 때만 실행됩니다. (docker run)
  - 컨테이너 생성 시, 추가적인 명령어에 따라 설정한 명령어를 수정할 수 있습니다.
- ENTRYPOINT
  - 컨테이너를 시작할 때마다 실행됩니다. (docker start)
  - 컨테이너 시작 시, 추가적인 명령어 존재 여부와 상관 없이 무조건 실행됩니다.
- 명령어 형식
  - CMD ["<커맨드>", "<파라미터1>", "<파라미터2>"]
  - CMD <커맨드> <파라미터1> <파라미터2>
  - ENTRYPOINT ["<커맨드>", "<파라미터1>", "<파라미터2>"]
  - ENTRYPOINT <커맨드> <파라미터1> <파라미터2>
참고
https://wooono.tistory.com/679

## 생성한 Dockerfile을 Image로 빌드
이미지 빌드 명령어
```
  docker build -t [이미지 이름:이미지 버전] [Dockerfile의 경로]
```
```
  docker build -t apache-image .
```

생성된 이미지 확인
```
  docker images
```

## Image로 Container를 생성
이제 컨테이너 생성 시 도커 볼륨을 통해 host와 도커 컨테이너의 html 폴더를 동기화 한다.
```
  docker run --name apache-container -d -p 80:80 -v ~/html/:/var/www/html apache-image
```
- --name : 컨테이너 이름
- -d : 백그라운드모드로 실행
- -p : [호스트포트][컨테이너포트] 포트 연결
- -v : 로컬과 컨테이너 파일 연동


## 접속 확인
Public DNS를 80번 포트로 접속해 접속을 확인합니다.


[출처](https://wooono.tistory.com/123) : https://wooono.tistory.com/123

<br><br>

## 회고 및 TIL
docker의 개념들에 대해 직접적으로 배우고, 이를 다루는 명령어들에 대한 내용들에 대해 배웠다.<br>
명령어들이 많기 때문에 기억하는데 시간이 좀 걸릴 것 같다는 생각이 든다.

sprint를 진행하면서 docker의 개념들에 대해 배웠던 부분과 명령어들의 실제 동작과정이나 결과물을 보면서 docker 구조에 대해 좀 더 이해할 수 있었다.

docker는 애플리케이션을 위한 독립된 환경을 만들어줄 수 있다는 것이 큰 장점이라고 생각된다. <br>
사람들마다 여러 가지 설정을 통해 커스터마이징된 작업이라고 할지라도, docker container 안에서 진행되었다면, 진행상황째로 image 파일로 만들어 다른 사람들에게 공유 할 수 있다는 점도 큰 이점이라고 생각되었다. <br>
방금 말한 내용들이 docker가 클라우드 환경에서 필수적인 요소 중 하나로 꼽히는 이유라고 생각된다. <br>
클라우드에서 새로운 인프라를 대여하여 설정할 때, 기존에 사용하던 인프라의 확장이라면 docker image들을 가져다가 container로 구현한다면 기존에 사용하던 인프라와 동일한 환경에서 확장시킬 수 있을 것이다.

오늘 내용들을 잘 기억해두고, 명령어들을 자주 사용해보며 더욱 더 내 자신의 것으로 만들 필요성을 느낀다.
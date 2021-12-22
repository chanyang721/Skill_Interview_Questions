# Docker 란 ?

![스크린샷 2021-12-14 오후 9.25.33.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5d154c75-86db-480e-89ea-689485d502b1/스크린샷_2021-12-14_오후_9.25.33.png)

위의 사진은 Docker가 사용하는 **컨테이너**라는 개념이 없던 시절에 서버 관리자가 다양한 이유로 인해 서버를 이전하는 과정이다.

Docker는 위 사진에 있는 모든 과정을 컨테이너라는 개념을 사용하여 복잡하지만 반복적인 세팅을 재사용할 수 있도록 추상화 시켰다.

또한, Docker는 하나의 프로그램에서 사용하는 다양한 모듈을 컨테이너라는 **격리된 실행 환경**에서 실행하도록 한다. 이것이 혁신이라고 불리는 이유는 다음과 같다.

1. 하나의 컴퓨터에서 두개의 node 버전을 동시에 사용할 수 없었는데, 컨테이너라는 격리된 실행 환경만 있다면 버전 충돌 없이 실행하는 것이 가능해진다.
2. 특정 모듈의 버전이 업데이트되는 경우, 해당 모듈을 사용하는 A프로그램의 다른 모듈과의 버전 호환성 문제를 한번에 해결하는 것이다. B프로그램에서 사용되는 버전과 동일하게 맞추지 않아도 되기 때문에 호환성 문제에서 자유로워진다.
3. 위의 과정을 모두 수동으로 하게 되면 어쩔 수 없이 발생하는 휴먼 에러에서 자유로워진다.

아래는 컨테이너라는 개념이 등장하기까지 다양한 접근 방식으로 실행 환경을 서로 격리시키기 위한 노력들이다. 그리고, 서버를 가장 이상적인 상태로 유지시킬 수 있을까? 라는 문제를 해결하기 위해 컨테이너 개념이 등장하기 까지의 과정이다.

## 애플리케이션 관리의 변화 과정

### 1. 하나의 OS, 여러개의 프로그램

![스크린샷 2021-12-08 오후 5.05.53.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c7247e3d-0f5e-47d8-bfd2-20f7c15ae14b/스크린샷_2021-12-08_오후_5.05.53.png)

애플리케이션을 만들기 위해 하나의 컴퓨터(OS)로 **App, Server, Database, .. 등**을 구성하여 애플리케이션을 배포하는 것이 가능하다. 하지만, 각각의 Server나 Database를 하나의 OS에서 **직접 관리하는 것은 복잡**하고, 다양하지만 반복적인 에러 메시지를 해결하며 **세팅하는데 시간적으로 많은 낭비가 발생**하게 된다.

### 2. 하나의 OS, 여러개의 가상OS

![스크린샷 2021-12-08 오후 5.15.24.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6d24d039-a8a9-49f7-9410-5c855b5fdc09/스크린샷_2021-12-08_오후_5.15.24.png)

따라서, 하나의 OS에 가상OS(Virtualbox)를 생성하는 시도를 했지만, 하나의 서버나 데이터베이스를 실행하기 위해서 운영체제(OS)를 구축하기 위한 모든 파일을 설치하기 때문에 실제 OS자체가 느려진다는 문제가 발생하게 되었다.

### 3. 하나의 OS, 여러개의 컨테이너

![스크린샷 2021-12-08 오후 5.16.09.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6f2bc15b-b5c2-4188-a3e9-ad2a4a078ce0/스크린샷_2021-12-08_오후_5.16.09.png)

이와 같은 문제로 가상 OS를 설치하는 것이 아닌, **애플리케이션(프로그램) 을 실행하는데 필요한 파일(File System)만 가지고 있는 격리된 실행 환경(컨테이너)**에서 각 프로그램을 실행시키는 방법을 사용하게 되었다. 하나의 **OS 실행 환경을 host(주인)**이라고 하며, 각각의 **격리된 실행 환경을 Container(컨테이너)**라고 부르게 된다.

> **When running a container, it uses an isolated filesystem.**

프로젝트를 진행하면서 다양한 서버를 관리하기 위해 프로젝트에서 사용하는 인프라와 개발 환경이 계속 변경되는 경우가 많다. 하나의 애플리케이션에서 서버는 다양한 개발 환경에서 버전 충돌과 같은 문제 없이 작동하도록 해야 했다. 때문에 하나의 애플리케이션의 다양한 개발 환경이 존재하여 각 프로젝트의 개발 환경을 직접 따로 관리해야 하는 복잡한 관리 시스템을 사용하고 있었다.

하지만, 컨테이너라는 개념으로 인해 사용자의 개발 환경, 애플리케이션의 모듈간의 호환성, 사용하는 인프라, 그리고 휴먼 에러에서 자유를 얻는 목표를 달성하게 되었다.

---

---

## Docker 요소

### 💡 Docker Hub [ app store ]

Docker hub는 app store와 같은 개념으로 다른 사람이 필요에 의해 만든 앱(이미지)들을 모아 놓은 데이터베이스이다. Docker Hub(app store)에 있는 이미지(프로그램)이 필요하다면, `docker pull [image name]` 명령어를 통해 이미지(프로그램)를 다운 받는 것이 가능하다.

### 💡 Image(이미지) [ program ]

> Since the image contains the container’s filesystem, **it must contain everything needed to run an application** - all dependencies, configuration, scripts, binaries, etc. The image also contains other configuration for the container, such as environment variables, a default command to run, and other metadata.

docker hub에 있는 image(이미지)를 다운받게 된다면, 해당 이미지를 실행하기 위한 모든 디펜던시, 환경설정, 스크립트 등을 가진 파일을 사용자의 로컬 docker에 이미지로 다운로드 받는다는 의미이다. 그리고 로컬에서 해당 이미지를 실행시키면 비로소 격리된 환경에서 작동하는 컨테이너가 된다.

다운로드 받기 위해서는 터미널에서 `**[docker pull NAME](https://docs.docker.com/engine/reference/commandline/pull/)**` 명령어를 통해 Docker Hub에 있는 이미지를 다운로드 받고, 이 이미지를 실행하기 위해서는 `**[docker run IMAGE](https://docs.docker.com/engine/reference/commandline/run/)`\*\* 명령어를 통해 같은 이미지를 이름이 다른 여러개의 컨테이너로 실행할 수 있다.

### Container(컨테이너) [ process ] 란?

> **a container is a sandboxed process on your machine that is isolated from all other processes on the host machine**

컨테이너란 **Linux 운영체제에서 제공하는 격리된 실행 환경을 제공하는 기술**이다. 따라서, 컨테이너를 사용하기 위해서는 Linux 운영체제를 사용해야하지만, window, macOS를 사용하더라도 리눅스 환경은 Docker를 설치하면 실행되는 순간 아래와 같은 리눅스 환경을 Docker가 자체적으로 만들어준다.

Docker에서는 Docker Hub를 통해 다운받은 이미지를 실행시키면 하나의 컨테이너를 생성하고 그 컨테이너 내부에는 다운 받은 이미지가 실행된다.

![스크린샷 2021-12-09 오전 11.42.44.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f1d10cf2-1084-4d5e-8149-9ef4f66d0880/스크린샷_2021-12-09_오전_11.42.44.png)

### Code Build란 ? [ [**_빌드 프로젝트 만들기(콘솔)_**](https://docs.aws.amazon.com/ko_kr/codebuild/latest/userguide/create-project-console.html#create-project-console-environment) ]

AWS CodeBuild는 전달 받은 소스 코드를 컴파일하여 빌드 및 테스트를 진행하고, 마지막으로 배포를 위한 아티팩트를 생성하는 서비스이다.

Code Pipeline에서 보통 두번째 단계에 속하고, 첫번째 단계인 Code Source로부터 전달 받은 소스 코드의 루트 디렉토리에 있는 **buildspec.yaml** 파일을 읽으면서 해당 파일에 명시된 Shell 명령어들을 설정된 Build 환경에서 실행한다.

![pipeline.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7d9002f5-23cf-4d78-97f1-6f9f7430712a/pipeline.png)

### **Code Build**에서 설정해야 하는 옵션과 데이터의 흐름도는 다음과 같다.

| 설정 옵션                     | AWS 링크 | 관련된 AWS 서비스 블로깅 |
| ----------------------------- | -------- | ------------------------ |
| Source(소스 공급자)           |          | ‣                        |
| Enviroment(실행 환경)         |          | ‣                        |
| Buildspec(빌드 단계 정의)     | AWS Link |                          |
| Batch(서비스 역할 설정)       |          | ‣                        |
| Artifacts(배포 아티팩트 생성) | AWS Link | ‣                        |
| Logs(로깅)                    |          | ‣                        |

![다운로드 (5).png](<https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ca2e1791-0d94-40b4-bba6-f35f2c2cfd3a/다운로드_(5).png>)

---

---

### Code Build 정의

생성하려는 Build를 어떤 서비스를 이용하여 결정하고, 이름을 붙여주는 단계이다.

여기서 지정한 이름을 기반으로 서비스 역할의 이름이 자동으로 생성된다. 생성된 서비스 역할은 AWS IAM로 가서 역할탭에 들어가 검색하면 해당 역할에 정책(접근 권한)을 추가할 수 있다.

![스크린샷 2021-12-14 오후 2.09.09.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/96f47c3c-e3cc-4da5-afcd-2076aecf9192/스크린샷_2021-12-14_오후_2.09.09.png)

![스크린샷 2021-12-14 오후 2.09.30.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4847ca80-35de-4ff4-8fe6-e95faea3a897/스크린샷_2021-12-14_오후_2.09.30.png)

---

---

### Enviroment (빌드 환경) **[ ECR ]**

Code Build의 빌드 환경에서 내가 사용하는 것은 Docker를 이용한 ECR 서비스이기 때문에 관리형 이미지를 선택한다.

그리고 Build이름과 비슷한 서비스 역할이 역할 이름에 자동으로 작성되며, **Batch( 서비스 역할 설정 )**을 참고하여 해당 역할을 IAM의 역할 탭에서 찾아 권한을 추가해야한다.

![스크린샷 2021-12-14 오후 2.09.51.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5df1b8c5-a483-41cb-8c33-dd624cebc5bb/스크린샷_2021-12-14_오후_2.09.51.png)

---

---

### **Batch (서비스 역할 설정) [ IAM ]**

여기서 Code Build가 AWS 서비스를 이용하기 위한 역할(권한)을 주어야 Build시에 AWS의 다른 서비스에 접근 할 수 있다.

여기서 설정하는 Code Build는 빌드 진행 도중에 AWS ECR, S3, CloudWatch에 접근해야 하기 때문에 **역할 이름**칸에 적힌 역할을 AWS IAM 서비스의 역할 탭에서 찾아 두번째 그림과 같이 권한을 추가해야 한다.

세번째 정책(CloudWatch log, S3, CodeBuild)은 자동으로 입력되며, 추가해야 하는 정책은 ECR과 관련된 위에 두개이다.

![스크린샷 2021-12-14 오후 2.14.54.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9c7c5190-7f47-4e9f-840c-cb1ca33a96b1/스크린샷_2021-12-14_오후_2.14.54.png)

![스크린샷 2021-12-14 오후 3.34.48.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4700eaa9-64ac-4dfa-91ca-bdfbb0bb6c03/스크린샷_2021-12-14_오후_3.34.48.png)

---

---

### Buildspec [ [**_Link_**](https://docs.aws.amazon.com/ko_kr/codebuild/latest/userguide/build-spec-ref.html) ]

지금까지 설정한 것은 어떤 소스 코드와 Build 환경에서 Build를 진행할지 결정했다.

Buildspec은 앞서 설정한 빌드 환경에서 소스 코드를 어떤 순서, 명령어로 실행 시킬지를 Shell Script로 작성한다.

Buildspec을 위한 파일명은 **buildspec.yaml** 이어야 하며, 소스 코드의 root 디렉토리에 위치시키면 Code Build에서 실행시킨다.

![스크린샷 2021-12-14 오후 2.11.16.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c037b4ad-2a73-4c44-9af9-1444e7c5917b/스크린샷_2021-12-14_오후_2.11.16.png)

### buildspec.yaml

- **buildspec.yaml 구문 예시**

  ```bash
  version: 0.2

  env:
    shell: shell-tag
    variables:
      key: "value"
      key: "value"
    parameter-store:
      key: "value"
      key: "value"
    exported-variables:
      - variable
      - variable
    secrets-manager:
      key: secret-id:json-key:version-stage:version-id
    git-credential-helper: no | yes

  phases:
    install:
      commands:
  			-
    pre_build:
      commands:
        - echo Logging in to Amazon ECR...
        - ECR Login 명령어
    build:
      commands:
  			- echo Build started on `date`
        - echo Building the Docker image...
  			- docker build -t [image url]
    post_build:
      commands:
        - echo Build completed on `date`
        - echo Pushing the Docker images...
  			- docker push [ ECR push 명령어 ]
  			- echo Writing image definitions file...
  			- printf ~

  artifacts:
    files:
      - location
  cache:
    paths:
      - path
  ```

### Dockerfile

여기서는 buildspec.yaml에서 ECR에 로그인하여 image를 가져온 뒤, Docker를 통해 Build를 하게 된다. 그렇기 때문에 Dockerfile에서 컨테이너에서 실행되는 명령어를 Shell Script로 작성하고, 여기서 테스트를 위한 명령어도 입력하여 테스트도 진행해야한다.

- **Dockerfile** 예시

  ```bash
  FROM node:15.10.0-alpine3.10

  ENV NODE_ENV=production

  WORKDIR /usr/src/app

  COPY [ "package.json", "tsconfig.json" ]

  RUN npm install --only=production

  COPY . .

  EXPOSE 3000

  ENTRYPOINT ["node"] // CLI 명령어 앞에 붙이는 명령어

  CMD ["./dist/app.js"] // 실행시키려는 파일
  ```

---

---

### **Logs(로깅) [ CloudWatch ]**

로깅 설정에서는 CloudWatch log를 설정하게 된다면, CloudWatch 서비스에서 Build가 일어나는 로그를 실시간으로 보는 것이 가능하고, 자동 기록으로 저장된다.

![스크린샷 2021-12-14 오후 2.25.16.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ad821814-61e1-4de0-8d95-1b764cb47222/스크린샷_2021-12-14_오후_2.25.16.png)

---

---

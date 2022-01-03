## ✏️ Nest를 알아보게 된 계기

글을 작성하는 시점에서 TypeScript, NodeJS, Koa, Sequelize, SQLite를 기반으로 한 서버를 만들 기회를 얻었다. 기본적인 서버 구조에서 TypeScript, Koa, SQLite만 새로 적용하면 되는 것이라 어렵지 않게 구현할 수 있다고 생각했다. 하지만, TypeScript와 Sequelize를 동시에 사용하여 모델을 정의한 뒤, 마이그레이션하여 Sequelize로 모델을 가져와 사용하기까지 2일 정도 삽질했다. 도중에 가장 막혔던 부분은 Koa의 특성상 Express와는 다르게 업스트림방식으로 라우팅하여 서버의 폴더 구조를 구성해야하는 것이고, 다른 하나는 JS로 TS파일을 컴파일하여 Sequelize를 적용하는 방식에서 생각보다 많은 시간을 사용하게 되었다.

그러던 중, TS를 지원하는 것과 동시에 서버의 파일 구조 더 직관적으로 설정해주며, ORM또한 쉽게 연결할 수 있는 NestJS라는 프레임워크를 사용하여 서버를 구축해보려고 한다.

## ✏️ Nest 란?

Nest의 공식문서의 소개글에 따르면, Nest는 JS나 TS를 사용하여 NodeJS의 서버를 구축하기 위한 프레임워크로 OOP, FP, FRP의 요소를 결합한다고 한다. 무엇보다도 기존에 NodeJS의 서버를 구축하기 위한 프레임워크인 Express에서 효과적으로 해결하지 못한 Architecture의 주요 문제를 해결하기 위해 만들어졌다.

## ✏️ Express VS Nest

Express는 미들웨어라는 추상적인 이름 아래에 Multer(이미지 파일 관리), passport(인증 기능), margan(응답 후 로그)등의 다양한 미들웨어들이 존재하고, 개발자가 해당 미들웨어의 기능들을 알고 있어야 사용하는 것이 가능했다. 하지만, Nest는 앞서 Express에서 미들웨어라는 이름으로 제공된 다양한 기능들을 각각 분리해서 사용하여 더 직관적으로 사용하는 것이 가능하다.

또한, Nest의 핵심은 Module 시스템이다. Nest는 각 Modules간 하나의 파일로 그룹화 한다. 그리고 하나의 그룹으로 묶인 그룹 내에서 import, export를 하기 때문에 서버의 구조를 알기 쉽고 각 모듈간에 의존성이 명확하다.

기존의 프로그래밍에서 나오는 핵심 개념인 의존성 주입, 인버전 오브 컨트롤, AOP의 개념에 대해 생각해볼 기회를 얻는다.

## ✏️ Nest 시작하기

서버의 모든 파일 구성을 직접 설정해야 하는 Express와는 달리, **Nest 서버는 간단한 명령어로 Nest에서 직접 제공해준다**. 이러한 이유로 NestJS는 기본적으로 서버가 어떤 구성을 가지고 있어야 하는지 알고 있어야 익히는데 편하다.

```jsx
$ npm i -g @nestjs/cli // NestJS를 글로벌에 설치한다.
$ nest new project-name // 새로운 NestJS 프로젝트 서버 구조 파일 생성한다.
```

- `**npm i -g @nestjs/cli`** 앞의 명령어는 NestJS **서버를 위한 가장 모범적인 아키텍쳐 패턴\*\*을 구현해 주는 NestJS 파일을 글로벌(`-g`)에 설치한다.

> **It embodies best-practice architectural patterns to encourage well-structured apps.**

- **`nest <command> [options]`** 의 형식으로 NestJS에서 제공하는 옵션에 접근할 수 있다.
  - `**nest new project-name`** 해당 명령어는 **@nestjs/cli**을 설치하면 사용할 수 있으며, **@nestjs/cli**에 있는 명령어를 보기 위해서는 `**nest —help\*\*`명령어를 입력하면 접근 가능하다.
  - **[ `nest —help`입력 시 나오는 전체 명령어 모음 ]**
    ![스크린샷 2021-10-02 오후 10.24.36.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ef2ebe65-9634-46ca-80ca-b90736a5c94e/스크린샷_2021-10-02_오후_10.24.36.png)

### 표준 파일 구조

`**nest new project-name**` 을 입력하면`**project-name**`을 파일 명으로, 옆의 그림과 같은 구조를 갖는 서버가 **자동으로 생성**된다.

### [ [\*\*Hot Reload](https://docs.nestjs.com/recipes/hot-reload) ]\*\*

또한, 기존의 Express에서 사용했던 nodemon과 같이 코드를 고쳤을때 서버를 다시 켜야 적용되는 것을 자동으로 해주기 위해 **Hot Reload**를 적용해야한다. 링크에 들어가면 두가지 방법이 있지만, 처음 나오는 방법을 순서대로 적용하면 손쉽게 적용하는 것이 가능하다.

![스크린샷 2021-10-02 오후 10.42.17.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/39dd702d-291e-4419-88fa-bd79719e743b/스크린샷_2021-10-02_오후_10.42.17.png)

## ✏️ Nest가 표준 파일 구조를 지정한 이유

Express와 Nest의 서버 구조상 가장 다른 부분이 있다면 라우팅 방식이다.

처음으로 돌아가서 Nest가 **서버를 위한 가장 모범적인 아키텍쳐 패턴**을 선정하여 사용자에게 제공하는 이유를 생각해보았다. **내 생각에는** Nest 개발자들은 **Express의 가장 모범적인 아키텍쳐 패턴을 사용하는 서버 구조를 기반으로 반복되는 로직들을 추상화하여 더 사용하기 쉽고 적용하기 편하게 하기 위해 Nest를 만들었다고 생각한다.**

이를 위해 Express에서 사용했던 서버의 구조와 라우팅 방식을 모듈 단위로 변경하여 각각 Controllers, Service, Repository라는 계층을 각각의 모듈 단위별로 만들어 관리하는 구조가 되었다고 생각한다.

그렇기 때문에 이 아래에서 서술하는 방향은 Express의 기능을 Nest가 어떤 파일 구조을 가지고 각 계층 구조를 어떤 방식으로 추상화하여 사용하고 있는지, 그리고 추상화 이전의 Express의 로직을 Nest로 변경해보면서 로직의 차이점과 어떤 로직이 추상화되고 있는지 확인하면서 진행할 예정이다.

### NestJS의 모듈

![스크린샷 2021-10-10 오전 8.42.27.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f2b162a7-35eb-4584-b71b-d4e12c6b8c16/스크린샷_2021-10-10_오전_8.42.27.png)

### Express의 파일 구조

![스크린샷 2021-10-11 오전 8.56.41.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/db7123cf-4584-41ab-bfed-26a5070aa9b4/스크린샷_2021-10-11_오전_8.56.41.png)

## Express의 표준 파일 구조와 데이터의 흐름

Express에서 클라이언트로부터 특정 Request를 받으면 아래와 같은 흐름에 따라 데이터가 이동하며 Request를 받아 Response를 보내기까지 각각의 계층을 방문하고 해당 계층의 목적에 맞게 로직을 작성하게 된다.

> `Request` → `index.ts` → `routes` → `middlewares` → `controllers` → `services` → `Database` → `services` → `controllers` → `Response`

각 이동 계층의 목적에 따라 어떤 로직을 담당하는지 알아보고, 해당 계층과 로직이 NestJS에서는 어떻게 추상화되어 간편하게 사용할 수 있게 되었는지 알아보자.

### `Request` → `index.ts (app.ts)` → `routes`

### **Configuration**

`npm i --save @nestjs/config`

---

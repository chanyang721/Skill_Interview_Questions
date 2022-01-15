### Express에서의 비동기 에러 핸들링

Express는 ES5 시절에 개발되어 ES6에 등장한 Async Await 명령어에 대한 비동기 처리에서 에러를 잡아내기 위해서는 Try Catch문을 사용해야한다. 

위와 같은 이유로 express로 서버를 구축하다보면 Controller, Service, Repository계층에서 생성되는 수 많은 Try Catch문을 반복적으로 작성하게 되는 현상이 발생하게 되며 코드를 입력하면서 불필요한 코드 작성이 많아지는 결과를 가져온다.

그렇기 때문에 비동기 함수에 Try Catch문을 감싸서 Catch문의 매계변수로 들어오는 error를 내가 원하는 Error 형식으로 변경하기 위해, 그리고 더 깔끔하고 하나의 Class에서 생성된 인스턴스로 Error를 관리하기 위해 이 블로깅을 하게 되었다. 우선 아래에 있는 블로깅은 Try Catch 문으로 비동기 함수를 감싸 반복적인 코드 작성을 피하고, 비즈니스 로직만을 함수 내에 작성할 수 있도록 하게 하려하기 위해 작성했다.

> `[ [Wrapping Try Catch](https://www.notion.so/Wrapping-Try-Catch-28241d7d36274b17b7fe71f08cb28225) ]`
> 

## Error Handling & Logging

Error Handling에서 무엇보다 중요한 것은 Logging이라고 생각되었다. 에러가 발생한 경우 부터 생각해보면, 대부분 Front 단계에서 발생한 에러들이 넘어온다. 그리고 에러 핸들링 하기 위해서 필요 했던 정보는 API, http Method, 그리고 Error Message가 있다. 현재 서비스에서는 AWS의 Cloud Watch로 모든 Log를 저장하고 있지만, 특정 에러 Log를 찾기 위해서 찾아봐야 하는 불편함은 남아있다.

이와 더불어 해당 에러의 형식을 일정하게 하기 위해 하나의 클래스를 만들어 상속 class로 관리하는 것이 아닌, 에러가 나는 환경에서 각각 `throw error`를 하고 있었다. 하지만, 400에러에 대한 형식을 전체적으로 변경해야 했는데, 해당 형식을 고치기 위해 하나씩 찾으며 수정해야 한다는 불편함을 겪게 되었다.

위와 같은 이유로, Error의 형식을 분류(Classification)하여 하나의 클래스를 만들어 통합적으로 관리하기 위해BaseError라는 클래스를 생성하여 `node`에서 제공하는 `Error`를 상속 받아 아래와 같은 BaseError를 생성하였다.

```jsx
export class BaseError extends Error {
  protected statusCode: number;
  protected isOperational: boolean;

  constructor(
    name: string,
    statusCode: StatusCode,
    message: string,
    isOperational?: boolean
  ) {
    super();
    this.name = name || "Server Error";
    this.statusCode = statusCode || 500;
    this.message = message || "No Error Messages";
    this.isOperational = isOperational || true;
    Error.captureStackTrace(this);
    Object.setPrototypeOf(this, new.target.prototype);
  }
}
```

또한, 해당 클래스에서 어느정도 범위가 정해지는 name, StatusCode에 대한 설정을 미리 해준다면, 에러마다 다르게 적용해야 하는 message만 인수로 넣은 BaseError의 상속 클래스의 인수로 넣어 인스턴스를 생성한다면 서비스 단계에서 생성되는 Error의 형식을 변경하는 사항이나, 혹은 재사용 가능하다는 장점을 얻을 수 있기 때문에 생성을 하게 되었다. 아래는 HTTP에서 제공하며, 자주 사용하는 StatusCode를 아래와 같이 정의하여 BaseError에 필요한 BaseError의 StatusCode의 타입, 그리고 값으로 사용하였다.

```jsx
export const enum StatusCode {
  OK = 200,
  Created = 201,
  Accepted = 202,
  No_Content = 204,
  Reset_Content = 205,

  Bad_Request = 400,
  Unauthorized = 401,
  Payment_Required = 402,
  Forbidden = 403,
  Not_Found = 404,
  Method_Not_Allowed = 405,
  Not_Acceptable = 406,
  Conflict = 409,

  Internal_Server = 500,
}
```

### Error Classification

---

### Reference

[Best Practices for Node.js Error-handling](https://www.toptal.com/nodejs/node-js-error-handling)

[Node.js Error Handling Best Practices: Hands-on Experience Tips - Sematext](https://sematext.com/blog/node-js-error-handling/)

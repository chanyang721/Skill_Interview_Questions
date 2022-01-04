### 필요성 인식

Express는 ES5 버전이던 시절에 개발되어 async await을 통한 비동기 환경에서 발생하는 에러를 잡아내지 못한다. 그렇기 때문에 try catch 문을 통해 비동기 함수에서 발생하는 에러들을 잡아내는 방법밖에 없다.

하지만, express를 사용한다면 수 많은 함수 내에서 try catch 문법을 를 반복적으로 사용해야하기 때문에 이를 해소하기 위해 비동기 함수를 매계 변수로 넣어 반복을 최소화 하기 위해 알아보았다.

그렇게 해서 찾은 해결 방법은 구글링을 통해 손쉽게 얻을 수 있었다.

```jsx
const wrapTryCatch = (action)
	=> (req, res, next)
	=> action(req, res).catch(next);
```

하지만, 해당 함수에서는 `catch(next)` 를 통해 에러를 잡아내는 express의 특징을 이용했기 떄문에 에러의 형식을 원하는 형식으로 커스터마에징 할 수 없었고, 에러의 형식 또한 HTML로 리턴되게 되어있다. Error handling을 할 때, 내가 원하는 Error 형식으로 만들기 위해서 위의 형식을 나름대로 변경해서 아래와 같이 만들었다.

```jsx
export const wrapTryCatch = function (controller) {
  return async (req: Request, res: Response, next: NextFunction) => {
    try {
      await controller(req, res, next);
    } catch (error) {
      console.error(error);
      const { name, statusCode, message, isOperational, stack } = error;
      res.status(400).send({ name, statusCode, message, isOperational });
    }
  };
};
```

여기서 내가 알지 못했던 점이 있었는데, `catch(error)` 에서 매계변수로 들어오는 `error`를 나는 단순하게 비즈니스 로직 상 발생한 Express의 기본 Error형식이 에러가 발생하면 자동으로 Catch되어 나와서 어떻게 원하는 Error 형식으로 변경해야 할지 고민하였다. 하지만, 내가 비동기 함수 내에서 throw명령어로 던지는 Error를 전달받는 것이었다. 이를 이용하여 Error의 Base Class생성한다면, 반복되는 Error 코드들을 하나의 Class에서 컨트롤 할 수 있다는 생각을 하게 되어 해당 모듈을 만들어 보고자 한다.

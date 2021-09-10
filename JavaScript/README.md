### var, let, const의 차이점 (중요) <br>
* var, let, const의 차이점은 재선언, 재할당, 스코프 단위에서 찾을 수 있다고 생각합니다.
* var는 재선언, 재할당이 가능하지만, 함수 단위의 스코프를 가지고 있으며, 글로벌 단위에서 선언 시 윈도우 객체에 포함되어 변수의 충돌이나 예측하기 힘든 오류가 발생하기 때문에 최근에는 사용하지 않고 있다고 알고 있습니다. 
* let은 재선언은 불가능하지만, 재할당은 가능하며, 블록 스코프를 가지고 있습니다. 
* const는 재선언, 재할당이 불가능한 선언문으로 블로 스코프를 가지고 있습니다.
  -> let은 변수의 재할당이 가능하다는 장점을 가지고 있지만, 이는 같은 변수 이름을 다르게 사용할 경우 에러의 가능성이 있기 때문에 const를 사용하는 것이 에러 발생을 줄일 수 있다고 생각합니다. 

### 호이스팅 (Hoisting)이 무엇인지 설명해주세요 (중요) <br>
* Hoist는 "끌어올리기" 라는 뜻으로, 자바스크립트 코드에서 작성된 순서와 상관없이 가장 먼저 선언되는 것을 호이스팅이라고 합니다. 
* 이러한 특징은 자바스크립트 엔진 구동시 선언을 가장 최우선으로 해석하기 때문에 발생합니다.
* 예를 들면 변수 선언문 중 var 키워드로 선언된 모든 변수와 함수 선언식은 자동적으로 호이스팅의 대상이 됩니다. 하지만, var로 선언된 변수의 할당된 값은 호이스팅의 대상이 아니기 때문에 변수에는 undefined 할당되어 호이스팅 됩니다.
* 변수에 할당은 런타임 과정에서 이루어 지기 때문에 호이스팅의 대상이 아닙니다.

### 클로저 (Closure) (중요) <br>
* 클로저란 두 개의 함수가 선언된 어휘적 환경의 조합으로, 이 어휘적 환경은 함수가 선언된 시점에서 접근 가능한 모든 지역 변수로 구성됩니다. 
* 따라서 클로저 함수란, 외부 함수의 변수에 접근할 수 있는 내부 함수를 말하며, 내부 함수에서는 지역 변수, 외부 함수의 변수, 전역 변수를 모두 사용할 수 있는 어휘적 환경을 가지게 됩니다.
* 자바사크립트에서 클로저의 역할은 변수와 메서드의 공개/비공개 여부를 결정하기 위한 방법입니다.
* 클로저를 생성하기 위한 조건으로는
1. 내부 함수가 익명 함수로서 외부함수의 return 값으로 사용되는 경우
```js
function outer() {
  let name = `clouser`;
  return function () {
    console.log(name)
  }
}
```
2. 내부 함수가외부 함수의 실행 환경에서 실행되는 경우
```js
function outer() {
  let name = `closure`;
  function inner() {
    console.log(name);
  }
  inner(); 
}
outer();
// console> closure
```
3. 내부 함수의 환경에서 사용되는 변수가 외부 함수의 변수 스코프를 가지는 경우
```js
let name = `Warning`;
function outer() {
  let name = `closure`;
  return function inner() {
    console.log(name);
  };
}

let callFunc = outer();
callFunc();
// console> closure
```
### this에 대해 아는 대로 설명해주세요. 함수를 선언하고 내부에 let a,b 선언하고 this.a를 호출할때의 상황에서 설명해주세요.) (중요) <br>
* 자바스크립트의 함수는 객체 타입으로 선언될 때마다 함수 내부에 암묵적으로 this라는 객체가 추가됩니다.
```js
/// 암묵적인 규칙 ///
function User(name) {
// this = {}; ⇒ 빈 객체가 만들어진다.
  this.name = name;
// return this ⇒ this가 반환된다.  
}

const user = new User("John")
user // ⇒ {name: "John"}
```
* 그렇기 때문에 this는 함수가 호출되는 상황에 따라 this의 값이 변하게 됩니다.
1. 객체(A)의 메서드(B) 호출 시, this는 A.B에서 A가 this가 됩니다. 
```js
const Obj = {
  name: "chanyang",
  sayName: function () {
    console.log(this)
  }
}
```
2. 함수 호출 시, A.B의 구조가 있으면 this는 A에 바인딩되지만, A.B의 구조가 없다면 전역 객체에 바인딩 됩니다.
```js
var value = 1000;
const Obj = {
  value: 10,
  func1: function () {
    console.log(`func1's this.value: ${this.value}`);

    let func2 = function () {
      console.log(`func2's this.value: ${this.value}`)
    }
    func2();
  }
}
Obj.func1();
// func1's this.value: 10
// func2's this.value: 1000
```
3. new 키워드로 생성자 함수를 호출하여 인스턴스 생성 시, this는 생성된 인스턴스가 됩니다.
```js
let Person = function (name) {
  console.log(this);
  this.name = name;
}

let student = new Person("chanyang");
console.log(student.name) // "chanyang"
```

### 원시값, 참조값 차이 및 개념 (중요) <br>

### 기존 자바스크립트와 ES6의 차이점 및 특징 (중요) <br>
* 기존의 자바스크립트와 ES6의 차이점은 ES6부터 추가된 화살표함수, 라스트파라미터(...), 디폴트파라미터, for/of문, 템플릿 리터럴(`${}`)와 같은 다른 언어에서 제공되는 다양한 기능을 사용할 수 있다는 것입니다. 
* Class라는 syntactic sugar 문법이 추가되어 프로토타입으로 인스턴스를 생성해야 했던 불편함을 해소했습니다.
* 그리고 ES6문법은 웹 브라우저에서 컴파일하지 못하기 때문에 바벨을 사용하여 ES5문법으로 트랜프파일링 후 컴파일 해야하는 특징이 있습니다.

### Promise 관련 개념 → 무엇인지, 왜 사용하게 되었는지, 어떤 특징이 있는지 등등 (중요) <br>

### Node.js -> 싱글스레드 개념 (중요) 

### Get,Post 메소드 차이점 (중요)

### 브라우저 작동 원리 (중요)

### React 관련 질문→ 개념, 장점, 단점, useState, React Hooks, 컴포넌트, JSX 등등 (중요)

### 위의 질문에서 연결 지어서 상태관리 질문 (중요) → Redux → Redux 기본개념

### 서버 사이드 랜더링 개념 (중요)

### HTTP 와 HTTPS 차이점 (중요)

### 트렌젝션 개념 및 단계에 대한 설명 (중요) 

### CI/CD 구현 여부 (중요)  

### 배포 진행 여부 (중요)



### 왜 개발자란 직업을 선택하게 되었는지 (중요)

### 프로젝트 진행할 때 발생했던 갈등 및 해결 방법 (중요)

### 개발하는 것에 대한 어려움 (중요)

### 앞으로 어떤 개발자가 되고싶은지 (중요)



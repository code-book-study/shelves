# chapter1 디자인 패턴과 프로그래밍 패러다임

## 디자인 패턴

### 싱글톤 패턴

- 하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴
- 장점
  - 사용하기 쉬우며 실용적이다.
- 단점
  - TDD하기 힘들다<br/>
    싱글톤 패턴은 미리 생성된 하나의 인스턴스를 기반으로 구현하는 패턴이므로 각 테스트마다 독립적인 인스턴스를 만들기가 어렵다.
  - 모듈간의 결합을 강하게 한다<br/>
    의존성(종속성) 주입을 통해 조금 느슨하게 만들어 해결할 수 있다.<br/>
    디커플링이(메인 모듈과 하위 모듈 중간에 "의존성 주입자"를 두어 메인 모듈이 간접적으로 의존성을 주입하는 방법)

#### JavaScript의 싱글톤 패턴

- 리터럴 {} 또는 new Object로 객체 생성 -> 자체만으로 싱글톤 패턴 구현
- 예제

  ```javascript
  // Singletone.instance라는 하나의 인스턴스를 가지는 Singletone 클래스
  class Singletone {
    constructor() {
      if(!Singletone.instance) {
        Singletone.instance = this
      }
      return Singletone.instance
    }
    getInstance() {
      return this
    }
  }

  // Singtone 클래스를 통해 a와 b는 하나의 인스턴스를 가진다.
  const a = new Singletone();
  const b = new Singletone();
  console.log(a === b) // true
  ```

  ```javascript
  // 데이터베이스 연결 모듈
  const URL = 'mongodb://localhost:0000/roseapp'
  const createConnection = url => ({'url' : url});

  class DB {
    constructor(url) {
      if(!DB.instance) {
        DB.instance = createConnection(url)
      }
      return DB.instance
    }
    connect() {
      return this.instance
    }
  }

  const a = new DB(URL);
  const b = new DB(URL);
  console.log(a === b) // true
  ```

### 팩토리 패턴

- 객체 생성 부분을 추상화한 패턴
- 상속 관계의 두 클래스에서, 상위 클래스는 중요 뼈대 결정, 하위 클래스는 객체 생성에 관한 구체적인 내용 결정하는 패턴
- 장점
  - 느슨한 결합, 유연성
  - 객체 생성 로직이 분리되어 있기 때문에, 리팩터링 및 유지보수성 증가

#### JavaScript의 팩토리 패턴

- new Object()로 구현 가능
- 예제

  ```javascript
  const num = new Object(42);
  const str = new Object('abc');
  num.constructor.name // Number
  srt.constructor.name // String
  ```

  ```javascript
  // 상위 클래스 : 중요 뼈대 결정
  class CoffeeFactory {
    static createCoffee(type) {
      const factory = factoryList[type];
      return factory.createCoffee();
    }
  }
  class Latte {
    constructor {
      this.name = "Latte";
    }
  }
  class Expresso {
    constructor {
      this.name = "Expresso";
    }
  }
  // 하위 클래스 : 구체적인 내용 결정
  class LatteFactory extends CoffeeFactory {
    static createCoffee() {
      return new Latte();
    }
  }
  class EspressoFactory extends CoffeeFactory {
    static createCoffee() {
      return new Espresso();
    }
  }

  const factoryList = { LatteFactory, EspressoFactory };

  const main = () => {
    // 라떼 커피를 주문한다.
    const coffee = CoffeeFactory.createCoffee("LatteFactory");
    // 커피 이름을 부른다.
    console.log(coffee.name) // Latte
  }
  main()
  ```

### 전략 패턴 (정책 패턴)

- 객체의 행위를 바꾸고 싶은 경우 전략이라 부르는 '캡슐화한 알고리즘'을 컨텍스트 안에서 바꿔주면서 상호 교체가 가능하게 만드는 패턴
- 관련 라이브러리

  - passport : Node.js에서 인증 모듈을 구현할 때 쓰는 미들웨어 라이브러리

    ```javascript
      const passport = require('passport');
      const LocalStrategy = require('passport-local').Strategy;

      passport.use(//passport.use 메서드에
        new LocalStrategy(// '전략'을 메개변수로 넣어서 로직 수행
        function(username, password, done) {
          User.findOne({ username: username }, function (err, user) {
            if (err) { return done(err); }
            if (!user) {
              return done(null, false, { message: 'Incorrect username' });
            }
            if (!user.validPassword(password)) {
              return done(null, false, { message: 'Incorrect password'})
            }

            return done(null, user);
          })
        }
      ));
    ```

### 옵저버 패턴

- 상태 변화를 관찰중인 객체의 상태 변화가 있을 때마다 메서드 등을 통해 옵저버들에게 변화를 알려주는 패턴
- 주체 : 객체의 상태 변화를 보고 있는 관찰자
- 옵저버들 : 객체의 상태 변화에 따라 전달되는 메서드 등을 기반으로 '추가 변화 사항'이 생기는 객체들
- 활용
  - 트위터 (주제, 팔로우 개념)
  - MVC (Model-View-Controller) 패턴 : 주체라고 볼 수 있는 Model에서 변경 사항이 생겨 update() 메서드로 옵저버인 View에 알려주고, 이를 기반으로 Controller 작동

#### JavaScript의 옵저버 패턴

- 프록시 객체를 통해서 구현가능
- 예시
  ```javascript
    fucntion ctreatReactiveObject(target, callback) {
      const proxy = new Proxy(target, {
        get(){} // 속성과 함수에 대한 접근 가로챔
        has(){} // in 연산자의 사용을 가로챔
        set(obj, prop, value) { // 속성에 대한 접근 가로챔
          if (value !== obj[prop]) {
            const prev = obj[prop];
            obj[prop] = value;
            callback(`${prop}가 [${prev}] >> [${value}] 로 변경되었습니다.`);
          }
          return true
        }
      })
      return proxy
    }
    const a = {
      "형규" : "솔로"
    }
    const b = creatReactiveObject(a, console.log)
    b.형규 = "솔로"
    b.형규 = "커플"
    // 형규가 [솔로] >> [커플]로 변경되었습니다.
  ```

### 프록시 패턴과 프록시 서버

#### 프록시 패턴

- 대상 객체에 접근하기 전 그 접근에 대한 흐름을 가로채 해당 접근을 필터링하거나 수정하는 등의 역할을 하는 계층이 있는 디자인 패턴
- 개체 속성, 변환 등을 보완
- 보안, 데이터 검증, 캐싱, 로깅에 사용

#### 프록시 서버

- 서버와 클라이언트 사이에서 클라이언트가 자신을 통해 다른 네트워크 서비스에 간접적으로 접속할 수 있게 해주는 컴퓨터 시스템이나 응용 프로그램
- 주요 사용되는 프록시 서버 : nginx, CloudFlare

> **관련 용어**
>
> **프록시 서버에서의 캐싱**<br/>
> 캐시 안에 정보를 담아두고, 캐시 안에 있는 정보를 요구하는 요청에 대해 캐시 안에 있는 데이터를 활용하는 것이다. 불필요한 외부와의 연결을 하지않기 때문에 트래픽을 줄일수 있다.
>
> **버퍼 오버플로우**<br/>
> 메모리 공간을 벗어나는 경우를 말한다. 이때 사용되지 않아야 할 영역에 데이터가 덮어씌워져 주소, 값을 바꾸는 공격이 발생하기도 한다.
>
> **gzip 압축**<br/>
> LZ77과 Huffman 코딩의 조합인 DEFLATE 알고리즘을 기반으로 한 압축 기술이다. 데이터 전송량을 줄일 수 있지만, 압축을 해제했을 때 서버에서의 CPU 오버헤드도 생각해서 사용해야한다.
>
> **DDOS 공격**<br/>
> 짧은 기간 동안 네트워크에 많은 요청을 보내 네트워크를 마비시켜 웹 사이트의 가용성을 방해하는 사이버 공격 유형.
>
> **CDN (Content Delivery Network)**<br/>
> 각 사용자가 인터넷에 접속하는 곳과 가까운 곳에서 콘텐츠를 캐싱 또는 배포하는 서버 네트워크. 사용자가 웹 서버로부터 콘텐츠를 다운로드하는 시간을 줄일 수 있다.

##### CORS와 프론트엔드의 프록시 서버

CORS (Cross-Origin Resource Sharing)는 서버가 웹 브라우저에서 리소스를 로드할 때 다른 오리진을 통해 로드하지 못하게 하는 HTTP 헤더 기반 메커니즘

### iterator 패턴

- iterator를 사용하여 컬렉션의 요소들에 접근하는 디자인 패턴
- 순회할 수 있는 여러 가지 자료형의 구조와는 상관없이 iterator라는 하나의 인터체이스로 순회가 가능

#### JavaScript의 iterator 패턴

```javascript
  // 다른 자료 구조인 map, set도
  const  mp = new Map();
  mp.set('a', 1);
  mp.set('b', 2);
  mp.set('c', 3);

  const st = new Set();
  st.add(1);
  st.add(2);
  st.add(3);

  // 같은 for (let a of b)라는 iterator 프로토콜로 순회할 수 있다.
  for (let a of mp) console.log(a);
  for (let a of st) console.log(a);

  /*
    ['a', 1]
    ['b', 2]
    ['c', 3]
    1
    2
    3
  */
```

> **관련 용어**
>
> **iterator 프로토콜**<br/>
> iteratorble 객체들을 순회할 때 쓰이는 규칙
>
> **iteratorble 객체**<br/>
> 반복 가능한 객체로 배열을 일반화한 객체

### 노출모듈 패턴

- 즉시 실행 함수를 통해 private, public 같은 접근 제어자를 만드는 패턴

#### JavaScript의 노출모듈 패턴

- 대표적 노출모듈 패턴 기반 모듈 방식 : CJS (CommonJS)
- 예시
  ```javascript
    const pukuba = (() => {
      // a, b는 private 범위를 가짐
      const a = 1;
      const b = () => 2;
      // c, d는 public 범위를 가짐
      const public = {
        c : 2,
        d : () => 3
      }
      return public
    })();
    console.log(pukuba) // { c: 2, d: [Function: d] }
    console.log(pukuba.a) // undefined
  ```

### MVC 패턴

- 애플리케이션의 구성 요소를 세 가지 역할로 구분하여 개발 프로세스에서 각각의 구성 요소에만 집중해서 개발할 수 있다.
- 장점 : 재사용성과 확장성이 용이
- 단점 : 애플리케이션이 복잡해질수록 모델과 뷰의 관계가 복잡해짐
- 예 : Java Spring

> **관련 용어**
>
> **Model**<br/>
> 애플리케이션의 데이터인 데이터베이스, 상수, 변수 등을 뜻함
>
> **View**<br/>
> 사용자 인터페이스 요소. 화면에 표시하는 정보만 가지고 있음
>
> **Controller**<br/>
> 이벤트 등의 메인 로직을 담당. 모델과 뷰를 잇는 다리역할을 하며 생명주기도 관리.

### MVP 패턴

- MVC의 C에 해당하는 컨트롤러가 Presenter로 교체된 패턴
- 뷰와 프레젠터는 일대일 관계 : MVC 패턴보다 더 강한 결합을 지닌 패턴

### MVVM 패턴

- MVC의 C에 해당하는 컨트롤러가 View Model로 교체된 패턴
- View Model : 뷰를 더 추상화한 계층
- 특징
  - 커맨드와 데이터 바인딩을 가짐
  - 뷰와 뷰모델 사이의 양방향 데이터 바인딩을 지원함
  - UI를 별도 코드 수정 없이 재사용할 수 있고 단위 테스팅이 쉬움
- 예 : Vue.js

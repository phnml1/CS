# 디자인 패턴
## 디자인 패턴 이란?
> 디자인 패턴이란 프로그램을 설계할 때 발생했던 문제점들을 객체 간의 상호 관계 등을 이용하여 해결할 수 있도록 하나의 "규약" 형태로 만들어 놓은 것을 의미한다.

개발의 **효율성**, **유지보수성**, **운용성이** 높아지며 **프로그램의 최적화**에 도움이 된다.

## 싱글톤 패턴
`하나의 클래스`에 오직 `하나의 인스턴스`만 가지는 패턴이다.

- 하나의 인스턴스를 만들어놓고 해당 인스턴스를 다른 모듈들이 공유하여 사용하기 때문에 `인스턴스를 생성할 때 비용이 줄어든다.`
-  의존성이 높아져, 각 테스트 마다 독립적인 인스턴스를 만들기 힘들어, TDD를 할 때 걸림돌이 된다는 단점이 있다.
```js
class Singleton {
	constructor() {
		if(!Singleton.instance) {
			Singleton.instance = this;
		}
		return Singleton.instance;
	}
	getInstance() {
		return this.instance
	}
}
const a = new Singleton();
const b = new Singleton();
console.log(a===b) //true
```
Singleton 클래스는 Singleton.instance라는 `하나의 인스턴스`를 가지고, 이를 통해, a와 b는 하나의 인스턴스만을 가진다.

이외에도 Mysql, mongodb와 같은 데이터베이스 연결 모듈에서도 이 패턴이 쓰인다.
```js
const URL = 'mongodb://localhost:27017/kundolapp' 
const createConnection = url => ({"url" : url})    
class DB {
    constructor(url) {
        if (!DB.instance) { 
            DB.instance = createConnection(url)
        }
        return DB.instance
    }
    connect() {
        return this.instance
    }
}
const a = new DB(URL)
const b = new DB(URL) 
console.log(a === b) // true
```
위의 코드에서는 DB.instance라는 하나의 인스턴스를 기반으로 a,b를 생성하였다. 이를 통해 데이터베이스 연결에 관한 인스턴스 생성 비용을 아낄 수 있다.


### 의존성 주입
**모듈간의 결합을 강하게 만들 수 있다라는 싱글톤 패턴의 단점을 의존성 주입을 통해 모듈간 결합을 좀 더 느슨하게 만들어 해결할 수 있다.**
```
의존성 = 종속성
(A가 B에 의존성이 있다는 것은 B에 변경사항에 대해 A또한 변해야 한다는 것) 
```
메인 모듈이 직접 하위 모듈에 의존성을 주기보다는 `의존성 주입자`가 이 부분을 중간에 가르쳐 메인 모듈이 **`간접`적으로 의존성을 주입**한다.
- 메인 모듈은 하위 모듈에 대한 **의존성이 떨어지게 된다.(**'디커플링이 된다')
- 모듈을 쉽게 교체할 수 있는 구조가 되어 **테스팅과 마이그레이션이 수월**
- 모듈간의 관계가 명확해지지지만, 모듈들이 더욱 분리 되므로 클래스 수가 늘어나 **복잡성이 증가**하거나 런타임 패널티가 생길 수도 있다. 

**의존성 주입 원칙**
상위 모듈은 하위 모듈에서 어떠한 것도 가져오면 안되며, 둘다 추상화에 의존해야 하며, 추상화는 세부 사항에 의존하면 안된다.
# 이터레이터와 노출 모듈 패턴
## 이터레이터 패턴
이터레이터 패턴은 `이터레이터를 사용`하여 `컬렉션의 요소들에 접근`하는 디자인패턴으로,  

이를 통해 순회할 수 있는 여러 가지 자료형의 구조와는 상관없이 `이터레이터`라는 하나의 인터페이스로 순회가 가능합니다.

```js
const mp = new Map() 
mp.set('a', 1)
mp.set('b', 2)
mp.set('cccc', 3) 
const st = new Set() 
st.add(1)
st.add(2)
st.add(3) 
const a = []
for(let i = 0; i < 10; i++)a.push(i)

for(let aa of a) console.log(aa)
for(let a of mp) console.log(a)
for(let a of st) console.log(a) 
/* 
a, b, c 
[ 'a', 1 ]
[ 'b', 2 ]
[ 'c', 3 ]
1
2
3
*/
```
```
- 이터레이터 프로토콜
이터러블한 객체를 순회할 때 쓰이는 규칙

- 이터러블한 객체
- 반복가능한 객체로 배열을 일반화한 객체
```

## 노출모듈 패턴
`즉시 실행함수`를 통해 private, public과 같은 접근 제어자를 만드는 패턴이다. 자바스크립트는 private, public과 같은 접근 제어자가 존재하지 않고 전역 범위에서 스크립트가 실행된다. 그렇기 때문에 노출모듈 패턴을 통해 private과 public 접근 제어자를 구현하기도 한다.
```js
const pukuba = (() => {
    const a = 1
    const b = () => 2
    const public = {
        c : 2, 
        d : () => 3
    }
    return public 
})() 
console.log(pukuba)
console.log(pukuba.a)
// { c: 2, d: [Function: d] }
// undefined
```
a와 b는 다른 모듈에서 사용할 수 없는 변수나 함수이며 `private` 범위를 가진다. c와 d는 다른 모듈에서 사용할 수 있는 변수나 함수이며 `public`범위를 가진다. 

노출 모듈패턴을 기반으로 만든 자바스크립트 모듈 방식으로는 CJS(CommonJS) 모듈 방식이 있습니다.


- public
클래스에 정의된 함수에서 접근 가능하며 자식 클래스와 외부 클래스에서 접근 가능한 범위

- protected
클래스에 정의된 함수에서 접근 가능, 자식 클래스에서 접근 가능하지만 외부 클래스에서 접근 불가능한 범위

- private
클래스에 정의된 함수에서 접근 가능하지만 자식 클래스와 외부 클래스에서 접근 불가능한 범위

- 즉시 실행 함수
함수를 정의하자마자 바로 호출하는 함수. 초기화 코드, 라이브러리 내 전역 변수의 충돌 방지 등에 사용한다.

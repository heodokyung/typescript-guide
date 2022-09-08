# Typescript-Guide
타입스트립트에 대하여 정리한 내용입니다.


### 타입스크립트를 사용해야 하는 이유
#### ⭐ Javascript
- 자바스크립트는 매우 유연한 언어라 에러를 사전에 보여주지 않기 때문에 코드에서 오류가 발생할 가능성이 높아집니다.

```javascript
// 예시) 🚫 숫자 배열 + false
[1,2,3,4] + false → '1,2,3,4false' // 배열이 사라지고 string으로 바뀜
```

```javascript
// 🚫 함수의 인자가 잘못 들어가도 실행됨
function add(a, b) {
  return a + b
}
add(1) → NaN //return값이 NaN일 뿐, 에러가 나지 않음
```

```javascript
const a = { a: "A" };
a.hello();
// 선언 되지 않은 함수를 호출할 때: 실행 시 에러 발생(실행 전에 에러 감지 불가)
```

#### ⭐ Tyescript
- 타입 안정성과 사전에 에러를 발생시켜 오류(버그)감소
- TypeScript는 JavaScript에 추가적인 구문을 추가하여 editor와의 단단한 통합을 지원합니다. editor에서 초기에 오류를 잡을 수 있습니다.
- TypeScript 코드는 JavaScript가 실행되는 모든 곳(브라우저, Node.js 또는 Deno 및 앱 등)에서 JavaScript로 변환될 수 있습니다.
- TypeScript는 JavaScript를 이해하고 타입 추론(type inference)을 사용하여 추가 코드 없이도 훌륭한 도구를 제공합니다.

### 타입스크립트 코드 테스트
<a href="https://www.typescriptlang.org/play" target="_blank">https://www.typescriptlang.org/play</a>

### 타입스크립트 핸드북
<a href="https://typescript-kr.github.io/pages/basic-types.html" target="_blank">https://typescript-kr.github.io/pages/basic-types.html</a>


### 타입스크립트 Type 정의
- ✅ 배열: 자료형[]
- ✅ 숫자: number
- ✅ 문자열: string
- ✅ 논리: boolean
- ✅ optional

#### 명시적 정의(변수 선언 시 타입 정의)
```javascript
let a: boolean = "x"
// 🚫 boolean 타입에 string타입 할당 불가 알림
```

#### 변수만 생성(타입 추론)
```javascript
let b = "hello"; // b가 string 타입이라고 추론
b = 1 // string으로 선언한 변수에 number값 할당
// 🚫 string 타입에 number타입 할당 불가 알림
```

#### Alias(별칭) 타입, optional로 선언
<a href="https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-aliases" target="_blank">type-aliases</a>

→ 타입값에 ?로 값을 주면 optional
```javascript
❗ ?를 :앞에 붙이면 optional

// ✅ Alias(별칭) 타입
type Player = {
    name: string,
    age?:number
}

const player : Player = {
    name: "nico"
}

⭐ 함수에서는 어떻게 쓸까
type Player = {
    name: string,
    age?:number
}
```

```javascript
// ❌ player.age가 undefined일 가능성 알림
if(player.age < 10) {
}

// ⭕ player.age가 undefined일 가능성 체크
if(player.age && player.age < 10) {
}
```

#### 함수에서 사용시
```javascript
function playerMaker1(name:string) : Player {
    return {
        name
    }
}

const playerMaker2 = (name:string) : Player => ({name})

const nico = playerMaker1("nico")
nico.age = 12
```

#### readonly
JavaScript에서는 mutability(변경 가능성)이 기본값이지만 타입스크립트에서는 readonly를 통해 읽기 전용으로 만들 수 있습니다.
```javascript
interface Pizza {
  readonly x: number;
}

let pizza: Pizza = { x: 1 };
pizza.x = 12; // error


const numbers: readonly number[] = [1, 2, 3, 4]
🚫 numbers.push(1)

// ❗ readonly가 있으면 최초 선언 후 수정 불가 ⇒ immutability(불변성) 부여
// but, javascript에서는 그냥 배열
```
<a href="https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes-func.html#readonly-and-const" target="_blank">readonly-and-const</a>

#### Tuple
정해진 개수와 순서에 따라 배열 선언
```javascript
const player: [string, number, boolean] = ["nico", 1, true]
// ❗ readonly도 사용가능 ⇒ readonly [...] 형태
```

#### undefined, null, any
- any: 아무 타입
- undefined: 선언X 할당X
- null: 선언O 할당X

#### unknown
unknown타입은 모든 값을 나타냅니다. 이것은 any타입과 비슷하지만 any보다 unknown이 더 안전합니다. 이유는 unknown값으로 작업을 수행하는 것은 합법적이지 않기 때문입니다.
```javascript
// 예시1
let a:unknown

if(typeof a === 'number'){
  let b = a + 1
}
if(typeof a === 'string'){
  let b = a.toUpperCase()
}
🚫 let b = a + 1

// 예시2
function hello(a: any) {
  a.b(); // OK
}

function hello2(a: unknown) {
  a.b(); // 에러: Object is of type 'unknown'.
}
```

#### void
void는 값을 반환하지 않는 함수의 반환 값을 나타냅니다. 함수에 return 문이 없거나 해당 return 문에서 명시적 값을 반환하지 않을 때 항상 유추되는 타입입니다.
```javascript
function hello() {
  console.log('x')
}

function noop() {
  return;
}
```

#### never
일부 함수는 값을 반환하지 않습니다. 이는 함수가 예외를 throw하거나 프로그램 실행을 종료함을 의미합니다.
```javascript
function fail(msg: string): never {
  throw new Error(msg);
}

function temp(name:string|number):never {
  if(typeof name === "string"){
    name
  } else if(typeof name === "number"){
    name
  } else {
    name
  }
}
```
- if 안에서는 string형의 name 반환합니다.
- else if 안에서는 number형의 name 반환합니다.
- else 안에서는 never형의 name 반환합니다.
- ⇒ 즉, 제대로 인자가 전달되었다면 else로 올 수 없습니다.



### Function
#### Call Signatures
프로퍼티로 호출 가능한 것을 설명하려면 객체 타입에 Call Signature을 작성할 수 있습니다. Call(=Function) Signature란 함수의 매개변수와 반환 값의 타입을 모두 type으로 미리 선언하는 것을 말하며 Call Signatures는 다음과 같이 함수의 매개 변수(parameter)와 반환 타입을 지정합니다.
```javascript
type Add = {
  (a: number, b: number): number;
}

const add: Add = (a, b) => a + b
```

##### 주의
```javascript
const add:Add = (a,b) => a+b
// 위의 Arrow함수를 일반 함수로 풀면 다음과 같게 됩니다.
function add(a, b) {
  return (a+b)
}

const add:Add = (a,b) => {a+b}
// 위의 Arrow함수를 일반 함수로 풀면 다음과 같게 됩니다.
function add(a, b) {
  a+b;
}

// 애로우함수에서 {}를 사용하게 되면 그 안의 값은 반환이 아니라 함수 내부 내용으로 처리되기에 반환값이 없는 void로 처리됩니다.
// 이에 따라 위에서 미리 선안한 Add자료형의 반환값은 number라고정해놓은 내용과 충돌하기에 에러가 발생합니다.
```

```javascript
type PizzaFunction = {
  pizza: string;
  (args: number): boolean;
};

function hello(fn: PizzaFunction) {
  console.log(fn.pizza, fn(6));
}
```
<a href="https://www.typescriptlang.org/docs/handbook/2/functions.html#call-signatures" target="_blank">call-signatures</a>


#### Overloads
하나의 함수가 복수의 Call Signature를 가질 때 발생합니다.  동일한 이름에 매개 변수와 매개 변수 타입 또는 리턴 타입이 다른 여러 버전의 함수를 만드는 것을 말합니다. TypeScript에서는 오버로드 signatures을 작성하여 "다양한 방식으로 호출할 수 있는 함수"를 지정할 수 있습니다.
```javascript
type Add={
  (a:number,b:number):number;
  (a:number,b:number,c:number):number;
}

const add:Add=(a, b, c?:number) => {
  if (c) return a + b + c;
  return a + b;
}

add(1,2)
add(1,2,3)
```
<a href="https://www.typescriptlang.org/docs/handbook/2/functions.html#function-overloads" target="_blank">overloads</a>

#### polymorphism(다형성)
다형성이란, 여러 타입을 받아들임으로써 여러 형태를 가지는 것을 의미합니다. 인자들과 반환값에 대하여 형태(타입)에 따라 그에 상응하는 형태(타입)를 갖을 수 있습니다.

타입스크립트에서 다형성을 가지기 위해선 제너릭을 사용합니다.
제네릭은 C#이나 Java와 같은 언어에서 재사용 가능한 컴포넌트를 만들기 위해 사용하는 기법으로 단일 타입이 아닌 다양한 타입에서 작동할 수 있는 컴포넌트를 생성할 수 있습니다.(구체적인 타입을 지정하지 않고 다양한 인수와 리턴 값에 대한 타입을 처리할 수 있습니다.) 타입스크립트에서 제네릭을 통해 인터페이스, 함수 등의 재사용성을 높일 수 있습니다.
제네릭은 선언 시점이 아니라 생성 시점에 타입을 명시하여 하나의 타입만이 아닌 다양한 타입을 사용할 수 있도록 하는 기법이라 정리할 수 있습니다.



##### 재너릭을 사용하지 않았을 때
```javascript
// 모든 타입을 다 적어주어야 함
type SuperPrint = {
  (arr: number[]): void,
  (arr: string[]): void,
  (arr: boolean[]): void,
  (arr: (number|string|boolean)[]): void
}

const superPrint: SuperPrint = (arr) => {
  arr.forEach(e => console.log(e));
}

superPrint([1, 2, 3])
superPrint(["a", "b", "c"])
superPrint([true, false, true])
superPrint([1, "b", true])
```
위와 같이 다양한 경우를 커버하는 함수를 작성할 때, 모든 조합의 Call Signature를 concrete type으로 적어주는 일은 번거롭습니다.

##### 재너릭을 사용할 때
```javascript
type SuperPrint = {
  (arr: T[]): T
}

const superPrint: SuperPrint = (arr) => arr[0]

// (arr: number[]) => number
superPrint([1, 2, 3])

// (arr: string[]) => string
superPrint(["a", "b", "c"])

// (arr: boolean[]) => boolean
superPrint([true, false, true])

// (arr: (string | number | boolean)[]) => string | number | boolean
superPrint([1, "b", true])
```
위의 코드처럼 type에 Generic을 할당하면 호출된 값으로 concrete type을 가지는 Call Signature를 역으로 보여주는 다형성(Polymorphism)을 가지게 됩니다

##### 함수에서 제너릭을 사용할 때
```javascript
function identity< Type >(arg: Type): Type {
  return arg;
}

// 제네릭 화살표 함수 (tsx기준)
const identity=< Type extends {} >(arg: Type):Type => {
  return arg;
}

let output = identity< string >("myString"); // 첫 번째 방법
let output = identity("myString"); // 두 번째 방법
// 두 번째 방법은 type argument inference(타입 인수 유추)를 사용합니다. 즉, 컴파일러가 전달하는 인수 유형에 따라 자동으로 Type 값을 설정하기를 원합니다.
```

##### any를 넣는 것과 Generic의 차이는 무엇일까요?
```javascript
// Any
type SuperPrint = {
  (arr: any[]): any
}

const superPrint: SuperPrint = (arr) => arr[0]

let a = superPrint([1, "b", true]);

// pass
a.toUpperCase(); // any를 사용하면 위와 같은 경우에도 에러가 발생하지 않습니다.

// Generics
type SuperPrint = {
  (arr: T[]): T
}

const superPrint: SuperPrint = (arr) => arr[0]

let a = superPrint([1, "b", true]);

// error
a.toUpperCase();
// Generic의 경우 에러가 발생해 보호받을 수 있다
// 이유는 Call Signature를 concrete type으로 하나씩 추가하는 형태이기 때문입니다.

type SuperPrint = {
  (arr: T[], x: M): T
}

const superPrint: SuperPrint = (arr, x) => arr[0]

let a = superPrint([1, "b", true], "hi");

// 위와 같이 복수의 Generic을 선언해 사용할 수도 있습니다
```
'any'를 사용하는 것은 어떤 타입이든 받을 수 있다는 점에서 'generics'과 같지만 함수를 반환하는데 있어 'any'는 받았던 인수들의 타입을 활용하지 못합니다.
정리하면 any와의 차이점은 해당 타입에 대한 정보를 잃지 않는다는 차이점으로 any는 any로서 밖에 알 수 없지만 generics는 타입 정보를 알 수 있습니다.

<a href="https://www.typescriptlang.org/docs/handbook/2/generics.html" target="_blank">Generics</a>


### classes And Interfaces
#### classes
##### 추상(abstract) 클래스
추상 클래스는 오직 다른 클래스가 상속받을 수 있는 클래스로 직접 새로운 인스턴스를 만들 수는 없습니다.

```javascript
abstract class User {
  constructor(
    private firstname:string,
    private lastname:string,
    public nickname:string
  ){
    abstract getNickname():void
  }
}

class Player extends User {
// 추상 메서드는 추상 클래스를 상속받는 클래스들이 반드시 구현(implement)해야하는 메서드입니다.
  getNickname(){
    console.log(this.nickname)
  }
}
```
##### 접근 가능한 위치 정리
| 구분　     | 선언한 클래스 내 | 상속받은 클래스 내 | 인스턴스 |
| :--------- | :--------------: | :-----:            | :-----:  |
| private    |    ⭕            |  ❌                |  ❌   |
| protected  |    ⭕            |  ⭕                 |  ❌   |
| public     |    ⭕            |  ⭕                 |  ⭕    |

public: 모든 클래스에서 접근 가능
private: 해당 클래스 내에서만 접근 가능 (자식 클래스에서도 접근 불가)
protected: 해당 클래스와 자식 클래스에서 접근 가능

##### 예시: 사전 구현하기
```javascript
type Words = {
  [key: string]: string;
};

class Dict {
  private words: Words;
  constructor() {
    this.words = {};
  }

  add(word: Word) {
    if (this.words[word.term] === undefined) {
      this.words[word.term] = word.def;
    }
  }

  def(term: string) {
    return this.words[term];
  }

  update(word: Word) {
    if (this.words[word.term] !== undefined) {
      this.words[word.term] = word.def;
    }
  }

  del(term: string) {
    if (this.words[term] !== undefined) {
      delete this.words[term];
    }
  }
}

class Word {
  constructor(public term: string, public def: string) {}
}

const kimchi = new Word("kimchi", "super cool food");
const pizza = new Word("pizza", "super nice piazza");
const dict = new Dict();

dict.add(kimchi);
dict.add(pizza);
console.log("KIMCHI:", dict.def("kimchi"));
console.log("PIZZA:", dict.def("pizza"));

dict.update(new Word("kimchi", "very incredible super food"));
console.log("UPDATE KIMCHI:", dict.def("kimchi"));
console.log("NOT UPDATE PIZZA:", dict.def("pizza"));

dict.del("pizza");
console.log("DELETE PIZZA", dict.def("pizza"));
console.log("NOT DELETE KIMCHI:", dict.def("kimchi"));
```
<a href="https://www.typescriptlang.org/docs/handbook/2/classes.html" target="_blank">classes</a>


#### Static Members
클래스에는 static 멤버가 있을 수 있습니다. 이 멤버는 클래스의 특정 인스턴스와 연결되지 않습니다. 클래스 생성자 객체 자체를 통해 액세스할 수 있습니다. static 멤버는 동일한 public, protected 및 private 과 함께 사용할 수도 있습니다. static Methods는 클래스를 생성안해도 사용이 가능합니다.

```javascript
class MyClass {
  static x = 0;
  static printX() {
    console.log(MyClass.x);
  }
}
console.log(MyClass.x);
MyClass.printX();
```
<a href="https://www.typescriptlang.org/docs/handbook/2/classes.html#static-members" target="_blank">static-members</a>

#### Interfaces
객체의 모양을 특정해주기 위해 사용합니다. 여기서는 firstName 및 lastName 필드가 있는 객체를 설명하는 인터페이스를 사용합니다.

```javascript
interface Person {
  firstName: string;
  lastName: string;
}
```
<a href="https://www.typescriptlang.org/docs/handbook/typescript-tooling-in-5-minutes.html#interfaces" target="_blank">interfaces</a>

#### implements
implements을 사용하여 클래스가 특정 인터페이스를 충족하는지 확인할 수 있습니다. 클래스를 올바르게 구현하지 못하면 오류가 발생합니다. implements절은 클래스가 인터페이스 유형으로 처리될 수 있는지 확인하는 것입니다. 클래스의 유형이나 메서드는 전혀 변경하지 않습니다. 또한 클래스는 여러 인터페이스를 구현할 수도 있습니다. 

```javascript
// 클래스 C는 A, B를 구현합니다.
class C implements A, B { }
```

```javascript
interface Pingable {
  ping(): void;
}

// Sonar클래스는 Pingable인터페이스를 implement했기 때문에 Pingable가 가진 ping메서드를 구현해줘야 합니다.
class Sonar implements Pingable {
  ping() {
    console.log("ping!");
  }
}
```
<a href="https://www.typescriptlang.org/docs/handbook/2/classes.html#implements-clauses" target="_blank">implements</a>


#### Type Aliases과 Interfaces의 차이점
Type Aliases과 인터페이스는 매우 유사하며 많은 경우 자유롭게 선택할 수 있습니다. 인터페이스의 거의 모든 기능은 type에서 사용할 수 있으며, 주요 차이점은 type을 다시 열어 새 속성을 추가할 수 없는 것입니다. 반면 인터페이스는 항상 확장 가능합니다.

<a href="https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#differences-between-type-aliases-and-interfaces" target="_blank">differences-between-type-aliases-and-interfaces</a>

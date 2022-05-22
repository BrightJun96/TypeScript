# 내가 정리하는 TypeScript 기본 문법

## 📲 Configuration

Typescript를 실행하기에 앞서 환경설정할 부분은 다음과 같다.

- typescript의 file extension은 .ts이다.
  `index.ts`
  ![](https://images.velog.io/images/kcj_dev96/post/177c6813-c7ef-4b6c-8c20-8e11a42f8a71/t1.png)

- Typescript option 설정은 tsconfig.json에서 설정한다
  ![](https://images.velog.io/images/kcj_dev96/post/6ad11255-5857-4e93-a793-9d7fb14f61de/t2.png)

- 프로젝트 폴더에 tsc --init 명령어를 사용하면 tsconfig.json에 모든 컴파일러 옵션이 있으며 대부분 주석 처리가 되있다.
  (필요한 부분은 주석을 해제해서 쓰면 됨.)

- watch mode를 실행하려면 명령어로 tsc 입력

### options

- "noEmitOnError": true => 에러가 났을 때 컴파일 해주지 않게 하는 옵션

- "watch": true => ts에서 변경한 사항들을 즉각적으로 js로 컴파일 해주는 옵션

- "target":"ECMA version" => 컴파일될 ECMA Script Version을 지정해주는 옵션

- "outDir" : "folder" => 컴파일 될 파일들을 지정할 폴더

## ⭐️ Typescript 특성

- 변수 및 반환값의 타입을 지정할 수 있다.

- TypeScript는 명시적으로 타입을 지정하지 않아도 타입을 유추하여 타입을 지정한다.

따라서 유추가능한 데이터 타입이라면 명시적으로 타입을 지정하지 않아도 된다.

- 함수 매개변수와 반환값의 타입 지정

```js
function taxcount(state: string, income: number, dependent: number): number {
  if (state === "seoul") return income * 0.06 - dependent * 500;
  if (state === "busan") return income * 0.05 - dependent * 500;
}
```

매개변수의 타입 지정 위치 : 매개변수 뒤에 :type

반환값의 타입 지정 위치 : 매개변수 넣는자리와 코드블럭 사이에 반환값의 타입을 지정해준다.

- TypeScript에서 초깃값없이 변수를 선언하면 any Type으로 유추한다.

## 📚 Type

### void

값을 반환하지 않는 함수를 선언하는데 사용된다.

함수에 void 타입을 선언하면 실행은 되지만 반환하지는 않는다.

_!_
자바스크립트 함수는 런타임 중에 함수 본문에 return문이 없는 경우 함수를 실행했을 때 undefined 반환한다.

하지만 void타입을 사용하면 이와 같은 실수를 방지할 수 있다.

### never

절대 반환을 하지 않는 함수에 사용한다.

절대로 실행이 종료되지 않는 함수나 오류를 발생시키기 위해서만 존재하는 함수를 예를 들 수 있다.

절대로 실행이 종료되지 않는 함수 ex) state값으로 인한 무한 리렌더링

never type으로 선언한 함수는 end point가 있는 즉, 끝날 수 있는 함수는 에러가 난다.

- 화살표 함수는 never type을 반환한다.

- never는 어떤 타입과도 호환되지 않는 타입으로 논리적으로 끝까지 실행될 수 없는 함수의 반환값이 never이 된다.

```js
const logger = () => {
while(true){
	console.log('서버가 실행 중 입니다.')
}
}

function padLeft(value:string,padding:string|number) : string{
if(typeof padding === "number") {
  returnArray(padding+1).join("") + value
}
  if(typeof padding === "string") {
   return padding + value
  }
}

else return padding // padding값은 나올 상황이 없다. 즉, else 블록은 실행되지 않는다.



```

![](https://images.velog.io/images/kcj_dev96/post/74a64268-7ce8-4528-a1cb-838a774c2e81/3.png)

### any

number,string,boolean,custom type을 할당할 수 있다.
그러나 타입 체크의 장점을 잃고 코드의 가독성도 떨어질 수 있어 되도록 사용하지 않는 것이 좋다.

**커스텀 타입**
사용자가 정의한 타입

### unknown

any와 비슷하나 먼저 타입을 지정하거나 좁히지 않으면 조작이 허용되지 않는다.

### custom type

type,interface,enum으로 custom type을 만들 수 있다.

**type**
type 키워드는 새로운 타입을 선언하거나 타입 별칭을 사용해 이미 존재하는 타입에 다른 이름을 붙여 사용할 수 있다.

```js
type Foot = number // Foot타입은 숫자
type Pound = number // Pound타입은 숫자

type Patient = {
	name : string;
  	height : Foot;
  	weight : Pound
}

let patient : Patient {
	name : "Jev",
    height : 184,
    weight : 78
}


```

## type alias & interface

type과 interface는 둘 다 **타입을 지정**해줄 수 있는 기능이다.
**type**은 **여러 데이터 타입을 지정**해줄 수 있지만 **interface**는 **객체**에 대해서만 타입을 지정해줄 수 있다.

**type**
type은 식별자에 대하여 객체뿐만 아니라 **여러 데이터 타입을 지정**해줄 수 있다.

```ts
type person = {
  name: string;
  height: number;
};

type weight = number;

type sum = (a: number, b: number) => number;

type fun1 = typeof sum;

type obj = {
  // 메서드명을 입력해주지 않아도 됨.
  (a: number, b: number): number;
};

const sumFun1: sum = (a, b) => a + b;
// type으로 정의한 객체 메서드를 함수 타입으로 사용할 수 있음.
const sumFun2: obj = (a, b) => a + b;
```

위처럼 type은 **객체를 포함하여 원시 타입,함수 등 다양한 타입을 유동적으로 사용**할 수 있다.

**interface**
**객체 타입을 표현**하는데에 사용된다.

```ts
interface Person {
height : number;
name : string;
birth : number;
  ...
}

interface obj {
  // 메서드명을 적어주지 않아도 됨.
  (a: number, b: number): number;
}

// 객체 메서드 타입을 타입으로 사용할 수도 있음.
const f1:obj = (a + b) => a + b

```

**특징**
**Intersection**
type과 interface로 선언한 다수의 객체 타입은 하나의 타입으로 합칠 수 있다.

두개의 타입을 합치기 위해 **&연산자**를 사용하면 된다.

다만 다수의 타입을 합친 식별자는 type으로 지정해줘야한다.

**type + type => type **

```ts
// 가능
type Name = {
  name: “string”
};

type Age = {
  age: number
};

type Person = Name & Age;
```

**interface + interface => type**

```ts
// 가능
interface Name {
  name: “string”
};

interface Age {
  age: number
};

type Person = Name & Age;
```

**interface + type => type**

```ts
// 가능
interface Name {
  name: “string”
};

type Age = {
  age: number
};

type Person = Name & Age;

```

**interface + interface => interface >> 에러**

```ts
// 불가능
interface Name {
  name: “string”
};

interface Age {
  age: number
};

interface Person = Name & Age; // 이거는 안됨.
```

(type으로 선언한 타입과 interface로 선언한 타입도 합칠 수 있나?)

**차이점**
type과 interface는 비슷하게 사용할 수 있지만 다음과 같은 차이점이 있다.

1. type은 여러 데이터 타입을 지정할 수 있지만 interface는 객체의 데이터 타입만을 지정할 수 있다.

2. Declartion Merging
   type은 이미 선언한 식별자명에 중복 선언이 안될뿐만 아니라 중복선언된 다수의 식별자명의 타입은 합쳐지지않는다.(can not merge)
   interface는 중복선언이 되고 중복된 다수의 식별자명끼리 서로 다른 프로퍼티를 합칠 수 있다. (can merge)

3. extends

```ts
interface Song {
  artistName: string;
}

interface Song {
  songName: string;
}

const song: Song = {
  artistName: "Freddie",
  songName: "The Chain",
};
```

위와 같이 Song이라는 interface가 2번 중복선언된다해도 서로의 프로퍼티를 합칠 수 있게 된다.

타입스크립트는 중복선언된 interface를 자동적으로 하나로 합쳐줄 수 있는 기능이 내장되어있기 때문이다.

하지만 type으로 중복선언하려하면 에러가 발생한다.

- https://blog.logrocket.com/types-vs-interfaces-in-typescript/#:~:text=Types%20vs.-,interfaces,%2C%20for%20example%2C%20an%20object.

## Generic

함수에서 쓰이는 경우

인터페이스에서 쓰이는 경우

## [typeof](https://www.typescriptlang.org/docs/handbook/2/typeof-types.html)

어떠한 변수나 프로퍼티의 타입을 참조할 수 있는 연산자

```ts
let s = "hello";
let n: typeof s;
```

typeof 연산자를 사용하면 다른 변수의 타입을 사용할 수 있다.

## [ReturnType](https://www.typescriptlang.org/docs/handbook/utility-types.html#returntypetype)

ReturnType은 **함수의 리턴값에 대한 타입**을 사용할 수 있는 연산자이다.

```ts
function f1(): { a: number; b: string };

type T0 = ReturnType<() => string>; // T0의 타입 : string

type T1 = ReturnType<(s: string) => void>; // T1의 타입 : void

type T2 = ReturnType<typeof f1>; // T2의 타입 : f1함수 반환값의 타입{a:number,b:string}

type T7 = ReturnType<string>; //Type 'string' does not satisfy the constraint '(...args: any) => any'.
```

위와 같이 ReturnType의 **꺽쇠괄호 안에는 함수**가 들어가야하며 **함수의 반환값의 타입**을 돌려준다.

**이미 선언한 함수의 타입을 사용**하기 위해서는 **typeof 연산자**를 사용하면 된다.

주의할 점은 ReturnType의 **꺽쇠괄호 안에는 문자열,숫자 등 함수가 아닌 데이터타입을 사용할 수 없다.**

#### declare

## 함수 타입 지정하는 법

**1. 함수 선언문 타입 지정하는법**

```ts
function f1(a: number, b: number): number {
  return a + b;
}
```

이 때 주의할 것은 **반환값이 void나 any가 아니라면 return(반환값)을 명시**해줘야한다.
**Error**

```ts
function f1(
  a: number,
  b: number
): number {} /*A function whose declared type is 
neither 'void' nor 'any' must return a value.*/
```

**반환값이 void**

```ts
function f1(a: number, b: number): void {}
```

**2. 화살표 함수 타입 지정하는 법**

```ts
// 함수표현식 선언과 동시에 타입할당
const f1 = (a: number, b: number): number => a + b;
const f2 = (a: number, b: number): void => {};

// 따로 지정해둔 타입을 함수표현식에 할당
type sum = (a: number, b: number) => number;
const f3: sum = (x, y) => x + y;
```

- https://ui.toast.com/weekly-pick/ko_20210521

## [keyof](https://www.typescriptlang.org/docs/handbook/2/keyof-types.html)

## as (type assertion)

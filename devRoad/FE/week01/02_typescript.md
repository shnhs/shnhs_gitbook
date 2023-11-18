# 타입스크립트 기초

[**TypeScript 공식문서**](https://www.typescriptlang.org/ko/)  
한국어를 제대로 지원하지 않지만, 몇몇 문서를 한국어로 읽을 수 있긴 하다.

- [TypeScript for the New Programmer](https://www.typescriptlang.org/ko/docs/handbook/typescript-from-scratch.html)
- [TypeScript for JavaScript Programmers](https://www.typescriptlang.org/ko/docs/handbook/typescript-in-5-minutes.html)

쉽게 REPL을 사용하고 싶다면 ts-node를 이용할 수 있다.
> REPL? REPL은 Read-Eval-Print-Loop의 약자.  
> 빠르고 간편하게 소스코드 실행 결과를 확인하고 싶을 때 사용한다.

```bash
npx ts-node
```

## 타입 지정

변수나 함수를 선언할 때 타입을 명시해준다.

```typescript
let name: string;
let age: number;

// 가능
name = '홍길동';
age = '29'; 

// 에러
name = 13;
age = '홍길동동';

function add(x: number, y: number): number {
  return x + y;
}

add(1, 2) // 3
```

## 타입 정의

오브젝트 타입을 정의하여 재사용 할 수 있다.  

`type` 대신 `interface`를 사용해도 동일한 기능을 할 수 있다.  
차이점이 있다면 타입은 새로운 필드를 추가할 수 없으나 인터페이스는 확장할 수 있다.  

```typescript
type Human = {
  name: string;
  age: number;
};

let boy: Human;

// 정상
boy = { name: '홍길동', age: 29};

// 에러
errorBoy = {name: '김에러'}; // 나이를 명시하지 않음
```

정해진 범주를 가지는 타입을 지정할 수 있다. Union 등에서 유용하게 사용할 수 있다.

```typescript
let category: 'food';

category = 'food'; // food 만 지정가능
```

배열은 뒤에 괄호를 붙여준다.

```typescript
let numberArray = number[];

numberArray = [1,2,3];
```

튜플을 사용하여 배열안에 들어갈 타입을 제한할 수 있다.

```typescript
let pair = [string, number];
pair = ['hp', 123];
```

## 타입추론

타입을 명시하는 부분을 생략하더라도 기본적으로는 내부적으로 할당한 값을 보고 타입을 추론한다.  

```typescript
const name = '홍길동'; // 내부적으로 name: string 으로 타입추론

name = '최길동' // 가능
name = 123 // 타입이 달라서 에러
```

## Union Type

변수가 여러개의 타입을 가질 수 있는 경우 사용할 수 있다.  
백엔드에서 해당 필드가 문자열로 넘어오는 경우에 매핑하기 위해 사용할 수 있다.

```typescript
let age: number | string;

age = 10;
age = '15';
```

혹은 문자열 매개변수를 제한할 때 유용하게 사용할 수 있다.

```typescript
// Category 타입의 변수에는 top, bottom, outer 라는 문자열만 할당가능
type Category = 'top' | 'bottom' | 'outer';

let category: Category;
category = 'top';
```

## Optional 파라미터 + 기본값 처리

`?` 기호를 사용하여 옵셔널 파라미터 처리를 할 수 있다.

```typescript
// name은 기본적으로 string 이지만 undefined도 받을 순 있음
function greeting(name?: string):string{
    return `Hello, ${name||'world'}`;
}

greeting('hong'); // Hello, hong
greeting(); // Hello, world
```

아예 기본값을 잡아줄수도 있다.

```typescript
// name 파라미터가 할당되지 않을 경우 world가 기본값
function greeting(name:string = 'world'):string{
    return `Hello, ${name}`;
}
```

## Intersection Type

두 타입을 집합시켜 타입을 확장 할 수 있다.

```typescript
type Human = {
  name: string;
  age: number;
};

type Creature = {
  hp: number;
  mp: number;
};

// Person은 Human과 Creature에 선언된 모든 필드가 있어야함
type Person = Human & Creature;

let person: Person;

person = { name: '홍길동', age: 13, hp: 256, mp: 16 };
```

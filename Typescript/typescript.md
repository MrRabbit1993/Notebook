## 基础数据类型

bolean、number、string、数组[]、元组 Tuple[]、枚举 Enum、any、void、null、undefined、nerver、object

## 变量申明

尽量使用 let 以及 const 进行变量申明，var 可以重复申明，以最后一次申明为准，并且不存在块级作用域，会导致变量提升等一些奇异的现象。

let 可以在不影响原来的变量的情况下定义同名的局部变量进行覆盖。

注意，解构出来的对象建议使用 const，const 不允许直接修改变量的值。

ps: 可以在申明的时候，同时申明变量的类型，防止类型的随意赋值

```typescript
let str: string;
str = "123456";
str = 1; // 不能将类型“number”分配给类型“string”。
```

## 解构

```typescript
let obj = { x: 1, y: 2 };
const { x, y } = obj;
console.log(x, y); // 输出1 2

let arr = [1, 2];
const [a, b] = arr;
console.log(a, b); // 输出1 2

// 对象展开
let moreArr = [1, 2, 3, 4];
const [a, ...b] = moreArr;
console.log(a, b); // 1, [2, 3, 4];

let moreObj = { x: 1, y: 2, z: 3 };
const copyObj = { ...moreObj };
console.log(copyObj); // {x: 1, y: 2, z: 3}
```

对于对象的解构来说，相当于第一层的深拷贝，类似于通过循环手动的去创建新的 key value，但是第二层的数据依然是浅拷贝的，所以使用解构来进行拷贝数据需要注意。

### 属性重命名

```typescript
let obj = { x: 1, y: 2 };
const { x: X, y: Y } = obj;
console.log(X, Y); //   1 2
console.log(x, y); // Uncaught ReferenceError: x is not defined
```

### 默认值

```typescript
let obj = { x: 1, y: 2 };
const { x = 2, y = 2, z = 2 } = obj;
console.log(x, y, z); //  1 2 2
```

## 接口

```typescript
interface Test {
  a: string;
  b: number;
}

const obj: Test = { a: "string", b: 1 };
```

_数据类型指定为接口时，注意属性不能多或者少，我们可以指定可选属性，或者只读属性来进行适配_

```typescript
interface Test {
  readonly a: string;
  b?: number;
  [key: string]: string | number;
}

const obj: Test = { a: "string" };
const obj2: Test = { a: "string", b: 2 };
const obj3: Test = { a: "string", b: 2, c: "string2" };
obj.a = "test"; // 不被允许
```

TypeScript 具有 ReadonlyArray<T>类型，它与 Array<T>相似，只是把所有可变方法去掉了，因此可以确保数组创建后再也不能被修改：

```typescript
let a: number[] = [1, 2, 3, 4];
let ro: ReadonlyArray<number> = a;
ro[0] = 12; // error!
ro.push(5); // error!
ro.length = 100; // error!
a = ro; // error!
/* 但是可以用断言重写*/
a = ro as number[];
a = <number[]>ro;
```

_对于完全不知道键值或者表单类型，建议使用扩展接口或者 Record_

```typescript
interface ObjRecord<T = unknown> {
    [key: string]: T;
}

Record<string, unknow>;
```

#### 函数接口

函数也可以用泛型来进行定义

```typescript
interface Func {
  (x: number, y: number): number;
}

const addNumber: Func = function (x, y) {
  return x + y;
};
/*函数里面的形参名字和实参名字可以不一致*/
```

### 类类型

```typescript
interface ClockInterface {
  currentTime: Date;
  new (hour: number, minute: number);
}

class Clock implements ClockInterface {
  currentTime: Date;
  constructor(h: number, m: number) {}
}
```

### 接口继承

和类一样，接口也可以相互继承。 这让我们能够从一个接口里复制成员到另一个接口里，可以更灵活地将接口分割到可重用的模块里。

```typescript
interface Shape {
  color: string;
}

interface PenStroke {
  penWidth: number;
}

interface Square extends Shape, PenStroke {
  sideLength: number;
}

let square = <Square>{};
square.color = "blue";
square.sideLength = 10;
square.penWidth = 5.0;

// 接口也可以继承类
class Control {
  private state: any;
}

interface SelectableControl extends Control {
  select(): void;
}

// 错误：“Image”类型缺少“state”属性。
class Image implements SelectableControl {
  select() {}
}
```

## 泛型

```typescript
function add(num1: number, num2: number):number {
    return num1 + num2;
}
// 如果我们要求这个方法同样适用于字符串
funtion add(num1: any, num2: any):any {
    return num1 + num2;
}
```

使用泛型

```typescript
function add<T>(arg1: T, arg: T): T {
    return arg1 + arg2;
}

// 泛型也可以有默认值
function add<T = number>(arg1: T, arg: T): T {
    return arg1 + arg2;
}

或者 const add2: <T>(arg1: T, arg: T) => T = add;

// 使用
let result = add<string>('test', 'test2');  // testtest2
```

泛型的参数名可以随意命名，只是一个形参
同样，接口也可以使用泛型

```typescript
interface GenericIdentityFn<T> {
  (arg: T): T;
}

function identity<T>(arg: T): T {
  return arg;
}

let myIdentity: GenericIdentityFn<number> = identity;

interface SomeType<T> {
  type: string;
  add: (x: T, y: T) => T;
}

// 泛型类
class GenericNumber<T> {
  zeroValue: T;
  add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function (x, y) {
  return x + y;
};
```

有时候会要求泛型必须有某些属性，比如 length，这时候可以使用泛型约束

```typescript
interface LengthWise {
  length: number;
}

function consoleArg<T extends LengthWise>(arg: T): void {
  console.log(arg.length);
}

loggingIdentity(3); // Error, number doesn't have a .length property
loggingIdentity({ length: 10, value: 3 });
loggingIdentity("test");
loggingIdentity([1, 2, 3]);
```

在泛型里使用类类型

```typescript
function create<T>(c: { new (): T }): T {
  return new c();
}
```

### 枚举

```typescript
enum Direction {
  Up = 1,
  Down,
  Left,
  Right,
}
enum Direction {
  Up = "UP",
  Down = "DOWN",
  Left = "LEFT",
  Right = "RIGHT",
}
function setDirection(dir: Direction): void {
  console.log(dir);
}
setDirection(Direction.Up);
```

默认枚举值是从 0 开始，也可以手动设置

const 枚举

常量枚举只能使用常量枚举表达式，并且不同于常规的枚举，它们在编译阶段会被删除。 常量枚举成员在使用的地方会被内联进来。 之所以可以这么做是因为，常量枚举不允许包含计算成员。

```typescript
const enum Directions {
  Up,
  Down,
  Left,
  Right,
}

let directions = [
  Directions.Up,
  Directions.Down,
  Directions.Left,
  Directions.Right,
];
// 生成的代码
var directions = [0 /* Up */, 1 /* Down */, 2 /* Left */, 3 /* Right */];
```

### 类型别名

通常用来表示一个联合类型或者元组类型

```typescript
type Easing = "ease-in" | "ease-out" | "ease-in-out";
let _type: Easing = "ease-in";
_type = "otherEase"; // error: "otherEase" is not allowed here
```

### 高级类型

交叉类型是将多个类型合并成一个类型

```typescript
function extend<T, U>(first: T, second: U): T & U {
  return { ...first, ...second };
}
```

联合类型是一个类型允许是多个类型种的一个

```typescript
let a: number | string;

interface Square {
  kind: "square";
  size: number;
}
interface Rectangle {
  kind: "rectangle";
  width: number;
  height: number;
}
interface Circle {
  kind: "circle";
  radius: number;
}

type Shape = Square | Rectangle | Circle;

function area(s: Shape) {
  switch (s.kind) {
    case "square":
      return s.size * s.size;
    case "rectangle":
      return s.height * s.width;
    case "circle":
      return Math.PI * s.radius ** 2;
  }
}

// 当如果类型缺失，比如
function area(s: Shape) {
  switch (s.kind) {
    case "square":
      return s.size * s.size;
    case "rectangle":
      return s.height * s.width;
  }
}

// 所以建议写上返回值类型，这样可以避免联合类型case缺少
function area(s: Shape): number {
  switch (s.kind) {
    case "square":
      return s.size * s.size;
    case "rectangle":
      return s.height * s.width;
    // 函数缺少结束 return 语句，返回类型不包括 "undefined"
  }
}
```

不用联合类型来处理的话，也可以使用重载的方式

```typescript
function area(s: Square): number;
function area(s: Rectangle): number;
function area(s: Circle): number;

function area(s): number {
  switch (s.kind) {
    case "square":
      return s.size * s.size;
    case "rectangle":
      return s.height * s.width;
    case "circle":
      return Math.PI * s.radius ** 2;
  }
}
```

#### 类型保护

对于一个基本类型而言，有时候会需要将它置为 null 或者 undefined，所以可以在赋值后方添加!来进行表示可空

```typescript
let x: string;
x = "test";
x = null!;
// 或者属性之后加上?表示可选，加了?的属性默认相当于|undefined
interface Person {
  name?: string;
  age?: number;
  say?: () => void;
}

let x: Person = { age: 12 };
x.age = 12;
// 这里
x?.say();
```

#### 索引类型 keyof

```typescript
function pluck<T, K extends keyof T>(o: T, names: K[]): T[K][] {
  return names.map((n) => o[n]);
}

interface Person {
  name: string;
  age: number;
}
let person: Person = {
  name: "Jarid",
  age: 35,
};
let strings: string[] = pluck(person, ["name"]); // ok, string[]
```



```typescript
interface Map<T> {
  [key: string]: T;
}
let keys: keyof Map<number>; // string
let value: Map<number>["foo"]; // number
```

#### 映射类型

TypeScript 提供了从旧类型中创建新类型的一种方式 — 映射类型。 在映射类型里，新类型以相同的形式去转换旧类型里每个属性。 例如，你可以令每个属性成为 readonly 类型或可选的

```typescript
type Readonly<T> = {
  readonly [P in keyof T]: T[P];
};
type Partial<T> = {
  [P in keyof T]?: T[P];
};

type Pick<T, K extends keyof T> = {
  [P in K]: T[P];
};

type Record<K extends string, T> = {
  [P in K]: T;
};

type PersonPartial = Partial<Person>;
type ReadonlyPerson = Readonly<Person>;

type Keys = "option1" | "option2";
type Flags = { [K in Keys]: boolean };

// 等于
type Flags = {
  option1: boolean;
  option2: boolean;
};
```

一些预定义的类型

- Exclude<T, U> -- 从 T 中剔除可以赋值给 U 的类型。
- Extract<T, U> -- 提取 T 中可以赋值给 U 的类型。
- NonNullable<T> -- 从 T 中剔除 null 和 undefined。
- ReturnType<T> -- 获取函数返回值类型。
- InstanceType<T> -- 获取构造函数类型的实例类型。
- Record<T, U> -- 设置对象的类型
- Readonly<T, U> -- 设置只读对象
- Partial<T, U> -- 设置属性可选的对象
- Pick<T, K extends keyof T> -- 设置可选属性

### 命名空间

随着更多验证器的加入，我们需要一种手段来组织代码，以便于在记录它们类型的同时还不用担心与其它对象产生命名冲突。 因此，我们把验证器包裹到一个命名空间内，而不是把它们放在全局命名空间下。

```typescript
namespace Validation {
  export interface StringValidator {
    isAcceptable(s: string): boolean;
  }
}

let validators: { [s: string]: Validation.StringValidator } = {};
```

多个文件可以使用同一个 namespace

### 声明合并

-  接口命名为相同的名字，就会自动进行合并

```typescript
interface Box {
  height: number;
  width: number;
}

interface Box {
  scale: number;
}

// 等同于
interface Box {
  height: number;
  width: number;
  scale: number;
}

let box: Box = { height: 5, width: 6, scale: 10 };
```

但是有另外一个特殊的情况，就是接口函数重载

```typescript
interface Cloner {
  clone(animal: Animal): Animal;
}

interface Cloner {
  clone(animal: Sheep): Sheep;
}

interface Cloner {
  clone(animal: Dog): Dog;
  clone(animal: Cat): Cat;
}

// 等同于，可以使用不同参数的方法进行定义都是可以的
interface Cloner {
  clone(animal: Dog): Dog;
  clone(animal: Cat): Cat;
  clone(animal: Sheep): Sheep;
  clone(animal: Animal): Animal;
}
```

- 命名空间的合并和接口的合并类似

- 命名空间也可以和接口，和类，和枚举进行合并，用来扩展属性

```typescript
class Album {
  label: Album.AlbumLabel;
}
namespace Album {
  export class AlbumLabel {}
} // 合并结果就是类里面有一个内部类

function buildLabel(name: string): string {
  return buildLabel.prefix + name + buildLabel.suffix;
}

namespace buildLabel {
  export let suffix = "!";
  export let prefix = "Hello, ";
}

console.log(buildLabel("Sam Smith")); // Hello, Sam Smith!

// 扩展枚举
enum Color {
  red = 1,
  green = 2,
  blue = 4,
}

namespace Color {
  export function mixColor(colorName: string) {
    if (colorName == "yellow") {
      return Color.red + Color.green;
    } else if (colorName == "white") {
      return Color.red + Color.green + Color.blue;
    } else if (colorName == "magenta") {
      return Color.red + Color.blue;
    } else if (colorName == "cyan") {
      return Color.green + Color.blue;
    }
  }
}

// 类似于
const Color = {
  red = 1,
  green = 2,
  blue = 4,
  mixColor(colorName: string) {
    if (colorName == "yellow") {
      return Color.red + Color.green;
    } else if (colorName == "white") {
      return Color.red + Color.green + Color.blue;
    } else if (colorName == "magenta") {
      return Color.red + Color.blue;
    } else if (colorName == "cyan") {
      return Color.green + Color.blue;
    }
  },
};
```

### 添加申明扩展

```typescript
declare global {
  interface Array<T> {
    toObservable(): Observable<T>;
  }
  interface Window {
    __POWERED_BY_QIANKUN__?: boolean;
  }
}

declare interface Window {
  __POWERED_BY_QIANKUN__?: boolean;
}

Array.prototype.toObservable = function () {
  // ...
};
```

## JSX

### 固有元素拓展

固有元素通过 JSX.IntrinsicElements 来查找，如果不存在这个接口，则所有的元素都会通过，否则只会通过已经申明的

```typescript
declare namespace JSX {
  interface IntrinsicElements {
    foo: any;
    [key: string]: any;
  }
}

<foo />; // 正确
<bar />; // 错误
```

支持属性检查

```typescript
declare namespace JSX {
  interface IntrinsicElements {
    foo: { requiredProp: string; optionalProp?: number };
  }
}

<foo requiredProp="bar" />; // 正确
<foo requiredProp="bar" optionalProp={0} />; // 正确
<foo />; // 错误, 缺少 requiredProp
<foo requiredProp={0} />; // 错误, requiredProp 应该是字符串
<foo requiredProp="bar" unknownProp />; // 错误, unknownProp 不存在
<foo requiredProp="bar" some-unknown-prop />; // 正确, `some-unknown-prop`不是个合法的标识符
```
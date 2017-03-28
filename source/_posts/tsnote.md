---
title: Typescript 学习笔记
date: 2017-03-28 23:03:16
tags: typescript
---
### ts

#### 数据类型

- 布尔值
- 数字
- 字符串
- 数组
- 元组(Tuple)

   表示已知元素数量和类型的数组,越界元素使用联合类型替代

- 枚举(enum)

   包含若干个枚举成员，和枚举值与枚举名的双向映射，枚举较为特殊，他的编译不是减量的，而且常数枚举会被删除

- 任意值

  any类型的值会直接通过编译阶段的检查

- 空值(void)

  默认情况下`null`和`undefined`是所有类型的子类型

- null和undefined

- never

- 类型断言

```javascript
let isDone: boolean = false; //boolean
let a: number = 0xf00d;    //number
let greeting: string = `hello,i am ${name}`;           //string
//array
let list: number[] = [1, 2, 3];
let list: Array<number> = [1, 2, 3]; //数组泛型，Array<元素类型>
//元组 Tuple
let x: [string, number];
x = ['hello', 2]; //ok
x = [2, 'hello']; // error!
x[3] = 'hi'; //ok
x[3] = 3; //ok
x[3] = false; // error! 不属于(string|number)
//枚举
enum Color {Red = 6, Green, Blue};  //red 6, green 7, blue 8
let c: Color = Color.Green;
//任意值
let notSure = 2;
let list: any[] = [1, true, "free"];
//void
let unusable: void = undefined;
let unusable: void = null;
//null & undefined (当strictNullChecks未开启时)
let a: number = null;
let b: string = undefined;

```

#### 接口

​        **我理解**相当于制定的规则，也可用于描述函数类型

- 不检查属性的顺序
- 接口里的属性并非都是必须的
- 名字后加?表示可选属性
- 属性名前加readonly则该属性被指定为只读属性
- 对象字面量会被特殊对待而且会经过 *额外属性检查*，当将它们赋值给变量或作为参数传递的时候。 如果一个对象字面量存在任何“目标类型”不包含的属性时，你会得到一个错误。使用类型断言可以绕过这个问题，将对象赋值给另外一个值也可以绕开对象字面量的检查
- 也可用于描述函数类型,这时不会检查参数的名字，而检查函数返回值是否与接口中匹配   

```javascript
interface FuncA {
    (a: string, b: number): string
}

let newFuncA: FuncA;

newFuncA = function (a: string, b: number) {
    return `${a}b`;
}
```

- 可索引的类型：可索引类型具有一个 *索引签名*，它描述了对象索引的类型，还有相应的索引返回值类型

```javascript
interface StringArray{
    [index: number]: string;
    //同样的，可以在索引签名前加上readonly变成只读的
    //readonly [index: number]: string;
}

let arr: StringArray = ['hello', 'world'];
//用number类型索引得到string类型的值
arr[1];
```

- 接口也可以用来描述类,接口中描述的方法在类里去实现它

```javascript
interface ClockInterface {
    currentTime: Date;
    setDate (d: Date);
}

class Clock implements ClockInterface {
    currentTime: Date;
    //实现接口中的方法
    setDate(d: Date) {
        this.currentTime = d;
    }
    constructor(h: number, m: number) {
    }
}
```

#### 函数

​        函数类型包括两部分：参数类型和返回值类型。

- ts中的函数参数都是必传的，编译器会检查是否每个参数都传入了值
- 在参数后添加? 可设置该参数为可选参数
- 也可以为参数设置默认值
- 剩余参数 restOfName(对比js中的arguments，arguments表示传入函数的所有参数，在ts中，restOfName表示剩余的参数)

```javascript
function buildName(firstName: string, ...restOfName: string[]) {
  return firstName + " " + restOfName.join(" ");
}

let employeeName = buildName("Joseph", "Samuel", "Lucas", "MacKinzie");
```

- ts中的函数重载利用any

```javascript
function pickCard(x): any {
  if (typeof x == "object") {

    }
    // Otherwise just let them pick the card
    else if (typeof x == "number") {

    }
}
```

### 类

```javascript
class Animal {
    name:string;
    constructor(theName: string) { this.name = theName; }
    able(skill: string) {
        console.log(`${this.name} is able to ${skill}.`);
    }
}

class Cat extends Animal {
    constructor(name: string) { super(name); }
    able(skill) {
        super.able(skill);
    }
}

let tom = new Cat("tom");
tom.able("miaomiao");
```



##### 修饰符(public/private/protect/readonly)

##### plublic

ts 中，成员默认为` public` 的, 当然，我们也可以显式的将成员标记为 public 的

##### private

当成员被标记成`private`时，它就不能在声明它的类的外部访问

```javascript
class Cat extends Animal {
    constructor(name: string) { super(name); }
    private catchMouserNo: number;
    able(skill) {
        super.able(skill);
    }
}

let tom = new Cat("tom");
tom.able("miaomiao");
tom.catchMouserNo;
//Property 'catchMouserNo' is private and only accessibale within class 'Cat'
```

##### protected

与 `private`类似，不能在声明它的类外使用，但是在派生类中仍然可以访问

```javascript
class Animal {
    public name: string;
    protected age: number;
    public constructor(theName: string, age: number) {
        this.name = theName;
        this.age = age;
    }
}

class Cat extends Animal {
    constructor(name: string, age: number) { super(name, age); }
    public showAge() {
        console.log(this.age);
    }
}

let tom = new Cat("tom", 2);
tom.showAge();  //2
new Animal("dog", 12).age;
//Propoty 'age' is protected and only accesible within class 'Animal' and its subclasses
```



PS: 构造函数也可以被标记成`protected`。 这意味着这个类不能在包含它的类外被实例化，但是能被继承

##### readonly

`readonly`修饰符将属性设置为只读的，只读属性只能在声明时或构造函数中进行初始化

##### 参数属性

给构造函数参数添加访问修饰符，可以将声明和赋值合并在一起

可以将上面的 Animal 类用如下方式改写

```javascript
class Animal {
    public constructor(public theName: string, protected age: number) {}
}
```

##### 存取器

```javascript
class Animal {
    private _age: number;
    set age(age: number){
        if (age > 10) {
            console.log('it is too old');
        } else {
            this._age = age;
        }
    }
    get age() {
        return this._age;
    }
}

let dog = new Animal();
dog.age = 10;
dog._age = 10; //Property '_age' is private and only accessibale within class 'Animal'
```

相当于是通过 get 和 set 方法来简洁的操作成员变量，提高了变量的安全性(我们可以在存取值时做一些逻辑判断的工作)

#### 静态属性

静态属性存在与类本身上而并不是类的实例上**我们可以认为类具有 *实例部分*与*静态部分*这两个部分**

```javascript
// 编译前
class Animal {
    static info = {
        type: "pet",
        maxAge: 20
    };
    getInfo(): void {
        console.log(Animal.info.type, Animal.info.maxAge);
    }
}

let catA = new Animal();
catA.getInfo();
// pet 20

// 编译后 可以看到 info 属性是在 Animal 上的而并非是在 catA 实例上
var Animal = (function () {
    function Animal() {
    }
    Animal.prototype.getInfo = function () {
        console.log(Animal.info.type, Animal.info.maxAge);
    };
    return Animal;
}());
Animal.info = {
    type: "pet",
    maxAge: 20
};
var catA = new Animal();
catA.getInfo();
```

#### 抽象类

抽象类作为其他派生类的基类，`abstract`关键字用于声明抽象类以及抽象类中的抽象方法

```javascript
abstract class Animal {
    abstract able(): void;
}

class Cat extends Animal {
    able(): void {
        console.log("cat can miaomiao!");
    }
}

let catA = new Cat();
catA.able();
// cat can miaomiao!
```

PS:这里的派生类 Cat 必须要包含父类 Animal 的抽象方法的实现，否则 ts 将给出错误提示

#### 泛型

可用于编写可复用的组件，使组件可以支持多种的数据类型

场景：写一个方法 使得传入的参数类型和返回值得类型保持一致

```javascript
function identity<T>(arg: T): T {
    return arg;
}

let a = identity<number>(123);
let b = identity<string>("this is a string");
let c = identity<boolean>(false);
let d = identity<number[]>([1, 2, 3, 4]);

console.log(a, b, c, d); //123 "this is a string" false [1, 2, 3, 4]
```

这里使用了一个类型变量 T ，T帮助我们捕获用户传入的类型，我们把这个 identity 函数叫做**泛型函数**

我们也可以在使用 identity 函数时不传入类型，因为编译器会为我们进行**类型推论**

```javascript
let a1 = identity(123);
let b1 = identity("this is a string");
```

##### 泛型接口

```javascript
interface GenericIdentityFn<T> {
    (arg: T): T;
}

function identity<T>(arg: T): T {
    return arg;
}

let myIdentity: GenericIdentityFn<number> = identity;
let myIdentity1: GenericIdentityFn<string> = identity;
```

这里传入类型参数来指定泛型类型

##### 泛型类

```javascript
class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function (x, y) { return x + y; };

let a = new GenericNumber<string>();
a.zeroValue = 'string!!';
a.add = function (x, y) { return x + y };
```

泛型类约束的是类的实例部分的类型，静态属性不能使用这个泛型类型。

#### 枚举

包含若干个枚举成员，和枚举值与枚举名的双向映射，枚举较为特殊，他的编译不是减量的，而且常数枚举会被删除

```javascript
//编译前
enum Direction {
    Up = 1,
    Down,
    Left,
    Right
}

let down = Direction.Down;
let nameOfDown = Direction[down];

console.log(down);  // 2
console.log(nameOfDown);  //Down

//编译后
var Direction;
(function (Direction) {
    Direction[Direction["Up"] = 1] = "Up";
    Direction[Direction["Down"] = 2] = "Down";
    Direction[Direction["Left"] = 3] = "Left";
    Direction[Direction["Right"] = 4] = "Right";
})(Direction || (Direction = {}));
var down = Direction.Down;
var nameOfDown = Direction[down];
console.log(down);
console.log(nameOfDown);
```

```javascript
//编译前
const enum Direction {
    Up = 1,
    Down,
    Left,
    Right
}
//编译后
let list = [Direction.Up, Direction.Left];
```



#### 类型的兼容

- 接口

接口的兼容性原则是：如果 x 要兼容 y ,那么 y 至少具有与 x 相同的属性(目标的成员会被一一检查)

```javascript
interface Struc {
    a: Number
};

let x: Struc;

let y = { a: 111, b: 'str1'};

x = y;  //OK
y = x;  //Error

```



- 函数

在js中，函数参数并不是全部都是必传的，所以在ts中，如果 x 要兼容 y ,x 的每个参数必须要在 y 中找的对应类型的参数

```javascript
let y = (a: number) => 0;
let x = (b: number, s: string) => 0;

x = y; // OK
y = x; // Error
```

对于返回值类型，x 若要兼容 y ，那么 y 的返回值必须是 x 的返回值得子集

```javascript
let x = () => ({name: 'Alice'});
let y = () => ({name: 'Alice', location: 'Seattle'});

x = y; // OK
y = x; // Error because x() lacks a location property
```



#### 命名空间

```javascript
// 编译前
namespace Validation {
    export interface StringValidator {
        isAcceptable(s: string): boolean;
    }

    const lettersRegexp = /^[A-Za-z]+$/;
    const numberRegexp = /^[0-9]+$/;

    export class LettersOnlyValidator implements StringValidator {
        isAcceptable(s: string) {
            return lettersRegexp.test(s);
        }
    }

    export class ZipCodeValidator implements StringValidator {
        isAcceptable(s: string) {
            return s.length === 5 && numberRegexp.test(s);
        }
    }
}

// 编译后
var Validation;
(function (Validation) {
    var lettersRegexp = /^[A-Za-z]+$/;
    var numberRegexp = /^[0-9]+$/;
    var LettersOnlyValidator = (function () {
        function LettersOnlyValidator() {
        }
        LettersOnlyValidator.prototype.isAcceptable = function (s) {
            return lettersRegexp.test(s);
        };
        return LettersOnlyValidator;
    }());
    Validation.LettersOnlyValidator = LettersOnlyValidator;
    var ZipCodeValidator = (function () {
        function ZipCodeValidator() {
        }
        ZipCodeValidator.prototype.isAcceptable = function (s) {
            return s.length === 5 && numberRegexp.test(s);
        };
        return ZipCodeValidator;
    }());
    Validation.ZipCodeValidator = ZipCodeValidator;
})(Validation || (Validation = {}));

```





### 三斜线指令

- 仅可放在包含它的文件的最顶部

- 用于声明文件间的依赖，告诉编译器在编译过程要引入其他的文件

- 相对于包含它的文件

  ​

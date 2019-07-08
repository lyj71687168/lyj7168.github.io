### 关于ts
ts是js的超级，提供了对类型系统和es6的支持，由microsoft开发

### 安装和编译
+ npm i -g typescript
+ tsc hello.ts

### 类型注解
Typescript的类型注解可以为函数或者变量添加类型的约束方式，使用冒号指定变量的类型，ts会进行静态检查，如果发现错误，编译的时候会报错
通常，编译报错也会生成js文件，如果终止了js文件的生成，可以在tsconfig.json中配置noEmitOnError

### tsconfig.json
+ tsconfig.json指定了编译这个项目用的根文件和编译选项
+ 调用tsconfig.json
  - 不带任何文件调用 tsc, 编译器从当前目录向父级逐级查找
  - 不带任何文件条用 tsc命令，但是后面加上 --project(-p)参数，可指定一个包含tsconfig.json文件的项目目录

+ tsconfig文件
  ```json
  {
    "compileOnSave": true,
    "compilerOptions": {
      "module": "commonjs",
      "noImplicitAny": true,
      "removeComments": true,
      "preserveConstEnums": true,
      "sourceMap": true,
      "typeRoots" : ["./typings"],
    },
    "files": [
      "hello.ts",
    ],
    "include": [
      "src/**/*"
    ],
    "exclude": [
      "node_modules",
      "**/*.spec.ts"
    ]
  }
  ```
  - compilerOptions 属性可以被忽略，编译器将使用默认值
  - files 指定一个包含相对或绝对路径的文件列表
  - include & exclude 指定文件glob匹配模式列表
    - 支持的glob通配符有 *(0个或多个字符，不含目录分隔符)  ?(一个字符) **(递归匹配任意子目录)
      - 如果files和include都没有被指定，默认包含当前目录及子目录下的所有ts, tsx, .d.ts文件，排除在exclude里面指定的文件，
      - 使用outDir指定的目录永远会被编译器排除，除非用files明确的指定进来，exclude没用。使用include指定的文件会被exclude过滤，但是files中的不会被过滤
  - tsconfig.json文件可以是个空文件，那么所有默认的文件（如上面所述）都会以默认配置选项编译。
 
  - 默认的所有的@type包都会被编译进来，node_modules/@types里面的子文件夹等等，如果指定了typeRoots,那么只有它下面的包才会被包含进来
  - 如果指定了types，只有被列出来的包才会被包含进来。 比如：
    ``` json
    "compilerOptions": {
          "types" : ["node", "lodash", "express"]
    }
    ```
  - 使用extend继承配置，tsconfig.json文件可以利用extends属性从另一个配置文件里继承配置。 
    extend是tsconfig.json里面的顶级属性，与compilerOptions等一样，他的值是一个字符串，指向要继承的文件的路径
    在原文件里面的配置会预先被加载，然后被来自继承文件里面的配置重写，

  - 在最顶层用compileOnSave为true,可以让编辑器在保存文件的时候，根据tsconfig.json重新生成文件。


### 数据类型
+ 声明布尔值，数字和字符串(关键字 变量名:类型 = 值) 
  + 布尔值
  ```js
  let isDone : boolean = false //可编译
  let isDone1: boolean = new Boolean(1)  //报错，new Boolean创建的是一个布尔对象，
  let isDone1: Boolean = new Boolean(1)  //可编译
  let isDone1: boolean = Boolean(1)  //可编译，直接调用Boolean可以返回布尔类型
  ``` 
  let isDone: boolean = true
  let num: number = 123
+ 声明数组
  ```js
  let list : number[] = [1,2,3]
  let list : Array<number> = [1,2,3]
  let list : any[] = [1,'2']  // 数组中允许出现任意类型
  ```
  类数组
  类数组不是数组类型，比如arguments,事实上常见的类数组都有自己的接口定义，如 IArguments, NodeList, HTMLCollection
  ```js
  function sum() {
    let args: IArguments = arguments
  }
  ```
+ 元组:
  表示一个已知元素个数和数据类型的数组
  let x: [string,number]
  x = ['hello','10']         
+ 枚举:
  是对js数据类型的一个补充，可以为一组数字提供标识
  enum Color {Red,Blue,Green}
  let c:Color = Color.Blur
  可以为枚举类型手动赋值，通过这个值得到对应的数据
  enum Color {Red=1,Blur=3,Green=5}
  let colorName : string = Color[3]
+ Any:
  为编程阶段不清楚的类型指定一个变量，比如用户输入的或者来自第三方代码库的，可以认为，声明一个变量为任意值之后，对它的任何操作，返回的内容的类型都是任意值。
  变量如果在声明的时候，未指定其类型，那么它会被识别为任意值类型.
  let notSure:any = 4
+ Void:
  void类型与any类型相反，表示没有任何类型，一个函数没有返回值的时候返回的是void,声明一个void类型只能为它赋值undefined和null
  ```js
  function alertName(): void {
    alert('My name is Tom');
  }
  let unusable: void = undefined;
  ```
+ undefined 和 null:
  与void的区别是，undefined和null类型是所有类型的子类型
  ```js
  let num:number = undefined
  ```
+ symbols:
  自ES6起，symbol成为了新的原生类型，symbols是通过Symbols构造函数创建的，它是 不可改变且唯一的
+ 联合类型:
  可以取值为多种类型中的一种
  ```js
  let myFavoriteNumber: string | number;
  myFavoriteNumber = 'seven';
  myFavoriteNumber = 7;
  ```
+ 接口: 使用接口来定义对象的类型:
  ```js
  interface Person {
    firstName: string;
    lastName: string;
  }
  let tom: Person {
    firstName : 'Tom',
    lastName: 18  // 变量的形状必须和接口的保持一致，如果不完全一致，那么用可选属性
  }
  // 可选属性
  interface Person {
    name: string;
    age?: number;
  }
  let tom: Person = {
      name: 'Tom'   // 但是不允许添加未定义的属性
  };
  // 任意属性：一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集
  interface Person{
    name: string;
    age?: number;
    [propName: string]:any
  }
  let tom: Person {
    name: 'Tom',
    gender: 'man'
  }
  ```
+ 函数
  ```js
  // 函数声明
  function sum(x:number, y?:number):number { // y是可选参数，可选参数需要放在必选参数的后面
    return x+y
  }
  // 函数表达式
  let sum : (x:number,y:number) => number = function (x:number,y:number):number {}
  // 这里的=>和es6的箭头函数不同，在ts中用来表示函数的定义，左边表示输入类型，右边表示输出类型
  ```  

### 类型断言，用来手动指定一个值得类型，<类型>变量 或者 变量as类型
  当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，我们只能访问此联合类型的所有类型里共有的属性或方法：
  ```js
  function getLength(something: string | number): number {
    return something.length;
  }
  // 会报错，因为length不是string和number的共有方法
  // 采用类型断言
  function getLength(something: string | number): number {
    if ((<string>something).length) {
        return (<string>something).length;
    } else {
        return something.toString().length;
    }
  }
  ```

### 声明文件
  当使用第三方库时，我们需要引用它的声明文件，才能获得对应的代码补全、接口提示等功能。
+ 声明语句
  例如引入了第三方库jquery,直接用$('#app')是会报错的，需要用声明语句来定义它
  ```js
  declare var jQuery: (selector: string) => any;  //全局变量声明文件的方式
  ```
+ 声明文件
  把声明语句单独的放到一个文件中，就是声明文件，必须以.d.ts为后缀   
+ 第三方声明文件
  一些库的社区已经声明文件不需要我们定义了，社区已经定义好了，但是更推荐@type的方式统一管理第三方库的声明文件
  ```js
  npm install @types/jquery --save-dev //直接用 npm 安装对应的声明模块即可
  ```

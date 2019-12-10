ES6

## 一、简介

ES 全称：**ECMAScript**

### 1、ES6 优点：

**1、 默认参数**

~~~JavaScript
//ES5
function hello(txt) {
    txt = txt || "hello world";
}
~~~

~~~java
//ES6
function hello(txt='hello world') {
    txt = txt;
}
~~~

 **2、字符串模版**

~~~javascript
//ES5
var compiled = _.template("hello: <%= name %>"); // _ 使用下划线需要额外引入一个包
complied({name: 'moe'});
~~~

~~~javascript
//ES6
var name = 'moe';
var txt = `hello ${name}`;
~~~

### 2、ES6更多特性

- 解构赋值
- 箭头函数
- Set 和 Map
- 异步操作
- 类与对象
- 模块化

### 3、相关框架

- Vue.js
- Element
- D3(处理图表框架)
- React
- Angular

### 4、基本技能

1. 工具
   1. **gulp** （任务自动化）
   2. **babel** （编译工具）
   3. **webpack**（编译工具）
   4. **npm**
   5. express
   6. mockjs

## 二、项目构建

### 1、基础架构

- 业务逻辑
  - 页面
  - 交互
- 自动构建
  - 编译
  - 辅助
    - 自动刷新
    - 文件合并
    - 资源压缩
- 服务接口
  - 数据
  - 接口

### 2、项目目录创建

**1、npm修改镜像源：**

~~~sh
npm config set registry https://registry.npm.taobao.org/
~~~

[参考](https://blog.csdn.net/palmer_kai/article/details/79391323)

**2、安装Express 应用程序生成器**

```sh
 npm install express-generator -g
```

[参考](http://www.expressjs.com.cn/starter/generator.html)

**3、构建ejs程序**

~~~sh
express -e . //创建脚手架命令， -e表示使用 ejs模版引擎 .表示在当前目录执行
~~~

~~~sh
npm install
~~~

4、创建相关文件

~~~sh
cd ..\tasks
mkdir util
cd .>util/args.js

#回到项目根目录
npm init
#创建下面两个文件，命名必须为这样，否则编译是会报错
cd >.babelrc
cd >gulpfile.babel.js
~~~






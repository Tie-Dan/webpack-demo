## Webpack进阶

### 核心打包原理

**打包工作的主要流程如下：**

1. 需要读到入口文件里的内容
2. 分析入口文件，递归的去读取模块所依赖的文件内容，生成AST语法树
3. 根据AST语法树，生成浏览器能够运行的最终代码

**具体细节:**

1. 获取主模块内容
2. 分析模块内容
   - 安装@babel/parser包（转AST）
3. 对模块内容处理
   - 安装@babel/traverse包（遍历AST）
   - 安装@babel/core和@babel/preset-env包（Es6转Es5） 
4. 递归所有模块
5. 生成最终代码

### 手写loader

loader本质上就是一个函数，这个函数会在我们加载一些文件时执行

#### 实现同步loader

1. 初始化项目
2. 创建sync函数并返回
   - 注意:函数返回值必须是一个buffer或者string类型
   - 注意: loader中的this有很多信息 所以一定不要使用箭头函数
   - 使用loader-utils包 完成更复杂loader的编写
     - 通过loader-util获取message信息
     - 返回处理过后的值
       - return 返回单条信息
       - this.callback 函数返回多个参数

3. webpack.config.js配置loader

   <img src="img/8.png" style="zoom:30%;" />

#### 实现异步loader

1. 在loader中使用this.async()方法 异步操作

   <img src="img/9.png" style="zoom:30%;" />

### 手写plugin

plugin通常是在webpack在打包的某个时间节点做一些操作，我们使用plugin的时候，一般都是new Plugin()这种形式使用，所以，首先应该明确的是，plugin应该是一个类。

plugin类里面需要实现一个apply方法，webpack打包时候，会调用plugin的aplly方法来执行plugin的逻辑，这个方法接受一个compiler作为参数，这个compiler是webpack实例。

1. 创建一个demoPlugin类
2. 编写compiler hooks的同步和异步写法

**参考:**[Compiler Hooks](https://v4.webpack.js.org/api/compiler-hooks/)








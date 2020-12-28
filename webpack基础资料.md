# Webpack资料

## 介绍

​	webpack是一个前端构建工具。

​	前端构建工具就是把开发环境的代码转化成运行环境代码。一般来说，开发环境的代码是为了更好的阅读，而运行环境的代码则是为了能够更快地执行。因此开发环境和运行环境的代码形式也不相同。比如，开发环境的代码，要通过混淆压缩后才能放在线上运行，这样代码体积更小，而且对代码执行也不会有任何影响。一般需要构建工具处理的几种情况：

前端构建工具就是把开发环境的代码转化成运行环境代码。一般来说，开发环境的代码是为了更好的阅读，而运行环境的代码则是为了能够更快地执行。因此开发环境和运行环境的代码形式也不相同。比如，开发环境的代码，要通过混淆压缩后才能放在线上运行，这样代码体积更小，而且对代码执行也不会有任何影响。一般需要构建工具处理的几种情况：

- 代码压缩

  将JS、CSS代码混淆压缩，让代码体积更小，加载更快

- 编译语法

  编写CSS时使用Less、Sass，编写JS时使用ES6、TypeScript等，这些标准目前都无法被浏览器兼容，因此需要构建工具编译，例如使用Babel编译ES6语法。

- 处理模块化：

  CSS和JS的模块化语法，目前都无法被浏览器兼容。因此开发环境可以使用既定的模块化语法，但是需要构建工具将模块化语法编译为浏览器可识别形式。例如使用webpack、Rollup等处理JS模块化。

- 其他的构建工具：

  ​	最早普及使用的是Grunt，后面又出现Gulp。Webpack是目前流行的构建工具，可以说是构建工具的神器，学习成本较高。

## 什么地方适合使用

适合场景：

1. vue、react、angular 都可以通过webpack构建

2. 全部可供访问的页面数量不超过500个

## 安装配置

webpack官网：<https://webpack.docschina.org/>

步骤：

> npm   i   webpack   webpack-cli   --save-dev

1.  npm

   ```js
   npm i	xxx --save
   npm i xxx --save-dev
   
   --save 会把依赖包名称添加到 package.json 文件 dependencies 键下
   --save-dev 则添加到 package.json 文件 devDependencies 键下
   
   dependencies是运行时的依赖
   devDependencies是开发时的依赖
   ```

2. Yarn

   ```js
   yarn add jquery // 表示通过“运行依赖”方式安装jquery后边没有参数，运行依赖也有参数，为--save 简称为 -S，但是可以不用设置
   yarn add webpack -D // webpack后边有-D参数，表示通过“开发依赖”方式安装 开发依赖参数为 --save-dev 简称为 -D
   ```

- 为什么要设置-D参数呢

  ​	项目开发完毕可能会上传到网络(例如github)，也有可能给朋友分享，无论哪种方式我们代码分享出去是不带**node_modules**目录的，所以我只需要拿到package.json安装依赖就行。

## 案例

​	在根目录下创建src文件夹，其中一个是入口文件app.js，一个是写内容的work.js。入口文件用来引入真正写页面的JS文件、CSS文件。

work.js内容：

```js
document.write('铁蛋儿真帅')
```

app.js内容：

```js
let dt = require('./work.js')
```

然后，再返回上一层目录，新建index.html文件（该文件和src属于同一层级），内容是：

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8"> 
        <title>test</title> 
    </head>
    <body> 
        <div>test</div> 
        <script src='./dist/main.js'></script> 
    </body> 
</html>
```

在命令行中执行：

```
webpack app.js
```

## 配置打包模式

​    给webpack配置打包模式，不配置打包会提示黄色

步骤：

1. 项目根目录创建webpack配置文件，名称为 webpack.config.js

2. 给webpack.config.js做如下配置

   <img src="img/1.png" style="zoom:50%;" />

   > production： 生产模式，打包的文件是优化压缩的
   >
   > development：开发模式，打包的文件有适当的回车、空白、注释
   >
   > 前期使用development，项目开发完毕即将上线就用production

## 配置入口和出口文件

项目主模板文件：index.html

项目主入口文件：src/app.js

项目出口文件：dist/main.js 

现在我们要对入口和出口文件做配置

入口：src/app.js

出口：dist/main.js

步骤：

1. 配置快速启动项package.json

   ```js
   "scripts": {
     "build": "webpack"
   },
   ```

2. 给webpack.config.js做如下配置

   ```js
   // webpack中引入其他的模块要使用commonJS模块化require导入 因为webapck是nodeJS开发的 不能使用import导入
   const ph = require('path')
   module.exports = {
       mode: 'development',
       entry: ph.resolve('./src/app.js'), // 绝对路劲
       output: {
           path: ph.resolve('./dist'), // 配置出口目录
           filename: "main.js" // 配置出口文件名称
       }
   }
   ```

3. 打包 npm run build


## 编译模板页面

现在给项目做打包处理，要通过手动方式在index.html中引入打包好的main.js文件，太笨了

webpack有一个插件，可以实现同时**打包/(复制)**index.html到达dist目录，并**自动**就引入main.js文件，我们要做到的事情就是直接运行打包好的模板文件即可

实现步骤：

1. 安装工具 yarn  add  html-webpack-plugin  -D

2. 在webpack.config.js中配置如下信息：

   ![](img/2.png)

3. 在index.html模板中不用引入任何的js文件了

4. 做物理打包 npm run build（编译生成模板文件了，并且有自动引入main.js文件）


## 实时打包

项目开发都是对src目录内部的文件进行更新，不要去修改dist打包好的文件

现在对src内部的任何文件做修改操作后，都需要重新打包，才可以看到对应效果

webpack本身有一个工具，名称为 webpack-dev-server，可以实现随时修改源文件，浏览器随时看到修改后的效果，不需要反复打包，这样就非常好。

webpack-dev-server安装运行起来之后，会给我们创建一个http的web服务。

**步骤：**

1. 安装 yarn  add webpack-dev-server

2. 在webpack.config.js中做如下配置

   ![](img/3.png)

3. 在package.json中做如下配置

   ![](img/4.png)

4. 通过  npm run serve 就可以实现 实时打包、实时编译、实时浏览器查看效果了

注意：

1. npm run  serve指令执行后，其是一个“前台”进程，不能关闭
2. 浏览器看到的实时效果是服务器通过“**内存**”提供的，没有物理文件，也不会生成dist目录

## 运行css文件

在当前项目中创建css文件并做引入使用会报错

1. 创建css文件    src/css/1.css，  和简单的样式    li{color:red;}

2. 在app.js中引入css文件  ，   import  './src/css/1.css'

3. 此时实时打包报错了

   错误提示：需要一个适当的loader来处理css文件


## loader-介绍

webpack很厉害，可以打包处理不同的内容(css/img/less/es6、es7等等)，但是具体处理工作webpack不参与，具体交给手下  loader去处理，loader是小兵，帮助webpack对不同内容做编码、降级处理

准确定义：

webpack本身就是一个**打包机器**，其不能对具体代码内容部分进行**处理**(或处理得非常有限)，不同的代码内容(less/scss/ES6(ES7)/image/css等等)需要webpack找到不同的**loader**(装载器)实现转码、编译、降级处理。

## loader-安装配置css相关loader

css内容相关的loader有：style-loader 和 css-loader

安装配置步骤：

1. 安装 yarn  add  css-loader  style-loader  -D   

2. 在webpack.config.js中做如下配置(**注意use的顺序一定是如下**)

   ![](img/5.png)

3. 现在重新 实时打包  npm  run  serve发现 css文件的样式已经生效

## loader-引入背景图片应用

1. 创建目录  src/img  并把两个图片(one.jpg  two.jpg)存入好
2. 给模板文件 index.html创建两个div
3. 在1.css样式文件中给两个div设置样式  和 背景图片
4. 现在实时打包报错了 


## loader-安装配置图片相关loader

img图片相关的loader有两个：url-loader   和   file-loader

安装配置loader步骤：

1. 安装 yarn  add  url-loader  file-loader -D
2. webpack.config.js中做具体配置，如下

```js
{
  //  图片处理loader配置
  test: /\.(png|jpg|gif)$/i,  // 正则匹配图片文件
  // 遇到图片文件就交给如下loader处理
  use: [
    {
      loader: 'url-loader',
      options: {
        // limit:设定大小阀值
        // a. 被处理图片大小 大于该阀值，就通过物理文件重新生成该图片
        // b. 被处理图片大小 小于等于该阀值，就把图片变为字符串(嵌入到应用文档中，好处是节省一个http资源)
        limit: 8192,
        // 做配置，使得生成的物理图片被存储在dist/images里边
				outputPath: "images",     
      }
    }
  ]
},
```

loader说明：

1. 只配置url-loader，file-loader不用配置，条件满足后url-loader会自动调用file-loader执行
2. limit:8192设置图片判断大小阀值的，一般建议是10k左右，原因是图片变为字符串大小会增加的(过大图片变为字符串我们就没有"利润"了)

```
url-loader: 负责把大小小于等于阀值的图片变为字符串
file-loader: 负责把大小 大于阀值的图片重新以物理文件形成生成在dist目录
```

注意：

​	图片loader只能处理css文件的**背景图片**，而index.html模板中通过img标签做的图片不给处理(只把其当做标签的普通属性了)

3. 现在重新实时打包 npm run serve，发现页面上两个div已经有背景图片效果了

## loader-应用less文件

1. 创建 src/css/2.less文件，并设置简单样式 ul{ li{border:2px solid orange;} }
2. 在app.js中引入less文件, import  './css/2.less'
3. 此时实时打包报错，告诉less缺少对应loader来处理

## loader-安装配置less相关loader

loader具体为：less-loader、less

步骤：

1. 安装依赖包， yarn  add less-loader  less -D

2. webpack.config.js做如下配置

   ```js
   {
     //less处理loader配置
     test: /\.less$/,
     use: ["style-loader", "css-loader", "less-loader"]
   },
   ```

   说明：

   ​	less样式文件处理需要3个loader，具体为上述，它们有严格的顺序，它们有做工作交接

   ​	它们执行的顺序是颠倒的(less>css>style)

   ```css
   style-loader：负责生成style标签，把css样式体现出来，之后该标签嵌入到应用文档中去
   css-loader：使得css文件可以通过import引入，并合并到main.js中
   less-loader: 该loader负责把less文件内容转变为 css内容
   ```

3. 现在在重新实时打包运行  npm run serve，发现less设置样式已经生效

## loader-运行es6标准代码

步骤：

1. 在work.js中应用es6内容(let、箭头函数、对象解构赋值、...展开运算符等等)

   ```
   // 对es6高级内容做应用
   let age = 18
   let getShuai = ()=>{
     return '铁蛋儿'
   }
   ```

2. 给项目做物理打包 npm run build

3. 发现情况不好，在main.js中生成的内容还是es6高级的信息

   > 我们本意是要把es6变为es5的，但是失败了


## loader-babel-loader和preset和plugin关键字解读

能够把es6高级内容变为es5的loader名称为 **babel-loader**

实际处理是这样的

es6/es7/es8等等高级标准有很多(let、箭头函数、对象解构赋值、...展开运算符、反勾号字符串等等)，每个标准都需要一个独立的**plugin**进行降级处理，如果使用许多高级标准内容，那么势必要为此安装许多plugin，这样工作比较繁琐，系统已经考虑到这点了，其通过**preset**把许多**常用**的plugin给做了集合，因此一般性的使用只需要安装preset即可搞定(如果项目应用到了一个生僻的高级标准内容，preset处理不来，就还需要再安装对应的plugin处理)

## loader-安装配置loader和preset做降低处理

babel-loader官网：<https://babel.docschina.org/>

步骤：

1. 安装依赖包,yarn add babel-loader @babel/core @babel/preset-env -D

2. 在webpack.config.js中做如下配置：

   <img src="img/7.png" style="zoom:50%;" />

   

3. 在项目根目录创建 babel.config.js文件，配置如下

   作用：使得babel-loader可以找到preset做代码降级处理

   <img src="img/6.png" style="zoom:50%;" />

4. 现在给项目做物理打包 npm run build,发现高级内容已经降级处理了

   






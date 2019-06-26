### webpack

webpack是javascript应用程序的静态模块打包器

例如项目中的sass、less这些文件不能被浏览器所识别这时就需要使用到打包工具

`loader`用于打包某些特定类型的格式 例如在js中引入css文件

`plugins`作用于整个打包过程

**常用的打包工具**

rollup	主要用来打包javaScript

parcel

FIS

- ###### 安装

```js
npm init
npm i webpack webpack-cli -D
```

- 在`package.json`中`script`写入

  - 使用production时代码会进行压缩
  - 使用development代码不会进行压缩

  ```js
  "script":{
      "build": "webpack --mode --production"
  }
  ```

- `webpack.config.js`向外暴露一个node的配置，webpack就会去读取这个文件然后进行打包，在该文件总写入，入口和出口文件

- 指定`webpack.config.js`文件在哪个目录下

  ```js
  "script":{
      "build": "webpack --mode --production --config scripts/webpack.config.js"
  }
  ```

  

- ######  入口(entry)

作用：指示webpack应该使用哪个模块，来作为其内部依赖。进入入口起点后，webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的。

默认值：`./src`

```js
module.exports = {
  entry: './src/index.js',  
};
```

入口文件可以写多个，需要以对象的格式写

不设置出口文件时，默认使用当前key作为文件名

```js
module.exports = {
  entry: {
      home: './src/home.js',
      about: './src/about.js'
};
```

- ###### 出口(output)

作用：告诉webpack在哪里输出它所创建

不配置`filename`时默认为main.js

`filename`：'[name].js' 时也是使用当前key作为文件名

默认值：./dist

```js
// 使用node中的路径解析模块
const path = require('path');
module.exports = {
  entry: './src/index.js',  
};

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'), 
    //使用chunkHash为文件名称动态生成hash值，每个文件的hash值不一样，8代表hash位数，不会同时更改
    // 在名字前加js会自动生成一个js目录并将文件添加到该目录下
    filename: 'js/[name].[chunkHash:8].js'
  }
};
```

使用`[hash]`时：

![1557834363639](C:\Users\hy\AppData\Roaming\Typora\typora-user-images\1557834363639.png)

使用`[hash:8]`时，不同的文件生成的相同的hsah值：

![1557834448233](C:\Users\hy\AppData\Roaming\Typora\typora-user-images\1557834448233.png)

当更改`home`文件时，同时更改：

![1557834843102](C:\Users\hy\AppData\Roaming\Typora\typora-user-images\1557834843102.png)

使用`[chunkHash:8]`时,每个文件的hash值不一样：

![1557834710999](C:\Users\hy\AppData\Roaming\Typora\typora-user-images\1557834710999.png)

当更改`home`文件时，不会同时更改：

![1557834776885](C:\Users\hy\AppData\Roaming\Typora\typora-user-images\1557834776885.png)

- ###### 使用`__dirname`和`process.cwd()`的区别

`__dianame`被执行的js文件所在目录——config文件所在目录

`process.cwd()`默认生成在dist目录下

- 当在html中引入js文件时，每次更改js文件都会动态生成一个新的文件，这时在html中就需要重新引入文件，这时就需要使用插件

  ```js
  npm i --save-dev html-webpack-plugin
  ```

  在配置文件中写入，这时就会自动生成index.html文件并自动引入当前文件下的js文件

  ```js
  const HtmlWebpackPlugin = require("html-webpack-plugin")
  
  plugins: {
      new HtmlWebpackPlugin({
          title: 'webpack',
          // 在dist下的index.html中引入模板文件
          template: 'public/index.html'
      })
  }
  ```

  使用模板语法

  在public/index.html中修改title

  ```html
  <title><%= htmlWebpackPlugin.options.title %></title>
  ```

- `css-loader`

- `style-loader`把处理的结果放在编译的js文件中去，等到网页运行的时候再动态插入到网页的头部style中去

  ```js
  npm i --save-dev css-loader
  ```

  ```js
  module: {
      rules: [
          {
              test: /\.css$/,
              use: [
                  'style-loader',
                  'css-loader'
              ]
          }
      ]
  }
  ```

  用于提取css代码，生成独立的css文件，并自动插入到html文件中去

  ```js
  npm install --save-dev mini-css-extract-plugin
  ```

  ```js
  const MiniCssExtractPlugin = require('mini-css-extract-plugin');
  module.exports = {
     module: {
     rules: [
          {
              test: /\.css$/,
              use: [
                  MiniCssExtractPlugin.loader,
                  'css-loader'
              	]
          	}
      	]
   	}
    plugins: [
      new MiniCssExtractPlugin({
        filename: '[name].css',
      }),
    ]
  }
  ```

  

- `webpack`创建本地服务器

  不会生成dist目录，因为是直接打包到内存

  这时就能使用`npm run dev`启动服务，这时就可以自动刷新

  ```js
  npm install webpack-dev-server --save-dev
  ```

  ```js
  "script":{
      "build": "webpack --mode --production --config scripts/webpack.config.js",
      "dev": "webpack-dev-server --mode --development --config scripts/webpack.config.js",    
  }
  ```

  在`webpack.config.js`中配置devServer，可以手动配置端口号，设置为刷新以后自动打开

  ```js
  devServer:{
      port: 3000,
      open: true
  }
  ```

  

- less-loader

  index.less

  ```js
  npm install less less-loader --save-dev
  ```

  ```js
  module.exports = {
     module: {
     rules: [
          {
              test: /\.css$/,
              use: [
                  MiniCssExtractPlugin.loader,
                  'css-loader'
              	]
          },{
              test: /\.less$/,
              use: [
                  MiniCssExtractPlugin.loader,
                  'css-loader',
                  'less-loader'
                  //第二种写法，可以写成对象，这样可以传递参数
                  //{
                  //	loader: 'less-loader',
                  //	options: {
                  
                 //	}
                 // }
              	]
          }
      ]
   }
  }
  ```

  
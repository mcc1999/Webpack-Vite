## 一、Webpack

### 1. Webpack 是什么？

`webpack` 是一个静态打包工具，将模块化开发的代码根据入口文件（一个或多个）的依赖打包成一个或多个`bundle`文件，打包后的`bundle`文件在 `HTML` 中通过 `script 标签`引入即可直接在浏览器运行。

- 注：’静态‘一词的理解（源于文心一言）

  Webpack 被定义为“静态模块打包器”（Static Module Bundler），这里的“静态”一词可以从多个角度来理解。

  1. **静态文件处理**：

    - Webpack 主要处理的是静态资源文件，如 JavaScript、CSS、图片等。这些文件在构建时（build time）是已知的，即它们是静态的，而不是动态生成的或运行时（runtime）才确定的。

    - Webpack 通过分析这些静态文件之间的依赖关系，将它们打包成一个或多个文件，以优化加载速度和性能。

  2. **静态依赖分析**：

    - Webpack 在构建过程中，会静态地分析每个模块的依赖关系。这意味着 Webpack 会在编译时读取和解析模块代码，以确定哪些文件需要被打包在一起。

    - 这种静态分析使得 Webpack 能够进行各种优化，如代码拆分（Code Splitting）、树摇（Tree Shaking）等，以减少最终打包文件的大小和提高加载性能。

  3. **静态配置**：

    - Webpack 的配置是静态的，即在构建开始前就已经确定。Webpack 的配置文件（通常是 `webpack.config.js`）在构建时会被读取和执行，以生成最终的打包文件。

    - 这种静态配置使得开发者能够在构建前对打包过程进行精细控制，包括入口文件、输出路径、加载器（Loaders）、插件（Plugins）等。

  4. **不依赖于运行时信息**：

    - 与某些动态打包工具或系统不同，Webpack 不依赖于运行时信息来打包模块。所有的打包和优化工作都是在构建时完成的，而不是在运行时动态进行。

    - 这使得 Webpack 的打包过程更加可靠和可预测，因为所有的依赖关系和优化策略在构建时就已经确定。



### 2. Webpack 的作用：解决'开发'与'生产'的矛盾

|开发|生产|
|-|-|
| 需要模块化，便于开发|浏览器自身无法解析模块化(es6 模块语法出来之前)|
|会使用一些新语法和框架特殊写法(ts、es6 等)|浏览器只认识 js，有的老浏览器对新语法如 es6 等支持不全|



### 3. Webpack 的功能有哪些？

- 核心功能：打包文件；

- 其他功能——通过 `loaders` 和 `plugins` 来实现：

  - 编译浏览器无法理解的东西——`es6、ts、vue` 等语法；

  - 替代人工执行一些原本需手动的操作——文件合并/拆分、图片压缩、资源处理等；

  - 帮助开发——开发模式 `dev server`；



### 4. Webpack 的基础配置

#### entry：必需项，指定入口文件

```JavaScript
// 单入口文件
{
  entry: 'app.js'
  //  数组模式
  // entry: ['app.js']
  //  对象形式
  /* entry: {
    app: 'app.js'
  } */
}

//  多入口文件
{
  // 数组模式, 指定的多个文件及其依赖会被打包到一个 bundle 文件中
  entry: ['app1.js', 'app2.js']
  //  对象形式，每个文件及其依赖会有自己打包后的 bundle 文件
  /* entry: {
    app1: 'app1.js'，
    app2: 'app2.js'
  } */
}
```




#### output：必需项，指定产出配置

```JavaScript
{
  output: {
    // 需要绝对路径
    path: __dirname + '/dist',
    // - name 是 entry 对象的 key 值
    // - hash 是随机生成的，:6 可截取 4 为 hash 值，文件内容不变 hash 值不会改变
    filename: '[name].[hash:6].bundle.js',
  }
}
```




#### mode：非必需项，webpack4 后必填，

- 可选值为：`'none' | 'development' | 'production'(default)`；

- 配置mode 后会启用相应环境的 `build-in optimizations`；



#### devServer：非必需项，开发模式配置



#### module：非必需项，loaders 配置

```JavaScript
{
  module: {
    rules: [
      // 每一个对象都是一个 loader
      // test 指定 loader 处理的文件类型
      // loader 指定使用的 loader
      {
        test: /\.js/,
        loader: 'babel-loader'
      },
      {
        test: /\.js/,
        /* - 当时某类文件需要多个 loader 处理，用 use 参数替代 loader 参数
         * - loader 应用顺序从右到左
         */
        use: ['loader3', 'loader2', 'loader1']
      },
      {
        test: /\.js/,
        /* - loader 配置需要参数时，用 use 参数替代 loader 参数
         * - 配置参数：use.options 参数
         */
        use: {
          loader: 'babel-loader',
          options: {}
        }
      },
    ]
  }
}
```




#### plugin：非必需项，plugins 配置

```JavaScript
{
  
  plugin: [
    new xxx1Plugin(), 
    new xxx2Plugin(),
  ]
}
```




#### optimization：非必需项，优化相关——代码压缩、分割等



#### resolve：非必需项，配置如何解析模块——路径解析



### 5. 如何用 webpack —— 打包命令和版本问题

- 全局安装 `webpack` 和 `webpacl-cli` ，使用的是全局安装的 webpack 版本

```Shell
// 1. 默认使用项目目录下配置文件的 webpack.config.js
webpack 

// 2. 指定配置文件
webpack --config xxx.xxx.js
```


- 项目内安装 `webpack`， 使用的是项目安装的 webpack 版本

```JSON
// - devDependencies 安装 webpack
// - package.json 编写脚本
{
  "script": "webpack", //  或 "webpack --config xxx.js"
}
```




### 6. Webpack 处理 JS

- `es6`转化—— `babel-loader`

- 代码规范—— `eslint`

- ...



#### （1）`es6`转化—— `babel-loader`

```JavaScript
/*
 * 1. 安装 babel-loader、@babel/core 依赖；
 * 2. webpack.config.js 中使用 babel-loader
   3. 编写 babel 配置，不编写配置即使配置了 babel-loader 也不会生效
 *    babel 相关参数可写在 use.options 里或者写在项目最外层目录下的 .babelrc  的 JSON 文件中；
 * 4. babel 相关配置使用预设的 @babel/preset-env，安装 @babel/preset-env 依赖
 */ 
module.exports = {
  // ...
  module: {
    rules: [
      {
        test: /\.js$/,
        use: {
          loader: 'babel-loader',
          //  配置写在.babelrc 文件了，也可写在 options 中，两者内容完全一样
          // options: {}
        }
      }
    ]
  }
}
```


```JSON
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "targets": {
          "browsers": [
            ">1%", // 所有占有率大于 1%的浏览器
            "last 2 versions", // 浏览器的最新两个版本
            "not ie<=8" // ie8 以上
          ]
        }
      }
    ]
  ]
}

```


  

#### （2）代码规范—— `eslint`

  ```JavaScript
/*
 * 1. 安装 eslint、eslint-webpack-plugin 依赖；
 * 2. webpack.config.js 中使用 eslint-webpack-plugin
 * 3. 编写 eslint 规范配置
 *    eslint 相关参数可写在 new eslintPlugin({}) 对象里或者写在项目最外层目录下的 .eslintrc.js 的 JSON 文件中；
 * 4. eslint 相关配置使用 第三方规范 eslint-config-standard 或 eslint-config-airbnb，安装依赖
 */ 
const eslintPlugin = require("eslint-webpack-plugin");
module.exports = {
  // ...
  plugins: [new eslintPlugin()],
}
```


  ```JavaScript
module.exports = {
  env: {
    // node: true,
    browser: true,
    es2021: true,
  },
  // 继承第三方规范集合，下面 rules 里规范会覆盖继承的规范集合中相应的规范
  // 第三方规范集合:
  //    eslint-config-standard
  //    eslint-config-airbnb
  extends: [
    "standard",
    // "plugin:vue/strongly-recommended"
  ],
  // 提供一些 eslint 本身没有的规范，比如 vue 模板语法相关规范
  // plugins只提供规范，并不应用，需要在 extends 中再继承一下
  // 例如：eslint-plugin-vue
  plugins: [
    // "vue"
  ],
  parserOptions: {
    ecmaVersion: 6,
    sourceType: "module",
    ecmaFeature: {
      // 有 jsx 文件需配置
      jsx: true,
    },
  },
  rules: {
    // 覆盖 eslint-config-standard 中 semi: ["error", "never"] 的规范
    semi: ["error", "always"],
  },
};

```


  

### 7. Webpack 处理 css 和静态资源

- CSS

  1.  使用`css-loader` ，让 webpack 认识 css；

  2. 将 css 引入 HTML：

    1. `style-loader`(把 css 通过 `style 标签`插入 `HTML`) 

    2.  `mini-css-extract-plugin`(将 css 提取为单独文件)

  3. less/sass 等预处理 css 语言：通过相应的 loader 转换成普通 css，再运用上述 1、2 逻辑；

  4. 压缩 css：`css-minimizer-webpack-plugin`;

- 静态资源（img、audio/video、...）—— webpack5 自带对资源文件的支持

  1. `file-loader`/`url-loader`：

    1. `file-loader`只能让 webpack 识别资源文件；

    2. `url-loader`基于`file-loader`，在让 webpack 识别资源文件基础上，能做一些优化；

  2. 优化操作

    1. 使用带 `hash` 的文件名来使用浏览器缓存

    2. 小图片转 `base64`

#### （1）处理 CSS 并插入到 HTML

```JavaScript
// mini-css-extract-plugin 除了要在 plugins 注册，也要再 module 中使用相应 loader
const miniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  // ...
  module: {
    rules:[
      {
        test: /\.css$/,
        use: [miniCssExtractPlugin.loader, "css-loader"],
        // use: ["style-loader", "css-loader"],
      },
    ],
  },
  plugins: [
    new miniCssExtractPlugin({ filename: "test.bundle.css" }),
  ],
};

```


#### （2）压缩 CSS bundle 文件

```JavaScript
const miniCssExtractPlugin = require("mini-css-extract-plugin");
const cssMinimizerPlugin = require("css-minimizer-webpack-plugin");

module.exports = {
  // ...
  module: {
    rules:[
      {
        test: /\.css$/,
        use: [miniCssExtractPlugin.loader, "css-loader"],
      },
    ],
  },
  plugins: [
    new miniCssExtractPlugin({ filename: "test.bundle.css" }),
    new cssMinimizerPlugin(),
  ],
};

```


#### （3）处理 LESS 等预处理 CSS 语言并插入到 HTML

```JavaScript
/*
 * 1. 安装 less、less-loader 依赖
 * 2. 配置 less 文件处理 loader
 */
const miniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  // ...
  module: {
    rules:[
      {
        test: /\.css$/,
        use: [miniCssExtractPlugin.loader, "css-loader", "less-loader"],
      },
    ],
  },
  plugins: [
    new miniCssExtractPlugin({ filename: "test.bundle.css" }),
  ],
};

```


#### （4）file-loader/url-loader 处理资源文件—— webpack3、4

```JavaScript
/*
 * 1. 安装 file-loader、url-loader 依赖
 * 2. 配置资源文件处理 loader
 * 3. limit 参数是的小于等于[limit] 的图片会被转换为 base64插入 js 文件中；
 *    limit 值不宜过大，过大会导致 js 文件过大，下载过慢
 *    limit 值也不宜过小，过小每张图片都会用新的请求获取，也会增加耗时
 */

module.exports = {
  // ...
  module: {
    rules:[
      {
        test: /\.(jpeg|jpg|png|gif|svg)$/,
        use: {
          loader: "url-loader",
          options: {
            limit: 5000,
            name: "[name].[hash:4][ext]",
          },
        },
      },
    ],
  },
};

```


#### （5）webpack 自带资源文件支持—— webpack5

```JavaScript
/*
 * 1. type:  可选值有"asset"、 "asset/inline"、 "asset/resource"
 *    asset：视资源大小而定
 *    asset/inline：无论大小都转换为 base64
 *    asset/resource：无论大小都都打包成单独文件
 */

module.exports = {
  // ...
  module: {
    rules:[
      {
        test: /\.(jpeg|jpg|png|gif|svg)$/,
        type: "asset", // "asset"、 "asset/inline"、 "asset/resource"
        parser: {
          dataUrlCondition: {
            maxSize: 5000,
          },
        },
        generator: {
          filename: "[name].[hash:4][ext]",
        },
      },
    ],
  },
};

```


### 8. Webpack 处理 HTML

- `html-webpack-plugin`作用：创建 HTML 文件、自动引入打包后的 bundle.js 等文件

- `html-webpack-plugin`需提供：

  - `html模板`

  - 打包后`filename`

- `bundle.js` 插入位置：`inject` 参数—— `head`、`body/true`；

- HTML 文件压缩：`minify` 参数

- 多入口文件、多 HTML—— 实例化多个 `html-webpack-plugin`，提供 `chunks`；

- 动态修改 HTML 内容：`html-webpack-plugin`传入参数，HTML 内使用字符串模版；

#### （1）webpack 处理一般 HTML

```JavaScript
const htmlPlugin = require("html-webpack-plugin");

module.exports = {
  // ...
  plugins: [
    new htmlPlugin({
      template: "./index.html",
      filename: "index.html",
      minify: {
        collapseWhitespace: false,
        removeComments: false,
        removeAttributeQuotes: false,
      },
      inject: "body",
    }),
  ]
};

```


#### （1）webpack 处理多入口文件、多 HTML

```JavaScript
const htmlPlugin = require("html-webpack-plugin");

module.exports = {
  // ...
  entry: {
    app1: 'index1.js',
    app2: 'index2.js'
  }
  plugins: [
    new htmlPlugin({
      template: "./index.html",
      filename: "index1.html",
      minify: {
        collapseWhitespace: false,
        removeComments: false,
        removeAttributeQuotes: false,
      },
      inject: "body",
      chunks: ["app2"],
    }),
    new htmlPlugin({
      template: "./index.html",
      filename: "index2.html",
      minify: {
        collapseWhitespace: false,
        removeComments: false,
        removeAttributeQuotes: false,
      },
      inject: "body",
      chunks: ["app2"],
    }),
  ]
};

```


#### （3）webpack 动态修改 HTML 内容

```JavaScript
const htmlPlugin = require("html-webpack-plugin");

module.exports = {
  // ...
  entry: {
    app: 'index.js'
  }
  plugins: [
    new htmlPlugin({
      template: "./index.html",
      filename: "index.html",
      minify: {
        collapseWhitespace: false,
        removeComments: false,
        removeAttributeQuotes: false,
      },
      inject: "body",
      title: "dynamic title",
    }),
  ]
};

```


```HTML
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <!-- const htmlPlugin = require("html-webpack-plugin") -->
    <!-- 无论引入时命名的是htmlPlugin或其他，字符串模版中一定是htmlWebpackPlugin -->
    <title><%= htmlWebpackPlugin.options.title%></title>
  </head>
  <body></body>
</html>

```


### 9. Webpack 代码分割

- 单入口单文件分割： 动态引入文件—— `import()`

- 多入口文件分割：单独打包共同依赖的文件——`optimization.splitChunks`

- 第三方库单独打包`optimization.splitChunks.cacheGroups`

#### （1）单入口单文件分割

```JavaScript
import "./index.css";
import "./index.less";
import fileImg from "./imgs/file.png";

const img = new Image();
img.src = fileImg;
console.log(a + b + 1);
document.body.appendChild(img);
//  只有又大又后面用到的依赖文件才动态引入
setTimeout(() => {
  import('./a.js').then(res=>{
    console.log(res.default);
  })
}, 3000)

```


#### （2）多入口文件分割——将所有共同依赖文件打包成一个文件

```JavaScript
module.exports = {
  optimization: {
    splitChunks: {
    // all、async(只分割按需加载：import())、initial(只分割同步依赖：import 'xxx'，按需加载也分割)
      chunks: 'all', 
      minChunks: 2, // 依赖重复次数
      minSize: 0, // 单位bytes  单独打包依赖的文件大小最小值
      filename: 'same-dep.[chunkhash].js'
    }
  }
}
```


#### （3）第三方库单独打包

```JavaScript
module.exports = {
  optimization: {
    splitChunks: {
      chunks: "all",
      cacheGroups: {
        vendors: {
          test: /[\\/]node_modules[\\/]/,
          name: "vendors",
          chunks: "all",
        },
        // lodash 单独打包
        lodashGroup: {
          test: /lodash/,
          name: "lodash",
          chunks: "all",
          // node_module中也有 lodash，设置一个更高的优先级（自定义 cacheGroup 优先级默认为0）来单独打包
          priority: 1,
        },
        common: {
          name: "common",
          chunks: "all",
          minChunks: 2,
          minSize: 0,
        },
      },
    },
    runtimeChunk: { name: "runtime" },
  }
}
```


  

### 10. Webpack devServer

- 原理：

  ![image.png](https://tc-cdn.flowus.cn/oss/663e4892-f9ec-4a04-9aae-1ec0ef966a9c/image.png?time=1734173100&token=56dc028a50faaed0c7cd07a5934ee64a4018d7dca6f6a05620d9947b4c095602&role=free)

- proxy 代理转发

```JavaScript
module.exports = {
  // ...
  devServer: {
    port: 3000,
    proxy: [
      // {
      //   // fetch('/api/xxx') 会发送到 http://host:port/api/xxx
      //   context: ["/api"],
      //   target: "http://localhost:9000",
      // },
      {
        // fetch('/user/xxx') 会发送到 http://host:port/api/v1/user
        context: ["/"],
        target: "http://localhost:9000",
        pathRewrite: { "^/user": "/api/v1/user" },
      },
    ],
  },
}
```


- sourceMap

```JavaScript
module.exports = {
  // ...
  devtool: 'eval-cheap-source-map',
}
```


### 11. Webpack 中获取当前环境

1. 安装 `cross-env` 依赖；

2. 在 `build` 或 `dev` 命令中 webpack 之前添加环境变量： `cross-env NODE_ENV=production/developmement`；

3. 在 `webpack.config.js` 中通过 `process.env.NODE_ENV` 来获取设置的环境变量；

### 11. Webpack 分析打包结果

#### 方法一：--json>stats.json + [https://webpack.github.io/analyse](https://webpack.github.io/analyse)

- webpack 打包命令添加参数`--json>stats.json`，获得一个 `stats.json` 文件；

- 将`stats.json`文件上传到[https://webpack.github.io/analyse](https://webpack.github.io/analyse)，查看打包结果分析；



#### 方法二：webpack-bundle-analyzer

```JavaScript
//  运行打包命令后，会自动打开一个打包文件分析的网站
const bundleAnalyzer = require("webpack-bundle-analyzer").BundleAnalyzerPlugin;
module.exports = {
  // ...
  plugins: [new bundleAnalyzer()]
}
```




### 12. Webpack 提前打包 —— Dll

4. 配置 `webpack.dll.config.js` 将第三方库等不变的库提前打包；

5. 运行 `webpack --config webpack.dll.config.js` 打包不变的文件；

6. 在 index.html 模版中将提前打包好的 `dll` 文件手动引入；

```JavaScript
const webpack = require("webpack");

module.exports = {
  mode: "production",
  entry: {
    vendor: ["lodash", "dayjs"],
  },
  output: {
    path: __dirname + "/dist",
    filename: "[name].dll.js",
    library: "[name]_library",
  },
  plugins: [
    // 通过这个插件输出一个 manifest.json 通知正式打包，哪些文件已提前打包
    new webpack.DllPlugin({
      path: __dirname + "/[name]-manifest.json",
      // name必须和 output 里的 library 保持一致
      name: "[name]_library",
      context: __dirname,
    }),
  ],
};
```


## 二、Vite

### 1. rollup 打包特点

- 不会生成过多的运行代码，`webpack` 中的 `runtime.js`;

- 多模块化规范（esm，cjs，amd...）打包；

- 打包命令：`rollup --config`，有`--config`才会在项目最外层目录查找`rollup.config.js`并使用；

```JavaScript
// 引入第三方库
const resolve = require("@rollup/plugin-node-resolve");
// 压缩代码
const terser = require("@rollup/plugin-terser");

module.exports = {
  input: "index.js",
  output: {
    file: "./dist/bundle.js",
    format: "es", // 打包模块化规范
  },
  // 不打包的模块
  external: [],
  plugins: [resolve(), terser()],
};

```


### 2. Vite 使用

#### 1. 使用流程

- 安装 `Vite`；

-  指定一个 `html` 文件为入口；

- 打包构建：`vite build` / 开发：`vite`；

#### 2. 资源处理

- 天生支持 css 及预处理 css 语言；

- 支持 typescript ；

- 能自己处理各种资源；

#### 3. 代码分割—— rollupOptions.manualChunks

```JavaScript
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import legacy from "@vitejs/plugin-legacy";

export default defineConfig({
  server: {
    port: 3000,
  },
  // build 内配置只对打包 build 有效
  build: {
    // 图片优化：转 base64
    assetsInlineLimit: 5000,
    rollupOptions: {
      output: {
        entryFileNames: "bundle.[hash:6].js",
        chunkFileNames: "[name].[hash:6].js",
        assetFileNames: "[name].[hash:6][extname]",
        // 代码分割
        manualChunks: {
          react: ["react", "react-dom"],
        },
        // manualChunks: (id) => {
        //   if (id.includes("node_modules/lodash-es")) return "lodash-es";
        //   else if (id.includes("node_module")) return "vendor";
        // },
      },
    },
  },
  resolve: {
    extensions: [".js", ".json", ".ts", ".jsx"],
    alias: {
      "@imgs": "./imgs",
    },
  },
  plugins: [
    react(),
    // 传统浏览器支持
    // legacy({ targets: ["defaults", "not IE 11"] })
  ],
});

```



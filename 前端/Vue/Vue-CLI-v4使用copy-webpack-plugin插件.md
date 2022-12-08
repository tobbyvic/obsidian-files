## build后将assets或static某个文件夹拷贝到dist目录中，使用copy-webpack-plugin插件

### 首先安装copy-webpack-plugin

```
npm install --save-dev copy-webpack-plugin
```

### 然后在vue.config.js中引入

```
configureWebpack: {
    plugins: [
      // 注意，这里的插件得是6.3.2版本的，最新的v11不支持vue-cli-service
      new CopyWebpackPlugin({
        patterns: [
          {
            from: path.resolve(__dirname, "public/install"), //我复制的是src目录下面的assets文件夹，这里面有img,font,json
            to: path.resolve(__dirname, "dist/install"), //这是复制到打包后的dist文件夹下的static目录下
          },
        ],
      }),
    ],
  },

```

### 发现报错！！

```
 ERROR  TypeError: compilation.getCache is not a function
TypeError: compilation.getCache is not a function
    at D:\16-test\test-copy-webpack\node_modules\copy-webpack-plugin\dist\index.js:693:33
    at SyncHook.eval [as call] (eval at create (D:\16-test\test-copy-webpack\node_modules\tapable\lib\HookCodeFactory.js:19:10), <anonymous>:9:1)
    at SyncHook.lazyCompileHook (D:\16-test\test-copy-webpack\node_modules\tapable\lib\Hook.js:154:20)
    at Compiler.newCompilation (D:\16-test\test-copy-webpack\node_modules\webpack\lib\Compiler.js:630:30)
```

### 解决方案

因为这里npm install --save-dev copy-webpack-plugin的时候，默认安装的是v11.0.0版本。此类新版本不兼容，所以要进行回退下，实测v6.3.2版本很好的兼容vue-cli-service。

更改package.json

![](/Pasted%20image%2020221208181914.png)

### 重新打包，成功。

```
npm run build
```

PS：图中install文件夹就是最上面配置的copy-webpack-plugin里面拷贝过去的。

![](/Pasted%20image%2020221208181929.png)

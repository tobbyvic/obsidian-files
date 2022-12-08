本文将展示如何从0开始初始化一个`typescript`项目。

点击访问[git仓库](https://segmentfault.com/a/1190000041781242), 点击下载[代码包](https://link.segmentfault.com/?enc=7j46NWwRNrDA52Yej69wbw%3D%3D.7KDR2z66F7EJ20TBPDalZHb081hrz3jxvpC%2FhFpMjLUUM4izL9hljtevtpjTU26u4nKoQx6WtYvb8dJ41GcF8pAfnAR59uVYaDJ3SyqqGjs%3D)

## 初始化

首先，我们选定一个文件夹，然后在文件夹中执行`npm init -y`命令来对项目进行初始化。

```js
anjie@panjies-Mac-Pro typescript-init % npm init -y
Wrote to /Users/panjie/github/yunzhiclub/typescript-init/package.json:

{
  "name": "typescript-init",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/yunzhiclub/typescript-init.git"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/yunzhiclub/typescript-init/issues"
  },
  "homepage": "https://github.com/yunzhiclub/typescript-init#readme"
}
```

该命令将为我们生成一个`package.json`文件：

```json
panjie@panjies-Mac-Pro typescript-init % tree
.
├── README.md
└── package.json

0 directories, 2 files
```

## 安装typescript

接下来，我们使用`npm i typescript --save-dev`来安装`ts`：

```shell
panjie@panjies-Mac-Pro typescript-init % npm i typescript --save-dev
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN typescript-init@1.0.0 No description

+ typescript@4.6.4
added 1 package from 1 contributor and audited 1 package in 2.22s
found 0 vulnerabilities
```

## typescript初始化

ts安装后，我们也需要一个初始化操作，该操作将默认生成ts的配置文件，对应的命令为`npx tsc --init`

```js
panjie@panjies-Mac-Pro typescript-init % npx tsc --init

Created a new tsconfig.json with:                                               
                                                                             TS 
  target: es2016
  module: commonjs
  strict: true
  esModuleInterop: true
  skipLibCheck: true
  forceConsistentCasingInFileNames: true

You can learn more at https://aka.ms/tsconfig.json
```


该命令将为我们自动生成`tsconfig.json`，如果你想进行一些定制，则只需要打开此文件，找到对应的项进行修改或启用即可。

## index.ts

基本的初始化工作完成后，便可以创建`index.ts`，并进行代码的测试了。

```ts
'use strict';

const hello = (world: string) => {
  console.log(`hello ${world}`);
}
```

## 编译运行

文件创建完成后进行编译及运行:

```
panjie@panjies-Mac-Pro typescript-init % npx tsc index.ts
panjie@panjies-Mac-Pro typescript-init % node index.js
hello world
```



## 自动编译运行

每次变更文件化都手动执行一下编译及运行固然可行，但这种方式的确无法忍受。[tsc-watch](https://link.segmentfault.com/?enc=5hE3jrq3dDERwXitQNXPMw%3D%3D.42aG%2Bw%2B5HYtubD12YxVKHKI2zOH9ZCpHagWh4%2B4j4Z3eQkUUBfHJ1ZtgctNoBbyq)则专门为此而生，运行`npm install tsc-watch --save-dev`来安装`tsc-watch`：

```
panjie@panjies-Mac-Pro typescript-init % npm install tsc-watch --save-dev
npm WARN typescript-init@1.0.0 No description

+ tsc-watch@5.0.3
added 20 packages from 16 contributors and audited 21 packages in 3.788s
found 0 vulnerabilities
```

安装完成后，我们打开`package.json`文件，在`scripts`中增加以下`dev`项：

```js
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "tsc-watch --noClear -p ./tsconfig.json --onSuccess \"node ./index.js\""
  },
```

然后我们在命令行中运行`npm run dev`则可以实现：当文件变更时重新编译、重新运行的目的。

## 直接运行ts

如果我们感觉还不够爽，还可以使用`nodemon`来直接运行`ts`文件：

```
npm install nodemon --save-dev
npm install --save-dev ts-node
npm install --save-dev tslib  
npm install --save-dev @types/node
```

安装完成后则可以使用`npx nodemon index.ts`来直接运行`ts`文件了。

最终的示例package.json如下：

```json
{
  "name": "mockapi",
  "version": "1.0.0",
  "description": "mock api",
  "repository": "",
  "main": "index.js",
  "engines": {
    "node": "14.16.1"
  },
  "scripts": {
    "build": "npx tsc",
    "run": "node dist/index.js",
    "dev": "concurrently \"npx tsc --watch\" \"nodemon -q dist/index.js\"",
    "start": "npx nodemon index.ts",
    "test": "npx nodemon test.ts"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
  },
  "devDependencies": {
    "@types/node": "^18.0.0",
    "concurrently": "^7.2.2",
    "nodemon": "^2.0.16",
    "ts-node": "^10.8.1",
    "tslib": "^2.4.0",
    "typescript": "^4.7.4"
  }
}
```



## 参考文档

-   [https://www.digitalocean.com/community/tutorials/typescript-new-project](https://link.segmentfault.com/?enc=J7kx9RelTYY5aRUWkrvjyA%3D%3D.NVV4l1IuI7BhOD6F%2BdGy%2BUXTRu4zvPqQ2T3efrPNY%2BUs1i8zpOuJc221qmQXyiCm9ayZUWnA%2BSHlUtByvru%2BShl8obSDYVhqLyLTq1LDDxE%3D)
-   [https://www.npmjs.com/package/tsc-watch](https://link.segmentfault.com/?enc=HIYM1abUyiUTrMwfVrfDIg%3D%3D.Qbh4mJu9s89BMZqbDbM5Gy2vMkAFagUGx84INTJBX1gEMbzD5evjeTtiC3blfymT)
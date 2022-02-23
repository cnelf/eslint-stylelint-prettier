# eslint-stylelint-prettier

#### ESLint 配置项

```javascript
// .eslintrc.js
module.exports = {
  env: {},
  globals: {},
  parser: 'espree',
  parserOptions: {},
  plugins: [],
  extends: [],
  rules: {},
  overrides: {}
};
```

##### env (Environments)

指定脚本的运行环境。每种环境都有一组特定的预定义全局变量。例如：配置 `env: { node: true }`，就可以正确读取 node 环境下的 `global`，`process` 等全局变量，否则 eslint 会提示变量未定义。

也可以通过注释来指定环境：

```javascript
/* eslint-env node, mocha */
```

常用的环境变量

- `browser` - 浏览器环境中的全局变量
- `node` - Node.js 全局变量和 Node.js 作用域
- `es6, es2020, es2021等` - 启用 ES6+的新特性
- `jquery` - jQuery 全局变量

<br/>

##### globals (全局变量)

配置全局变量，key 值为对应的变量名称。值设置为 `writable` 表示允许重写变量，`readonly` 表示不允许重写变量。

```javascript
{
  globals: {
    var1: 'writable',
    var2: 'readonly'
  }
}
```

也可以通过注释来指定全局变量：

```javascript
/* global var1:writable, var2:readonly */
```

<br/>

##### parser (解析器)

ESLint 默认使用 Espree 作为其解析器，你也可以指定一个不同的解析器。解析器的作用是将源代码解析成 AST (抽象语法树)，可以更方便地进行代码规则校验以及代码转换。

<br/>

##### parserOptions（解析器配置）

默认情况下，ESLint 支持 ECMAScript 5 语法。你可以覆盖该设置，以启用对 ECMAScript 其它版本和 JSX 的支持。

可用的选项有：

- `ecmaVersion` - 指定使用的 ECMAScript 版本
- `sourceType` - 默认值为 `script`，如果代码是 ECMAScript 模块，可以指定为 `module`
- `ecmaFeatures` 额外的语言特性：
  - `globalReturn` - 允许在全局作用域下使用 `return` 语句
  - `impliedStrict` - 启用全局 strict mode (如果 ecmaVersion 是 5 或更高)
  - `jsx` - 启用 JSX

<br/>

##### plugins (插件)

默认情况下，ESLint 中的规则只会对 JS 进行校验，如果我们想对 Vue 进行校验的话，就需要增加 `eslint-plugin-vue` 插件。插件一般通过 npm 安装，在配置插件时，可以使用 `plugins` 关键字来存放插件的列表，插件名称可以省略 `eslint-plugin-` 前缀。

```javascript
{
  plugins: ['vue'];
}
```

<br/>

##### extends (扩展)

ESLint 中的规则很多，但是默认都不会开启的，我们需要在 rules 中设定这些规则开关，这个环节非常繁琐。因此 ESLint 设计了 extends 这个字段，用于继承预先定义好的校验规则，一般通过 npm 安装。例如：`eslint-config-prettier`，扩展名称可以省略 `eslint-config-` 前缀。

```javascript
{
  extends: ['eslint:recommended', 'prettier']
}
```

内置的扩展：

- `eslint:recommended` - eslint 推荐的扩展内容，报告了一些常见的问题，具体内容可以查看在 [规则页面](http://eslint.cn/docs/rules) 中被标记为 √ 的规则。
- `eslint:all` - 启用当前安装的 eslint 中所有的核心规则，不推荐使用。

<br/>

##### rules (规则)

ESLint 附带大量的校验规则，要改变一个规则设置，必须将规则 ID 设置为下列值之一：

- `'off'` 或 `0` - 关闭规则
- `'warn'` 或 `1` - 开启规则，使用警告级别的错误（不会导致程序退出）
- `'error'` 或 `2` - 开启规则，使用错误级别的错误（当被触发时，程序会退出）

```javascript
{
  rules: {
    curly: 'error',
    quotes: ['error', 'single'],
    // 插件规则
    'plugin1/rule1': 'error'
  }
}
```

也可以通过注释来指定规则：

```javascript
/* eslint quotes: ["error", "double"], curly: "error" */
```

或通过块注释临时禁止规则出现警告：

```javascript
/* eslint-disable  */ 或; /* eslint-disable no-console */
console.log('foo');

/* eslint-disable-next-line */ 或; /* eslint-disable-next-line no-console */
console.log('foo');
```

<br/>

##### overrides (覆盖)

一般用于覆盖指定文件校验规则。

```javascript
{
  overrides: [
    {
      files: ['bin/*.js'],
      rules: {
        quotes: ['error', 'single']
      }
    }
  ];
}
```

#### 代码格式化与校验常用的第三方库

##### eslint

JavaScript 代码检测工具，根据.eslintrc.js 文件下配置的校验规则，对代码进行静态检查，打印出具体的错误内容。

<br/>

##### @babel/eslint-parser

用于替代 `babel-eslint`（已不再维护），作用是使 JS 能够使用一些还处于实验性质的语法，例如：装饰器等，也可以作为 Typescript 的 解析器。

<br/>

##### eslint-plugin-vue

Vue 官方提供的 eslint 插件，对 vue 文件制定了一系列的推荐的规范。

<br/>

##### prettier

常用的代码格式化工具，配合编辑器可以支持自定义规则，保存后自动格式化。

<br/>

##### eslint-config-prettier

关闭了 ESLint 中一些不必要的规则以及可能与 Prettier 冲突的规则 。

<br/>

##### eslint-plugin-prettier

以 ESLint 规则的方式运行 Prettier，通过 Prettier 找出格式化前后的差异，并以 ESLint 问题的方式报告差异，同时针对不同类型的差异提供不同的 ESLint fixer。

### husky 和 lint-staged

#### 什么是 githooks

Git Hooks 就是 Git 在执行特定事件（如 commit、push、receive 等）时触发运行的脚本，类似于”钩子函数“，没有设置可执行的钩子时将被忽略。

初始化本地 git 仓库后，会自动在项目目录下生成 `.git/hooks` 文件夹，该文件夹内有一些以 `.sample` 结尾的钩子示例脚本，如果想启用对应的钩子，只需要手动删除文件的 `.simple` 后缀即可。

但是，我们一般不手动去改动 `.git/hooks` 里的文件内容，因为 `.git` 文件并不会通过 git 上传到远程服务器。

<br/>

#### husky

husky 的作用就是会自动帮你在 `.git/hoods` 目录内生成对应的钩子文件和 shell 脚本，从而开启 githooks 的能力。

##### husky 的安装

```javascript
npm install -D husky@4
```

这里安装 v4 版本，新版本的 husky 的配置过程相对比较繁琐。

##### husky 的配置

你可以直接在 `package.json` 内写配置信息，也可以使用 `.huskyrc`、`.huskyrc.js` 或 `husky.config.js` 文件进行配置。

1. 在 `package.json` 内新增配置内容

```json
{
  "husky": {
    "hooks": {
      "pre-commit": "echo \"git commit trigger husky pre-commit hook\""
    }
  }
}
```

2. 在 `.huskyrc.js` 内进行配置

```javascript
module.exports = {
  hooks: {
    'pre-commit': 'echo "git commit trigger husky pre-commit hook"'
  }
};
```

以上配置后会在 git commit 执行前在控制台打印一段信息。

<br/>

#### lint-staged

在上传时，我们通常不需要对整个项目都执行 lint 检查，因为之前上传过的代码应该是符合 lint 规范的，只需要对当前修改的文件执行检查即可。lint-staged 的作用就是能够让 lint 只检查在暂存区（git add .）的文件。

##### lint-staged 的安装

```javascript
npm install -D lint-staged
```

##### lint-staged 的配置

```json
// package.json
{
  "lint-staged": {
    "src/**/*.{js,vue}": ["prettier --write .", "eslint --fix ."]
  }
}
```

这里 lint-staged 的配置是暂存区中的文件，只要是在 src 目录下的 js、vue 文件都执行 prettier 和 eslint 的命令。

配合 husky 提供的 pre-commit 钩子，就可以实现在 commit 提交前先进行代码格式化和检查，如果检查报错就自动拒绝 commit。

完整的配置：

```json
// package.json
{
  "script": {
    "prettier": "prettier --write .",
    "lint": "eslint --fix ."
  },
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "src/**/*": [
      "npm run prettier"
    ],
    "src/**/*.{js,vue}": [
      "npm run lint"
    ]
  },
  "devDependencies": {
    "eslint": "^8.9.0",
    "husky": "^4.3.8",
    "lint-staged": "^12.3.4",
    "prettier": "^2.5.1"
    ...
  }
}
```

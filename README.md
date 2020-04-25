# CodeLint

代码约束实现demo 

## 各部分实现介绍
#### git钩子
###### husky
1.安装
`yarn add husky --dev`
2.配置
```
// package.json
{
  "husky": {
    "hooks": {
      "pre-commit": "npm test",
      "pre-push": "npm test",
      "...": "..."
    }
  }
}
```
> 详见：[husky](https://www.npmjs.com/package/husky#hooks-arent-running)

###### precommit
1.安装
`yarn add pre-commit --dev`
2.配置
```
// package.json
{
  "name": "437464d0899504fb6b7b",
  "version": "0.0.0",
  "description": "ERROR: No README.md file found!",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: I SHOULD FAIL LOLOLOLOLOL \" && exit 1",
    "foo": "echo \"fooo\" && exit 0",
    "bar": "echo \"bar\" && exit 0"
  },
  "pre-commit": [
    "foo",
    "bar",
    "test"
  ]
}
```
> 详见：[pre-commit](https://www.npmjs.com/package/pre-commit)

![husky与pre-commit配置截图](https://upload-images.jianshu.io/upload_images/1490138-62b259449005546c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 自动化任务管理
###### lint-staged
对于git 暂存区文件执行设置好的命令
1.安装
`yarn add lint-staged --dev`
2.配置
```
{
  "lint-staged": {
    "*": "your-cmd"
  }
}
```
![lint-staged配置](https://upload-images.jianshu.io/upload_images/1490138-bcc651ce286c5060.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
> lint-staged配置可以在package.json中，也在在配置文件.lintstagedrc中。
详见：[lint-staged](https://www.npmjs.com/package/lint-staged)

#### js代码校验
###### eslint
按照配置的规则对js代码进行检查的工具

1.安装
```
yarn add eslint --dev
yarn add eslint-plugin-react  --dev // react项目要装的eslint插件
yarn add eslint-loader --dev  // 可选webpack loader，使用后如果代码不符合eslint规范编译会报错（已有项目不建议使用，会满屏的报错）。
```
2.配置
```
// 项目根目录下 .eslintrc.js
module.exports = {
    "env": {
        "browser": true,
        "es6": true
    },
    "extends": [
        "eslint:recommended",
        "plugin:react/recommended"
    ],
    "globals": {
        "Atomics": "readonly",
        "SharedArrayBuffer": "readonly"
    },
    "parserOptions": {
        "ecmaFeatures": {
            "jsx": true
        },
        "ecmaVersion": 2018,
        "sourceType": "module"
    },
    "plugins": [
        "react"
    ],
    "rules": {
        "eqeqeq": 2,//必须使用全等
        "init-declarations": 1,//声明时必须赋初值
        "prefer-const": 1,//首选const
    }
};

```
> 项目根目录下`eslint --init`可根据提示生成eslint配置文件
也可使用别人配置好的`.eslintrc`文件，但要注意该配置是否使用了继承和插件，如有需安装相关依赖


#### css样式校验
###### stylelint
1.安装
```
yarn add stylelint --dev
yarn add stylelint-config-standard --dev  //官方提供的样式规范配置
yarn add stylelint-webpack-plugin --dev  // 可选webpack plugin，使用后如果代码不符合stylelint规范编译会报错（已有项目不建议使用，会满屏的报错）。
```
2.配置
```
// 项目根目录下.stylelintrc.json
{
  "extends": "stylelint-config-standard"
}

```

#### 代码风格格式化
###### prettier
格式化代码工具。用来保持团队的项目风格统一

1.安装
```
yarn add prettier --dev --exact   //安装prettier
yarn add  eslint-plugin-prettier --dev  // eslint插件，将prettier的规则加入到eslint中，prettier的规则在eslint都是可以通过 `eslint --fix`修复的
```
2.配置
```
// 项目根目录下.prettierrc.js
module.exports = {
    tabWidth: 2,
    // 更多规则，详见prettier文档 options
};


```

> `eslint-config-prettier` 关闭`eslint ` 中与`prettier ` 冲突的选项，只会关闭冲突的选项，不会启用`prettier ` 的规则
eslint-plugin-prettier 启用 `prettier `  的规则

#### git commit规范校验
1.安装`commitlint `依赖与`@commitlint/config-conventional`官方扩展规则。
`npm install --save-dev @commitlint/{cli,config-conventional}`
2.commit 规则
git commit的消息这样组成：
其中header是必须有的，body，footer可选。
```
header
--空一行
body
--空一行
footer
```
3.header的规则
Commit message格式，注意冒号后面有空格
`<type>: <subject>`
type:
```
build：主要目的是修改项目构建系统(例如 glup，webpack，rollup 的配置等)的提交
ci：主要目的是修改项目继续集成流程(例如 Travis，Jenkins，GitLab CI，Circle等)的提交
docs：文档更新
feat：新增功能
merge：分支合并 Merge branch ? of ?
fix：bug 修复
perf：性能, 体验优化
refactor：重构代码(既没有新增功能，也没有修复 bug)
style：不影响程序逻辑的代码修改(修改空白字符，格式缩进，补全缺失的分号等，没有改变代码逻辑)
test：新增测试用例或是更新现有测试
revert：回滚某个更早之前的提交
chore：不属于以上类型的其他类型
```

subject:
subject是 commit 目的的简短描述，不能超过50个字符，且结尾不加英文句号

4.配置 package.json 文件
添加 husky 字段
```
"husky": {
    "hooks": {
      "commit-msg": "commitlint -e $HUSKY_GIT_PARAMS"
    }
  },
```

>说明：节省时间，commit规则使用官方提供的扩展，暂不自定义规则

> 跳过校验 `git commit --no-verify -m "代码规范强制提交测试"`

详见[@commitlint/config-conventional](https://www.npmjs.com/package/@commitlint/config-conventional)
[https://commitlint.js.org/](https://commitlint.js.org/)
[https://commitlint.js.org/#/reference-rules](https://commitlint.js.org/#/reference-rules)

##### Commitizen: 替代 git commit

1.安装依赖
`yarn add  commitizen --dev`
2.`git commit`操作替换为`git-cz`
3.导出changelog
`npm install -g conventional-changelog-cli`
`cd my-project`
`conventional-changelog -p angular -i CHANGELOG.md -s`

#### 多编辑器统一
editorConfig
```

// 项目根目录下.editorconfig
root = true  //项目里读editorcongig文件时，读到此文件即可，不用继续往上搜索

[*]  //规范指定:全部文件
charset = utf-8  //文档的字符编码：使用UTF-8 - Unicode 字符编码；常见的字符编码有两种：1.UTF-8 - Unicode 字符编码；2.ISO-8859-1 - 拉丁字母表的字符编码。

//使用Unix-style 换行符，并且每个文件以换行结束
end_of_line = lf //跟系统有关。win用cr lf，linux/unix用lf，mac用cr。
insert_final_newline = true   //保存文件时，在最后一行后加上一行空行


//2个空格缩进
indent_style = space //tab类型：空格（不使用制表符）
indent_size = 2  //tab键长度是两个空格


trim_trailing_whitespace = true  //去掉每行代码最后多余的空格

```


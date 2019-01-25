# git-cz-simple-example

[commitizen](https://github.com/commitizen/cz-cli) 和 [commitlint](https://github.com/marionebl/commitlint) 的简单示例。

> 参考：《[简单使用Commitizen-规范你的commit message](https://www.jianshu.com/p/36d970a2b4da)》，请先阅读此篇文章，里面已经说得很清楚了，这里我就不多做阐述。

## 使用 commitizen 来执行规范

[官方地址](https://github.com/commitizen/cz-cli)

1. 全局安装commitizennode模块

``` bash
npm install -g commitizen
```

2. 在项目目录下运行命令

``` bash
commitizen init cz-conventional-changelog --save --save-exact
```

3. 此时可能会报找不到package.json的错误,使用下面命令来自动生成一个项目的package,然后在运行2中的命令.

``` bash
npm init -y
```

运行完以上一律使用 `git cz` 代替 `git commit` 来提交代码,同时会显示一下选项来自动生成符合格式的 commit message.

命令行里输入 `git cz` ，会出现如下的选择：
```
feat:     A new feature
fix:      A bug fix
docs:     Documentation only changes
style:    Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)
refactor: A code change that neither fixes a bug nor adds a feature
perf:     A code change that improves performance
test:     Adding missing tests or correcting existing tests
build:    Changes that affect the build system or external dependencies (example scopes: gulp, broccoli, npm)
ci:       Changes to our CI configuration files and scripts (example scopes: Travis, Circle, BrowserStack, SauceLabs)
chore:    Other changes that don't modify src or test files
revert:   Reverts a previous commit

# 翻译如下
专长：新功能
修复：错误修复
文档：仅文档更改
样式：不影响代码含义的更改（空格、格式、缺少分号等）
重构：既不修复错误也不添加功能的代码更改。
性能：提高性能的代码更改。
测试：添加缺少的测试或更正现有测试
构建：影响构建系统或外部依赖项的更改（示例范围：gulp, broccoli, npm）
CI：对CI配置文件和脚本的更改（示例范围：travis、circle、browserstack、sattlabs）
杂务：不修改SRC或测试文件的其他更改
还原：还原以前的提交
```
选择一个 `commit` 类型后，会依次出现如下的英文提示：
```
? Select the type of change that you're committing: 
? What is the scope of this change (e.g. component or file name)? (press enter to skip)

? Write a short, imperative tense description of the change:

? Provide a longer description of the change: (press enter to skip)

? Are there any breaking changes? (y/N)   <-- 这里如果是大版本更新，与之前的功能不兼容，才选 y ，默认是 N
? Does this change affect any open issues?  (y/N)  <-- 这里是说解决了 issues 里面的哪个问题，默认是 N

# 翻译如下
？选择要提交的更改类型：
？此更改的范围是什么（例如组件或文件名）？（按Enter键跳过）

？写一个简短的命令式的变化描述：

？提供更改的较长描述：（按Enter键跳过）

？有什么突破性的变化吗？（y/n）
？此更改是否影响任何未解决的问题？（y/n）
```

## 使用 commitlint 来进行 git commit 的规范校验

[官方地址](https://github.com/marionebl/commitlint)

项目中安装
``` bash
npm install -D @commitlint/{cli,config-conventional}
```

创建 `.commitlintrc.js` 文件
``` bash
echo "module.exports = {extends: ['@commitlint/config-conventional']};" > .commitlintrc.js
```

安装 `husky`
``` bash
npm i husky -D
```

修改 `package.json`
``` json
{
  ...
  "husky": {
    "hooks": {
      "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
    }  
  }
}
```

测试，运行 `git commit -m "foo: this will fail"` 将会报错，提交不成功
``` bash
git commit -m "foo: this will fail"
husky > npm run -s commitmsg

   input: foo: this will fail
   type must be one of [build, chore, ci, docs, feat, fix, perf, refactor, revert, style, test] [type-enum]
   found 1 problems, 0 warnings

husky > commit-msg hook failed (add --no-verify to bypass)

git commit -m "chore: lint on commitmsg"
husky > npm run -s commitmsg

   input: chore: lint on commitmsg
   found 0 problems, 0 warnings
```

## 注意

必须先存在 `git` 仓库，再去执行 `commitizen init cz-conventional-changelog --save --save-exact` 生成 `commit` 规范，配置 `commitlint` 才能生效。

## standard-version 发布版本

[standard-version](https://github.com/conventional-changelog/standard-version)

安装
``` bash
npm i -D standard-version
```

添加 release 发布脚本
``` bash
{
  ...
  "scripts": {
    ...
    "release": "standard-version"
  }
  ...
}
```
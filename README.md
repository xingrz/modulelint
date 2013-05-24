modulelint
==========

Module Lint 是一款检测模块各种素质的工具。面向 Node 模块开发人员。

## 初步思路

- 检查模块的目录是否符合 CommonJS 包规范，每符合一样加 10 分
  - [x] lib
  - [x] doc
  - [x] test
  - [ ] package.json
- [x] 检查 README 文件的内容，去掉空格后每 1000 字 10 分。上限 30 分
- [ ] 检查 License 文件。具备 License 文件的项目加 10 分。检查文件名包含 License 与否。
- [ ] 检查测试用例数量。每个 case 加 2 分
- [ ] 检查测试覆盖率。覆盖率从 50% 开始，每覆盖 5% 加 5 分。高于 95% 加 10 分
- [ ] 检查 Coding Style, 每个文件不超过 3 个问题的，加 2 分。没有问题的加 5 分
- [x] 模块名字少于 10 个字母的，加 5 分
- [ ] API 注释，每个完整的注释加 5 分
- [x] 文档。doc 目录下每个文档加 5 分。每个文档去空格后，不应少于 500 字
- [ ] Change log 的检查，具备 Change log 的加 5 分。通常是 History.md 文件
- [x] 项目协作。通过 'git summary'，查看 contributor 的数量，从 2 个开始，每多一个贡献者，加 10 分
- [x] 通过 `registry.npm.org/module` 接口检查当前模块是否发布，已发布则加 5 分
- [x] 有 travis-ci 且 passing 状态加 15 分，有但 failed 只加 5 分
- [x] 文件 UTF-8 without BOM 检测。查出非 UTF-8 without BOM，每个文件扣 5 分

## 求你帮实现
上面罗列了一些初步的检测项，您也看到了，这是一件复杂的事情。每一项都要去寻找对应的工具来进行分析。所求代码贡献，每成功提交和合并一个检查器，赠送图灵社区图书一本。

### 代码提交指南
目录结构的组织如下

```
├── README.md
├── bin
│   └── modulelint
├── index.js
├── lib
│   ├── checklist // 该目录存放所有的检查项
│   │   └── folder.js // 检查项，必须导出check方法和rule描述。
│   └── modulelint.js
└── package.json
```

每个提交的检查相都应该存放在 `checklist` 目录下。每个检查相都应该导出 rule 描述和 check 方法

```js
/**
 * 规则描述
 */
exports.rule = "当前规则";
/**
 * 检查项
 * @param {String} source 检查的目录
 * @param {Function} callback 返回数据的回调函数
 */
exports.check = function (source, callback) {
  // 你的实现
  // 不对你用同步或异步方法做任何限制，但是为了兼容两种情况，结果请用callback传递返回
  if (err) {
    callback(err);
  } else {
    // 返回的结果包含两个属性。分数和纠错信息
    // result = {score: 10, info: somereason};
    callback(null, result);
  }
};
```

## 目标
每个开发者在完成他的模块之后，并不知道他做得好不好。排除掉模块的功能和接口是否优秀和令人感兴趣，至少我们可以从基本功上看开发者是否努力。
高分值也可以让模块开发者得到一点点成就感。  
希望这个指标可以略微量化下开源开发者做出的努力和收获，在推动标准化和提高基本要求方面出一点力。  

## 不算指导的指导
[优秀的JavaScript模块是怎样炼成的](http://www.infoq.com/cn/articles/how-to-create-great-js-module)

## 如何调用？
安装为命令行工具

```sh
npm install modulelint -g
```

自检一下试试看：

```sh
modulelint -i ~/git/modulelint
```
结果：

```
modulelint jacksontian $modulelint -i .
项目路径：/Users/jacksontian/git/modulelint
总得分数为：10
==============
检查项：folder
得分：10
1. folder `doc` is missing
2. folder `test` is missing
```
根据指导信息添加 `doc` 和 `test` 目录后：

```
modulelint jacksontian $modulelint -i .
项目路径：/Users/jacksontian/git/modulelint
总得分数为：30
==============
检查项：folder
得分：30
```


## 值得致谢的贡献者们
以下数据通过 `git-summary` 于2012-11-26生成。

```

 project  : modulelint
 repo age : 5 weeks
 active   : 20 days
 commits  : 60
 files    : 14
 authors  : 
    33	Jackson Tian            55.0%
    15	XiNGRZ                  25.0%
     6	tianqi                  10.0%
     5	Hyvi                    8.3%
     1	Will Wen Gunn           1.7%


```

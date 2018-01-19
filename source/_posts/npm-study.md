---
title: npm-study
date: 2017-11-21 18:25:12
tags: npm_cmd
---
# npm 简介
npm 可以使js开发者分享和重利用代码更为容易，并且更新要分享的代码也更为容易
# package.json　的使用
npm init 创建一个package.json 文件
```
{
	"name" : "project_name",
	"version" : "1.0.0"
}
```
# npm 常用命令
npm init --yes 创建一个默认的package.json
```
{
  "name": "react_study",
  "version": "1.0.0",
  "description": "This project was bootstrapped with [Create React App](https://github.com/facebookincubator/create-react-app).",
  "main": "index.js",
  "dependencies": {
    "accepts": "^1.3.4",
    "acorn": "^4.0.13",
    "acorn-dynamic-import": "^2.0.2",
    "address": "^1.0.3",
    "ajv": "^5.3.0",
    ....
 
  },
  "devDependencies": {},
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}

```
npm set init.author.email "xujianhuaap@gmail.com"
npm set init.author.name "skullmind"
npm set init.license "MIT"
npm install <package_name> --save //添加新的依赖包
npm publish <package_name> --tag beta --access public 
npm install <package_name> --save-dev //添加开发依赖包
npm update 
npm outdated //查看过时的包
npm uninstall <package_name> --save//移除本地对应的包并重package.json中移除
# npm　版本管理
npm 遵从语义版本原则（SemanticVersioning)
1. bug修复和次要更改，增加x.y.z 中的z
2. 新特性打破现存特性的　增加x.y.z 中的ｙ
3. 重大打破向后兼容的　增加x.y.z中的

```
^1.2.3 := >=1.2.3 <2.0.0
^0.2.3 := >=0.2.3 <0.3.0
^0.0.4 := >=0.0.4 <0.0.5
^允许化不改变，最左边非零数字

～允许补丁级别变化
～0.2.3 := >= 0.2.3 <0.3.0
~ 1.2.3 := >= 1.2.3 <1.2.4
```
npm install <package_name>@<version range> --save
# npm scope
@scope/project_name
npm init --scope=scope // package.json name字段会添加scope
npm config set scope username //不建议用

# npm 运行机制
## 包和模块
包是由package.json 描述的文件和目录，具体包括一下种类
a) 一个文件夹包含package.json
b) 一个包含a)的压缩包
c) 一个url 指向b)
d) 一个<name> @ <version> 以c)注册到仓库
e) 一个<name>@<tag> 指向d)
f) 一个<name> 有一个最近的tag 指向e)
g) 一个可以产生a)的git url 

模块是在Node.js代码中可以通过require()加载的目录或js文件
a) 一个含有main字段的package.json 文件的文件夹
b) 一个文件夹含有index.js
c) 一个js文件


# Vue
## 开发规范
### BEM命名方式(css)：
- block :代表块
- element :代表元素
- modifier :代表修饰符
``` css
  .block {}
  .block__element {}
  .block--modifier {}
```
### Eslint(检查JavaScript编程语法错误：编程规范，控制代码质量)
- 首先安装Node.js(>=4.x),nmp version 2+
- 全局安装或者本地安装
- 安装npx
``` js
  npm i npx -g
  yarn global add npx
  //npx执行指令的时候，先在当前目录查找=>全局=>零时安装
  //推荐facebook包管理yarn
  npm i yarn -g
  npm install elint //本地安装
  yarn add eslint //本地安装

  yarn global add eslint //全局安装
  npm install eslint -g //全局安装

  yarn config set registry http://registry.npm.taobao.org //设置淘宝镜像
  //让eslint成为项目构建的一部分，可以选择本地安装
  npm install eslint --save-dev
  //设置配置文件
  npx eslint --init
  //项目运行eslint:执行检测
  npx eslint yourfile.js
  npx eslint yourfile.js --fix //执行可以代码修复

  //全局安装eslint:
  npm install -g eslint
  eslint --init //设置配置文件
  npx eslint yourfile.js  //项目运行eslint:执行检测
```

## MVVM
- V ：视图
- VM ：同步Model和View的对象（监听）
- M ：数据模型



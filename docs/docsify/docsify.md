
## 参考资料

- [docsify中文官网](https://docsify.js.org/#/zh-cn/)

## 快速开始

推荐全局安装 docsify-cli 工具，可以方便地创建及在本地预览生成的文档。

~~~shell
npm i docsify-cli -g
~~~

## 初始化项目

如果想在项目的 ./docs 目录里写文档，直接通过 init 初始化项目。

~~~shell
docsify init ./docs
~~~

## 开始写文档

初始化成功后，可以看到 ./docs 目录下创建的几个文件

- index.html 入口文件
- README.md 会做为主页内容渲染
- .nojekyll 用于阻止 GitHub Pages 忽略掉下划线开头的文件

直接编辑 docs/README.md 就能更新文档内容，当然也可以添加更多页面。

## 本地预览

通过运行 docsify serve 启动一个本地服务器，可以方便地实时预览效果。默认访问地址 http://localhost:3000 。

~~~shell
docsify serve docs
~~~

## Loading 提示

初始化时会显示 Loading... 内容，你可以自定义提示信息。

~~~html
  <!-- index.html -->
<div id="app">加载中</div>
~~~

## 设置文档标题
### name

- 类型：`String`

文档标题，会显示在侧边栏顶部。

```js
<!-- index.html -->
window.$docsify = {
  name: 'docsify',
};
```

name 项也可以包含自定义 HTML 代码来方便地定制。

```js
<!-- index.html -->
window.$docsify = {
  name: '<span>docsify</span>',
};
```


## 配置GitHub挂件

### repo

- 类型：`String`
- 默认值: `null`

配置仓库地址或者 `username/repo` 的字符串，会在页面右上角渲染一个 [GitHub Corner](http://tholman.com/github-corners/) 挂件。

```js
<!-- index.html -->
window.$docsify = {
    repo: 'docsifyjs/docsify',
    // or
    repo: 'https://github.com/docsifyjs/docsify/',
};
```

## 设置生成目录的最大层级
### subMaxLevel

- 类型：`Number`
- 默认值: `0`

自定义侧边栏后默认不会再生成目录，你也可以通过设置生成目录的最大层级开启这个功能。

```js
window.$docsify = {
  subMaxLevel: 2,
};
```

## 加载自定义导航栏

### loadNavbar

- 类型：`Boolean|String`
- 默认值: `false`

加载自定义导航栏。设置为 `true` 后会加载 `_navbar.md` 文件，也可以自定义加载的文件名。

```js
<!-- index.html -->
window.$docsify = {
    // 加载 _navbar.md
    loadNavbar: true,

    // 加载 nav.md
    loadNavbar: 'nav.md',
};

<!-- _navbar.md -->
* [En](/)
* [简体中文](/zh-cn/)
```

## 启用封面页
### coverpage

- 类型：`Boolean|String`
- 默认值: `false`

启用[封面页]。开启后是加载 `_coverpage.md` 文件，也可以自定义文件名。

```js
<!-- index.html -->
window.$docsify = {
  coverpage: true,

  // 自定义文件名
  coverpage: 'cover.md',

  // 多个封面页
  coverpage: ['/', '/zh-cn/'],

  // 多个封面页，并指定文件名
  coverpage: {
    '/': 'cover.md',
    '/zh-cn/': 'cover.md',
  },
};

<!-- _coverpage.md -->

![logo](_media/icon.svg)
# docsify <small>3.5</small>
    
> 一个神奇的文档网站生成器。

- 简单、轻便 (压缩后 ~21kB)
- 无需生成 html 文件
- 众多主题

[GitHub](https://github.com/docsifyjs/docsify/)
[Get Started](#docsify)
```

## 加载自定义侧边栏
### loadSidebar

- 类型：`Boolean|String`
- 默认值: `false`

加载自定义侧边栏，参考[多页文档]。设置为 `true` 后会加载 `_sidebar.md` 文件，也可以自定义加载的文件名。

```js
<!-- index.html -->
window.$docsify = {
  // 加载 _sidebar.md
  loadSidebar: true,

  // 加载 summary.md
  loadSidebar: 'summary.md',
};
```

## 自动为每个页面增加标题
### autoHeader

- 类型：`Boolean`

同时设置 `loadSidebar` 和 `autoHeader` 后，可以根据 `_sidebar.md` 的内容自动为每个页面增加标题。[#78](https://github.com/docsifyjs/docsify/issues/78)

```js
<!-- index.html -->
window.$docsify = {
  loadSidebar: true,
  autoHeader: true,
};
```
## 部署到 GitHub Pages
### 推送到仓库
~~~shell
...创建新存储库
echo "# NetworkPorter" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/NetworkPorter/NetworkPorter.git
git push -u origin main
...推送现有存储库
git remote add origin https://github.com/NetworkPorter/NetworkPorter.git
git branch -M main
git push -u origin main
~~~
### 配置 GitHub Pages 服务
> 设置中找到  GitHub Pages 配置,选择分支,选择部署目录为docs,保存.
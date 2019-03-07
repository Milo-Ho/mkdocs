title: MkDocs
authors: milo

# MkDocs-material {:#}

## Introduction -- 简介 {:#introduction}
符合google material ui规范的静态文档网站生成器，使用markdown进行文档书写<br/>

#### mkdocs
- python编写的markdown解释器、编译器，带有本地cli工具
- 自带基于Tornado的小型http服务，用于本地调试
- 内置一键式发布至GitHub Pages
- 内置mkdocs风格、readthedocs风格的主题，并支持自定义主题
- 支持调用python模块实现语法及渲染的扩展

#### mkdocs-material
- python模块，符合google material ui规范的mkdocs自定义主题
- 针对特定语法、功能做了渲染优化
- 根据客户端浏览器页面尺寸自动缩放，对PC、移动设备都友好
- 丰富的页面配色，多达19种主体配色和16种悬停链接文字配色


## Install -- 安装 {:#install}

### Install with Pip -- 通过 Pip 安装 {:#install-with-pip}
使用 [pip](/python/pip/) 安装：<br/>
```
:::bash 
pip install mkdocs mkdocs-material
```


## Deploy to Local Server -- 部署到本地 {:#deploy-to-local}

### ⅰ. Create a new Project -- 创建新项目 {:#mkdocs-new}
执行以下命令创建一个新的 MkDocs 项目：<br/>
```
:::bash 
mkdocs new my_project
```

项目的文件结构：<br/>
```
:::text
- my_project/
  - mkdocks.yml
  - docs/
    - index.md
```

`mkdocs.yml` 是主配置文件，`docs/` 文件夹用来存放你的文档源文件。<br/>


### ⅱ. Configure -- 修改配置 {:#configure}
在主配置文件 `mkdocs.yml` 里修改 MkDocs 配置。<br/>
[查看 mkdocs.yml 范例](mkdocs_yml.md)<br/>

##### ▍Theme -- 主题 {:#theme}
```
:::yaml
theme:
  name: material
```


##### ▍Navigation -- 导航 {:#navigation}
```
:::yaml
nav:
  - Home: index.md
#  - Menu:
#    - SubMenu1: menu/sub_menu_1.md
#    - SubMenu2:
#        - SubMenu3: menu/sub_menu_3.md
```


##### ▍Extensions -- 扩展 {:#extensions}
```
:::yaml
markdown_extensions:
  - admonition
  - codehilite:
      guess_lang: false
      linenums: false
  - toc:
      permalink: true
  - footnotes
  - meta
  - def_list
  - pymdownx.arithmatex
  - pymdownx.betterem:
      smart_enable: all
  - pymdownx.caret
  - pymdownx.critic
  - pymdownx.details
  - pymdownx.emoji:
      emoji_generator: !!python/name:pymdownx.emoji.to_png
  - pymdownx.inlinehilite
  - pymdownx.magiclink
  - pymdownx.mark
  - pymdownx.smartsymbols
  - pymdownx.superfences
  - pymdownx.tasklist
  - pymdownx.tilde
```


### ⅲ. Run MkDocs builtin server -- 运行本地 mkdocs 服务 {:#mkdocs-serve}
```
:::bash
mkdocs serve
```

这时，mkdocs内置的服务器已经启动，通过浏览器打开 http://127.0.0.1:8000/ 可以查看效果。<br/>


## Deploy to Remote Server -- 部署到远程 {:#deploy-to-remote}

### Deploy to GitHub Pages -- 发布到 GitHub {:#mkdocs-gh-deploy}

###### ▍Add to Repo -- 添加到仓库
ⅰ. 在 GitHub 创建一个 repo<br/>
ⅱ. 将 repo 同步到本地<br/>
ⅲ. 将 mkdocs 根目录下的所有东西移到本地git目录下<br/>

###### ▍Deploy to GitHub -- 发布
ⅳ. 在本地 git 目录下执行 `mkdocs gh-deploy` 启动 mkdocs 服务
```
:::bash 
mkdocs gh-deploy
```


### Deploy to HTTP Server -- 发布到 HTTP-Server {:#mkdocs-build}
通过 `mkdocs build` 编译出 html 并手动同步至 http server 的根目录<br/>

###### ▍Generate Site Files -- 生成站点文件
在 git 根目录下执行命令：<br/>
```
:::bash
mkdocs build
```
命令执行完毕后可以看到 `site/` 目录<br/>

###### ▍Deploy to HTTP-Server -- 发布 
将 `site/` 目录里的所有东西手动同步至自己的 http server 的根目录<br/>

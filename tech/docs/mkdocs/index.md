title: MkDocs
Summary: A Simple MkDocs Project Introduction.
authors: milo

# MkDocs Summary {:#}

## Document 文档 {:#document data-toc-label="Document 文档"}
- [本文参考来源](https://cyent.github.io/markdown-with-mkdocs-material/)
- [mkdocs](https://www.mkdocs.org)
- [mkdocs-material](https://squidfunk.github.io/mkdocs-material/)
- [python markdown](https://pythonhosted.org/Markdown/)
- [pymdownx](https://facelessuser.github.io/pymdown-extensions/)


## Introduction 简介 {:#introduction}
符合google material ui规范的静态文档网站生成器，使用 [markdown](/markdown/) 进行文档书写。<br/>

#### mkdocs
- Python 编写的 markdown 解释器、编译器，带有本地 cli 工具
- 自带基于 Tornado 的小型 http 服务，用于本地调试
- 内置一键式发布至 GitHub Pages
- 内置 mkdocs 风格、readthedocs 风格的主题，并支持自定义主题
- 支持调用 Python 模块实现语法及渲染的扩展

#### mkdocs-material
- Python 模块，符合 google material ui 规范的 mkdocs 自定义主题
- 针对特定语法、功能做了渲染优化
- 根据客户端浏览器页面尺寸自动缩放，对 PC、移动设备都友好
- 丰富的页面配色，多达19种主体配色和16种悬停链接文字配色


## Install 安装 {:#install}
个人安装的环境：<br/>

- Python 3.7.2
- pip3
- 依赖的Python包：

    Package | Module | Version
    --- | --- | ---
    mkdocs | mkdocs | 1.0.4
    mkdocs-material | material | 4.0.1
    Markdown | markdown | 3.0.1
    pymdown-extensions | pymdownx | 6.0

??? note "使用pip安装Python包"
    使用 [pip](/python/pip/) 安装：<br/>
    ```
    pip install mkdocs==1.0.4 mkdocs-material==4.0.1 Markdown=3.0.1 pymdown-extensions==6.0
    ```
    
??? note "如果下载慢，可以更换安装源"
    ```
    pip install --trusted-host pypi.douban.com -i http://pypi.douban.com/simple/ mkdocs==1.0.4 mkdocs-material==4.0.1 Markdown=3.0.1 pymdown-extensions==6.0
    ```


## Usage 用法 {:#usage}
mkdocs 有4个命令：new, serve, build, gh-deploy<br/>
![mkdocs](/img/mkdocs/mkdocs.png)

### ▍mkdocs new {:#mkdocs-new data-toc-label="mkdocs new"}
新建一个 MkDocs 项目<br/>

![mkdocs new](/img/mkdocs/mkdocs_new.png)

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


### ▍mkdocs serve {:#mkdocs-serve data-toc-label="mkdocs serve"}
运行内置开发服务器<br/>

![mkdocs serve](/img/mkdocs/mkdocs_serve.png)

进入 `my_project` 目录启动 MkDocs 服务：<br/>
```
:::bash
cd my_project
mkdocs serve
```

??? attention
    如果不在 `my_project` 目录中，会提示 `Config file 'mkdocs.yml' does not exist.`

这时，mkdocs内置的服务器已经启动，通过浏览器打开 [http://127.0.0.1:8000/](http://127.0.0.1:8000/) 可以查看效果。<br/>


### ▍mkdocs build {:#mkdocs-build data-toc-label="mkdocs build"}
生成 MkDocs 文档<br/>

![mkdocs build](/img/mkdocs/mkdocs_build.png)

通过 `mkdocs build` 编译出 html 并手动同步至 http server 的根目录<br/>

在 git 根目录下执行命令：<br/>
```
:::bash
mkdocs build
```
命令执行完毕后可以看到 `site/` 目录。<br/>
将 `site/` 目录里的所有东西手动同步至自己的 http server 的根目录。<br/>


### ▍mkdocs gh-deploy {:#mkdocs-gh-deploy data-toc-label="mkdocs gh-deploy"}
把文档发布到 GitHub Pages<br/>

![mkdocs gh-deploy](/img/mkdocs/mkdocs_gh_deploy.png)

ⅰ. 在 GitHub 创建一个 repo<br/>
ⅱ. 将 repo 同步到本地<br/>
ⅲ. 将 my_project 根目录下的所有东西移到本地 git 目录下<br/>
ⅳ. 在本地 git 目录下执行 `mkdocs gh-deploy` 发布到 GitHub Pages<br/>


## Configuration 配置 {:#configuration}
在主配置文件 `mkdocs.yml` 里修改 MkDocs 配置。<br/>
[查看完整的 mkdocs.yml 配置](mkdocs_yml.md)<br/>

### Theme 主题 {:#theme}
```
:::yaml
theme:
  name: material
```


### Navigation 导航 {:#navigation}
```
:::yaml
nav:
  - Home: index.md
#  - Menu:
#    - SubMenu1: menu/sub_menu_1.md
#    - SubMenu2:
#        - SubMenu3: menu/sub_menu_3.md
```


### Extensions 扩展 {:#extensions}
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

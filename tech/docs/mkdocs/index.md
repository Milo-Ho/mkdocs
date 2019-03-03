title: MkDocs
authors: milo

# MkDocs {:#}

## 1. Introduction -- 简介 {:#introduction}

## 2. Install -- 安装 {:#install}

### Install with Pip -- 通过 Pip 安装 {:#install-with-pip}
使用 [pip](/python/pip/) 安装：<br/>
```
:::bash 
pip install mkdocs mkdocs-material
```


## 3. Deploy to Local Server -- 部署到本地 {:#deploy-to-local}

### 3.1. Create a new Project -- 创建新项目 {:#mkdocs-new}
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


### 3.2. Configure -- 修改配置 {:#configure}
在主配置文件 `mkdocs.yml` 里修改 MkDocs 配置。<br/>
[查看 mkdocs.yml 范例](mkdocs_yml.md)<br/>

#### Theme -- 主题 {:#theme}
```
:::yaml
theme:
  name: material
```


#### Navigation -- 导航 {:#navigation}
```
:::yaml
nav:
  - Home: index.md
#  - Menu:
#    - SubMenu1: menu/sub_menu_1.md
#    - SubMenu2:
#        - SubMenu3: menu/sub_menu_3.md
```


#### Extensions -- 扩展 {:#extensions}
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


### 3.3. Run MkDocs builtin server -- 运行本地 mkdocs 服务 {:#mkdocs-serve}
```
:::bash
mkdocs serve
```

这时，mkdocs内置的服务器已经启动，通过浏览器打开 http://127.0.0.1:8000/ 可以查看效果。<br/>


## 4. Deploy to Remote Server -- 部署到远程 {:#deploy-to-remote}

### 4.1. Deploy to GitHub Pages -- 发布到 GitHub {:#deploy-to-github}

#### Add to Repo -- 添加到仓库 {:#add-to-repo}
1. 在 GitHub 创建一个 repo
2. 将 repo 同步到本地
3. 将 mkdocs 根目录下的所有东西移到本地git目录下

#### Deploy to GitHub -- 发布 {:#mkdocs-gh-deploy}
4. 在本地 git 目录下执行 `mkdocs gh-deploy` 启动 mkdocs 服务
```
:::bash 
mkdocs gh-deploy
```


### 4.2. Deploy to HTTP Server -- 发布到 HTTP-Server {:#deploy-to-http-server}
通过 `mkdocs build` 编译出 html 并手动同步至 http server 的根目录<br/>

#### Generate Site Files -- 生成站点文件 {:#mkdocs-build}
在 git 根目录下执行命令：<br/>
```
:::bash
mkdocs build
```
命令执行完毕后可以看到 `site/` 目录<br/>

#### Deploy to HTTP-Server -- 发布 {:copy-site-to-http-server}
将 `site/` 目录里的所有东西手动同步至自己的 http server 的根目录<br/>

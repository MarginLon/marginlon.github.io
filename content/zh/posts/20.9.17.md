---
title: "Meme主题部署到GitHub Pages"
description:
toc: true
authors:
tags: 
  - github
  - github page 
  - meme
categories:
  - github page
series:
  - github
date: "2020-09-17T16:28:39+08:00"
lastmode: "2020-09-17T16:28:39+08:00"
featuredImage: 
featuredVideo: 
draft: false
---

本地运行[Meme主题](https://github.com/reuixiy/hugo-theme-meme)使用正常Hugo流程即可，注意使用__Extended__版本Hugo即可。简单叙述一下部署到Github Pages的流程，以供日后使用。

--------
# 本地安装Hugo（以windows例）
- 安装扩展版Hugo
- 检查命令：
>`hugo version`
- 建站命令：
>`hugo new site blog`
- Meme主题：
> ~ $ cd blog<br/>
> ~/blog $ git init<br/>
> ~/blog $ git submodule add --depth 1 https://github.com/reuixiy/hugo-theme-meme.git themes/meme<br/>
- 替换config：
> ~/blog $ rm config.toml<br/>
> ~/blog $ cp themes/meme/config-examples/zh-cn/config.toml config.toml<br/>
- 本地运行:
> hugo server -D
# 部署Github流程（以workflow方法）
- 正常部署hugo的命令：
> hugo --theme=meme --baseUrl="http://username.github.io" --buildDrafts<br/>
> //生成public文件夹<br/>
> cd public<br/>
> git init<br/>
> git add .<br/>
> git commit -m "info"<br/>
> git remote add origin http://github.com/username/username.github.io<br/>
> git push -u origin master<br/>
- Meme:
1. 推送源码而非`public`
> //在blog下运行<br/>
> git remote add origin http://github.com/username/username.github.io<br/>
> git push -u origin master<br/>
  2. 新建workflow

    ```yml
    #.github/workflows/build.yml
    name: build

    on:
      push:
        branches:
        - master
    jobs:
      build:
        runs-on: ubuntu-latest

        steps:
        - name: 'Building...'
          uses: reuixiy/hugo-deploy@v1
          env:
            DEPLOY_REPO:username/username.github.io
            DEPLOY_BRANCH:build
            DEPLOY_KEY: ${{ secrets.DEPLOY_KEY }}
            # https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
            TZ: Asia/Shanghai
      # DEPLOY_REPO为仓库 DEPLOY_BRANCH为分支 DEPLOY_KEY为私钥
    ```
 3. 生成公私钥
     - 公钥(id_rsa.pub)<br/>
       Settings > Deploy keys > Add deploy key<br/>
       勾选 Allow write access<br/>
     - 密钥(id_rsa)<br/>
       Settings > Secrets > New secret<br/>
       Name: DEPLOY_KEY<br/>
  4. push
     ```
     git add -A
     git commit -m "refactor: use hugo-deploy action"
     git push
     ```
5. Action处查看日志，Settings > Options > Github Pages，Source中的Branch选为build，完成即可上线。
<br/>
---
[Margin](https://marginlon.github.io/)

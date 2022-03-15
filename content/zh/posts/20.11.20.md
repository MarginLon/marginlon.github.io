---
title: "Github 远端仓库download不同本地"
description:
toc: 
authors:
tags: 
  - github
categories:
  - github
series:
  - github
date: '2020-11-20T18:28:39+08:00'
lastmode: "2020-11-20T18:28:39+08:00"
featuredImage: 
featuredVideo: 
draft: false
---
## 仅此记录过程，供日后复用。
---
- 1. 多主机共享ssh Public/Private Key
    + 原本地仓库的id_rsa和id_rsa.pub打包传输到其他主机
        + 生成 ssh key 命令：
        ```shell
        ssh-keygen -t -rsa -C "example@example.com"

        ssh -v git@github.com
        // No more authentication methods to try.
        // Permission denied (publickey).
        
        ssh -agent -s
        // SSH_AUTH_SOCK=/tmp/ssh-GTpABX1a05qH/agent.404; export SSH_AUTH_SOCK;  
        //SSH_AGENT_PID=13144; export SSH_AGENT_PID;  
        // echo Agent pid 13144;

        ssh-add ~/.ssh/id_rsa
        // 若 Could not open a connection to your authentication agent.
        
        eval `ssh-agent -s`
        ssh-add ~/.ssh/id_rsa
        ```
        + github settings 将id_rsa.pub录入

        + 验证
        ```
        ssh -T git@github.com
        ```
    + 设置用户信息
    ```
    $ git config --global user.name "Margin"
    $ git config --global user.email "example@example.com"
    ```
- 2. git clone address
- 3. git remote add origin address
- 4. git pull origin master
- 5. git push
---
- Errors
    - The following untracked working tree files would be overwritten by merge:
        - reason: 是由于一些untracked working tree files引起的问题。所以只要解决了这些untracked的文件就能解决这个问题。
        - Answer: ```$ git clean -d -fx```
        - Explanation: git clean 参数
            - -n 显示要删除的文件和目录
            - -x 删除忽略文件已经对git来说不识别的文件
            - -d 删除未被添加到git的路径中的文件
            - -f 强制运行
        - ```
            $ git clean -n 
            $ git clean -df
            $ git clean -f 
          ```
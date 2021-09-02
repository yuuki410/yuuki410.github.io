---
title: 利用 Hexo Action 自动部署博客
date: 2021-09-02 02:37:41
categories: 
#- diary
- guide
#- note
#- source
#- tool
tags:
- Hexo
- GitHub
- GitHub Action
- 持续集成
---
Hexo 好看，但是缺点是每次写完一篇新的博客就要敲一行 `hexo d` 推送一次，到了手机上则是直接废掉。

所以能不能自动部署呐？

当然可以，Github Action！

为了避免重复造轮子，Github 上甚至已经有现成写好的脚本可以使用了，那就直接白嫖好了～QAQ

照抄一下[Hexo Action](https://github.com/marketplace/actions/hexo-action)的示例配置即可，以下是本人的配置方法：
```yaml ~/yuuki410.github.io/.github/workflows/hexo_actions.yml
name: Hexo Deploy

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    name: A job to deploy blog.
    steps:
    - name: Checkout
      uses: actions/checkout@v1
      with:
        submodules: true # Checkout private submodules(themes or something else).
    
    # Caching dependencies to speed up workflows. (GitHub will remove any cache entries that have not been accessed in over 7 days.)
    - name: Cache node modules
      uses: actions/cache@v1
      id: cache
      with:
        path: node_modules
        key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
        restore-keys: |
          ${{ runner.os }}-node-
    - name: Install Dependencies
      if: steps.cache.outputs.cache-hit != 'true'
      run: npm ci
      #run: npm install
    
    # Deploy hexo blog website.
    - name: Deploy
      id: deploy
      uses: sma11black/hexo-action@v1.0.3
      with:
        deploy_key: ${{ secrets.DEPLOY_KEY }}
        #user_name: your github username  # (or delete this input setting to use bot account)
        #user_email: your github useremail  # (or delete this input setting to use bot account)
        #commit_msg: ${{ github.event.head_commit.message }}  # (or delete this input setting to use hexo default settings)
    # Use the output from the `deploy` step(use for test action)
    - name: Get the output
      run: |
        echo "${{ steps.deploy.outputs.notify }}"
```

然后push之前要设置一下仓库的deploy key，首先生成ssh key：
```bash
ssh-keygen -t rsa -C "username@example.com"
```

然后在仓库设置秘密变量名为`DEPLOY_KEY`。

好了上面的全部都是抄<https://github.com/marketplace/actions/hexo-action>的。

享受push完了就能自动部署的快乐吧（
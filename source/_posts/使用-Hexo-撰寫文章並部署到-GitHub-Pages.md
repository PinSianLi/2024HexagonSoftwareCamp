---
title: 使用 Hexo 撰寫文章並部署到 GitHub Pages
date: 2024-05-14 20:18:57
img: '/2024HexagonSoftwareCamp/images/hexo.jpg'
tags: 
  - 技術
  - 網頁
  - 部落格
---

## 1. 安裝 Node.js 和 nvm

### 1.1 官方網站

你可以從 [Node.js 官方網站](https://nodejs.org/) 下載並安裝 Node.js。為了更方便地管理 Node.js 版本，我們推薦使用 nvm (Node Version Manager)。

### 1.2 安裝 nvm

你可以從 [nvm 官方網站](https://github.com/nvm-sh/nvm) 獲取更多資訊。以下是兩種安裝 nvm 的方式：

#### 方式一：使用 cURL 安裝

打開終端並運行以下命令：

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
```

#### 方式二：使用 Wget 安裝

打開終端並運行以下命令：

```bash
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
```

安裝完成後，重新啟動終端或運行以下命令以加載 nvm：

```bash
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
```

### 1.3 安裝 Node.js

使用 nvm 安裝最新的 Node.js 版本：

    ```bash
    nvm install node
    ```

你也可以安裝特定版本，例如：

    ```bash
    nvm install 20
    ```

安裝完成後，確認 Node.js 和 npm 是否安裝成功：

    ```bash
    node -v
    npm -v
    ```

## 2. 安裝 Hexo CLI

接下來，我們安裝 Hexo CLI：

    ```bash
    npm install -g hexo-cli
    ```

## 3. 初始化 Hexo

選擇一個目錄來存放你的博客，然後在該目錄下運行以下命令來初始化 Hexo：

    ```bash
    hexo init blog
    cd blog
    npm install
    ```

## 4. 撰寫文章

使用以下命令來創建新文章：

```bash
hexo new post "你的文章標題"
```

這會在 `source/_posts` 目錄下創建一個新的 Markdown 文件，你可以用喜歡的編輯器打開並編輯它。

## 5. 本地預覽

在部署之前，可以使用以下命令來預覽你的博客：

```bash
hexo server
```

這會啟動一個本地服務器，打開瀏覽器並訪問 `http://localhost:4000` 就可以看到你的博客了。

## 6. 部署到 GitHub Pages

首先，配置 `_config.yml` 文件，添加 GitHub Pages 的相關設置。找到並編輯以下部分：

```yaml
deploy:
  type: git
  repo: https://github.com/yourusername/yourrepository.git
  branch: main
```

然後，安裝 hexo-deployer-git 插件：

```bash
npm install hexo-deployer-git --save
```

最後，運行以下命令來生成靜態文件並部署到 GitHub Pages：

```bash
hexo generate
hexo deploy
```

## 7. 自動化部署（可選）

如果你希望自動化部署流程，可以考慮使用 GitHub Actions。創建一個名為 `.github/workflows/deploy.yml` 的文件，內容如下：

```yaml
name: Deploy Hexo to GitHub Pages

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout
      uses: actions/checkout@v2

    - name: Setup Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '20'

    - name: Install Hexo and dependencies
      run: |
        npm install hexo-cli -g
        npm install

    - name: Deploy
      run: |
        hexo generate
        hexo deploy
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

這樣，每當你推送到 main 分支時，GitHub Actions 會自動部署你的博客到 GitHub Pages。

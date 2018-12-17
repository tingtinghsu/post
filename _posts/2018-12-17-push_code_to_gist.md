---
layout: post
title:  "將程式碼上傳至gist"
date:   2018-12-17 08:23:00 +1000
categories: git gist
---

# 原始需求

Oct-Nov.2018完成鐵人賽之後，想將文章整理放在自己的blog，但對於版型不甚滿意。

製作新版的過程中git blog, 從修改版型[註1]變成從 [https://tingtinghsu.github.io](https://tingtinghsu.github.io) 換到[https://tingtinghsu.github.io/blog](https://tingtinghsu.github.io/blog) 的工程。

## Jekyll資料夾結構

```bash

|-- index.html   #網站首頁
|-- _config.yml  #網站設定檔
|
|---/_includes  #網站結構資料夾
|    |--/JB      #Jekyll-bootstrap 模組
|    |--/themes  #Jekyll-bootstrap 預設版型
|
|---/_layouts   #版型
|
|---/_posts     #文章資料夾

```

## 舊版blog與新版blog差異

| 比較 | 舊版 | 新版|
|---|---|---|
|遠端網址|[https://tingtinghsu.github.io](https://tingtinghsu.github.io)| [https://tingtinghsu.github.io/blog](https://tingtinghsu.github.io/blog)|
| local路徑 | `/本機Github/tingtinghsu.github` | `/本機Github/blog`
啟動server| jekyll server  | jekyll server -p 4001
本機網址| jekyll server  | jekyll server -p 4001

[註1] 版型修改途中遇到大量問題，例如`push`之後，收到Github的警告訊息：

```bash

GitHub opened this alert 9 minutes ago

1 jekyll vulnerability found in Gemfile.lock 9 minutes ago

Remediation
Upgrade jekyll to version 3.8.4 or later. For example:
gem "jekyll", ">= 3.8.4"
Always verify the validity and compatibility of suggestions with your codebase.

Details
CVE-2018-17567 

moderate severity

Vulnerable versions: >= 3.8.0, < 3.8.4
Patched version: 3.8.4
Jekyll through 3.6.2, 3.7.x through 3.7.3, and 3.8.x through 3.8.3 allows attackers to access arbitrary files by specifying a symlink in the "include" key in the "_config.yml" file.

```

其原因主要來自於[Jekyll官方支援的版型](https://pages.github.com/themes/)，其Jekyll版本與穩定的blog版本不相容。

解法：在搬移內容的過程為了保留舊版本做對照(舊版僅供自己參考)，我將版型改為[lanyon](http://lanyon.getpoole.com/)，並按照[此教學影片]()**更改了blog網址**。

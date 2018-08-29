---
layout: post
title:  "解決Git常見錯誤 non-fast-forward問題"
date:   2018-08-29 11:52:00 +1000
categories: git github
---
最近每次遇到bug或錯誤都挺興奮 ~~開心(?)~~ 的，*因為又可以寫筆記了*
也可以記錄新手學習之路上遇到的常見問題，提供其他新手參考。

最近學Udemy平台Ruby學習線上課程，我在本機練習做了一個Ruby on Rails專案
（名稱定為：yelpdemo），

![https://ithelp.ithome.com.tw/upload/images/20180829/20111177kyLYw9hISV.png](https://ithelp.ithome.com.tw/upload/images/20180829/20111177kyLYw9hISV.png)

我先用`subl .`叫出Sublime編寫專案內的網頁，做一點修改後，
再用Terminal按照我自己[Day2筆記](https://ithelp.ithome.com.tw/articles/10198964)所說的Git步驟試著同步此專案到Github的同名repository儲存庫。


### 前情提要：我再Github的repo是上週設定好的，所以自己有點忘記當初發生什麼事～導致接下來的問題


### Step1. 首先`git status`
看看目前working directory中所有檔案的情形，紅色代表這些檔案需要被追蹤：

![https://ithelp.ithome.com.tw/upload/images/20180829/20111177faIYTaL67l.png](https://ithelp.ithome.com.tw/upload/images/20180829/20111177faIYTaL67l.png)

### Step2. `git add .`
把以上所有檔案交給Git，再輸入一次`git status`，綠色代表已更改為被追蹤：

![https://ithelp.ithome.com.tw/upload/images/20180829/20111177TwdEmf4DYw.png](https://ithelp.ithome.com.tw/upload/images/20180829/20111177TwdEmf4DYw.png)

### Step3 `git commit` 
`git commit -m "first commit"`，備註-m裡寫上"執行第一個提交指令commit"，將staged檔案儲存到儲存庫：

![https://ithelp.ithome.com.tw/upload/images/20180829/20111177m0erHLYic9.png](https://ithelp.ithome.com.tw/upload/images/20180829/20111177m0erHLYic9.png)
然後再輸入一次`git status`：

![https://ithelp.ithome.com.tw/upload/images/20180829/20111177y4gEfjUE01.png](https://ithelp.ithome.com.tw/upload/images/20180829/20111177y4gEfjUE01.png)
好了，到這邊都挺順的～

### Step4.設定好要加入的路徑 `git remote add origin`
`git remote add origin https://github.com/(你的帳號)/(你的專案名稱)`：
設定遠端origin的repository路徑，然後再推送：
`git push -u origin master` 
-u是update的意思，用了參數-u之後，未來就能直接使用不帶參數的`git pull`從以前push過的分支來pull。

>如果你沒有先`git remote add origin`就直接`git push`
>就會像我一開始一樣，出現了'找不到儲存庫'的問題喔！

![https://ithelp.ithome.com.tw/upload/images/20180829/20111177y7KBrSMI3T.png](https://ithelp.ithome.com.tw/upload/images/20180829/20111177y7KBrSMI3T.png)

### Step5. `git push -u origin master`

> [燈等！]更新被拒絕了，因為上週我在Remote建的的repo裡面，有些更動過的檔案版本，本地端並沒有：

![https://ithelp.ithome.com.tw/upload/images/20180829/20111177ElauUZHXsa.png](https://ithelp.ithome.com.tw/upload/images/20180829/20111177ElauUZHXsa.png)

> [解決]提示告訴我可以先用`git pull`，

![https://ithelp.ithome.com.tw/upload/images/20180829/20111177Z2gTdZDmzg.png](https://ithelp.ithome.com.tw/upload/images/20180829/20111177Z2gTdZDmzg.png)

> 然後再`git push`一次：

![https://ithelp.ithome.com.tw/upload/images/20180829/20111177KVWSk3rmg5.png](https://ithelp.ithome.com.tw/upload/images/20180829/20111177KVWSk3rmg5.png)

*重點在於最後一行：git push預設會推送「本地分支」到相同檔名的「遠端分支」*
```
When push.default is set to 'matching', git will push local branchesto the remote branches that already exist with the same name.
```


> [燈等！]問題再度出現：The current branch master has no upstream branch，表示當前分支主機沒有上游分支:
![https://ithelp.ithome.com.tw/upload/images/20180829/201111776Dl8yTegA2.png](https://ithelp.ithome.com.tw/upload/images/20180829/201111776Dl8yTegA2.png)

> [解決]按照提示，依樣畫葫蘆地`git push --set-upstream origin master`

![https://ithelp.ithome.com.tw/upload/images/20180829/20111177fhtIcNgZql.png](https://ithelp.ithome.com.tw/upload/images/20180829/20111177fhtIcNgZql.png)

> [燈等！] 新的問題產生，提示說我的master是non-fast forward 

![https://ithelp.ithome.com.tw/upload/images/20180829/201111776GMlki3aTG.png](https://ithelp.ithome.com.tw/upload/images/20180829/201111776GMlki3aTG.png)

這個non-fast forward 的意思是本機commit和遠端的不相同，本地和遠端的Repository發生文件衝突問題（線圖有分岔）。

由於上週在遠端創建的資料夾內容文件發生改變，因此不允許我把本地的文件覆蓋上去。

關於合併local端和remote端，近一步了解可以看[git官網](https://git-scm.com/book/zh-tw/v1/Git-%E5%88%86%E6%94%AF-%E9%81%A0%E7%AB%AF%E5%88%86%E6%94%AF)，或是[連猴子都能懂的Git入門指南](https://backlog.com/git-tutorial/tw/stepup/stepup1_4.html)解釋

在push之前，我需要
`git fetch`
`git merge`
將遠端的改變用merge合併到本地上。

或是我可以用`git push -f`  強制覆蓋本地文件替代git儲存庫的內容。

Google關鍵字在[stackoverflow](https://stackoverflow.com/questions/10298291/cannot-push-to-github-keeps-saying-need-merge)找到的解法是
`git push -f origin <branch>`。

-f代表force：強制推送。如果不想把遠端分支的更動合併（merge）到本地分支，則可以使用這個指令。**但要小心使用。**

[解決]

![https://ithelp.ithome.com.tw/upload/images/20180829/20111177vYKA8TDUfV.png](https://ithelp.ithome.com.tw/upload/images/20180829/20111177vYKA8TDUfV.png)

回到github站上檢查，終於同步完成：

![https://ithelp.ithome.com.tw/upload/images/20180829/201111779euCL1MqMF.png](https://ithelp.ithome.com.tw/upload/images/20180829/201111779euCL1MqMF.png)

===
後記：
這是寫給自己提升記憶的筆記。相信之後如果在本機再建立其他新專案、遠端建立儲存庫並使用gite功能，可能還是會遇到以上類似的問題，身為自學git新新手，我沒有直接發問求救，而是靠google增加觀念、進而找到解答，因此還蠻開心的！

看了資料，有些工程師的建議是少用`git pull`，以`git fetch`和`git merge`代替。也許以後實作之後有類似的經驗和心得，再寫點相關的文章。

ref: 
[git](https://blog.exfast.me/2016/05/git-instructions-instructions/)
[龍哥的git部落格] (https://gitbook.tw/chapters/using-git/add-to-git.html)

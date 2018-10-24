---
layout: post
title:  "30天修煉Ruby面試精選30題 - Day25 and or 和 && || 比較"
date:   2018-10-24 17:51:00 +1000
categories: ruby interview
---

# Ruby經典面試題目 #25

`Day25 [and], [or] 和 [&&], [||] 比較? What is the difference between [and]`, `[or]` and `[&&]`, `[||]` ?`

基本上來說：

* `and` 就是 `&&`（且）
* `or` 就是`||` （或）

然而，`and` 和 `&&` 以及 `or` 和 `||` 的意思雖然一樣，但就像以前在數學課學過的`先乘除後加減`的概念一樣，程式中的運算子之間也具有優先順序之分。`&&`, `||` 的優先權高於`and`, `or`

# short-circuit evaluation 最短路徑計算

在此我們介紹一個概念：`short-circuit evaluation`(短路計算、捷徑計算、最小化計算)，當表達式的結果已經確定時，程式會節省資源，減少不必要的運算。舉個例子來說：

```ruby
a = 0
b = 1
if (a == 1 && b%2 == 1)
  p "true"  
else
  p "false"
end
```

在以上的例子裡，`&&`代表括弧內的條件判斷必須`全部為真`才成立。一開始我們指定`a = 0`，因此，在`if (a==1)`這個敘述為`false`的情況，我們不需要走完下一條判斷式`b%2 == 1 (b為奇數)`，這就是「最短路徑計算」的概念。

# && 與 and

來舉個例子：我的鐵人賽日記`IronmanDairy` 來到第25天，且第25天是個奇數天，顯示為true.

`&&` 與 `and` 都是`且`的意思。

```ruby
IronmanDairy = 25

p IronmanDairy == 25 && IronmanDairy % 2 == 1 #=>true
```

和

```ruby
IronmanDairy = 25

p IronmanDairy == 25 and IronmanDairy % 2 == 1 #=>true
```

相同。

# `&&` 的優先權高於 `and`

從剛剛的例子，我們明白到 `&&`和 `and` 可以確認一串條件判斷式時，其中是否有`false`或`nil`的部分，全部為真的話才會通過條件判斷。


```ruby
ironman_and_dairy = ironman_and_dairy.publish! and IronmenDairy.find_by_day(25)  
```

用上面例子來說明:
`ironman_and_dairy`鐵人賽日記只有在我PO文`(publish)`之後，才會被找到。

# && 出現錯誤的情形

```ruby
p IronmanDairy = 25 &&IronmanDairy / 2

#undefined method `/' for true:TrueClass (NoMethodError)
```

為什麼會出現NoMethodError呢？

原因是因為`&&`有較高的優先權，上式對Ruby來說，會寫成：

```ruby
p IronmanDairy = (25 &&IronmanDairy) / 2
```

而`25 && IronmanDairy` 這則判斷式為`true`，但TrueClass 除以2是無意義的，




# 練習: `a = nil and true`

* [Difference between “and” and && in Ruby?](https://stackoverflow.com/questions/1426826/difference-between-and-and-in-ruby)

* [Using “and” and “or” in Ruby](http://www.virtuouscode.com/2010/08/02/using-and-and-or-in-ruby/)

* [Difference between “and” and && in Ruby?](https://stackoverflow.com/questions/1426826/difference-between-and-and-in-ruby)
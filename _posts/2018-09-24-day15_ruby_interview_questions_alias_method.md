---
layout: post
title:  "30天修煉Ruby面試精選30題 - Day15 alias 和 alias_method 比較"
date:   2018-09-24 08:56:00 +1000
categories: ruby rails interview junior
---

昨天的文章在研究map和collect的時候，有一句話提到map是collect的別名 (map is an alias for collect)。除了讓我學到alias這個英文單字之外，也意外發現，ruby裡面還有alias方法！今天就來研究alias吧～

---

# Ruby經典面試題目 #15

`Day15 alias 和 alias_method比較? What's the difference between alias和 alias_method?`

## alias

幾年前我曾經認養寵物-兩隻可愛的虎斑貓，首先最重要的步驟就是幫可愛的毛小孩取名了！還記得剛開始認養的時候非常興奮，所以我想用程式碼紀念一下當初的過程。
為了命名第一隻貓（一隻底色為白色的虎斑貓），首先我建立Pet類別，定義cat_name方法，且此方法的`alias`別名是`name`（新的別名要放在舊的名稱前面）：

```ruby
class Pet
  def cat_name
    p "Mac"
  end

  alias name cat_name #Method之間不需要放","逗號
end

Pet.new.name #=> Mac
```

結果印出：Mac。這是我家寵物的名字。

## alias_method

如果利用`alias_method`寫法也是可以的，只要把name方法前面加上`:冒號`，變成`Symbol`符號的寫法：

```ruby
class Pet
  def cat_name
    p "Mac"
  end

  alias_method :name, :cat_name #Symbol之間要加","逗號
end

Pet.new.name #=> Mac
```

用`alias_method`的優點是可以作用在繼承的類別，來看看例子🌰：

我家的Mac是一隻調皮的小公貓，有另外一個綽號叫做“麥少爺"。所以我寫了一個類別`Cat`繼承自`Pet`。在`Cat`類別和`Pet類別`下都有同名的方法`cat_name`。

```ruby
class Pet
  def cat_name
    p "Mac"
  end

  def self.nickname
    alias_method :name, :cat_name
  end
end

class Cat < Pet
  def cat_name
    p "麥少爺"
  end
  nickname
end

Cat.new.name #=> "麥少爺"
```

當我們用`Cat.new.name`產生一個新物件，程式會走一遍`Cat`類別，到了`nickname`這個方法，會繼承父類別`Pet`的`self.nickname`方法。

尋找到`alias_method`之後，呼叫`name`別名的舊名`:cat_name`，再回來`Cat`類別跑完執行`cat_name`方法。

此時Mac的花名出現啦！叫做`麥少爺`。

## alias_method與alias的不同

如果把上述程式碼轉為`alias`試試看，幫我的第二隻貓：Dell設定綽號。

```ruby
class Pet
  def cat_name
    p "Dell"
  end

  def self.nickname
    alias name cat_name
  end
end

class Cat < Pet
  def cat_name
    p "戴公子"
  end
  nickname
end

Cat.new.name #=> Dell

```

`Cat.new.name` 呼叫`nickname`方法後，並沒有出現別名`戴公子`，名字仍為`Dell`。
這是因為`alias`只會尋找其關鍵字存在的scope，在本案例裡是`Pet`類別裡面。

比一比：

[alias](http://ruby-doc.org/stdlib-1.9.1/libdoc/rdoc/rdoc/RDoc/Alias.html) | [alias_method](http://ruby-doc.org/core-2.1.5/Module.html#method-i-alias_method)
------------- | -------------
在libdoc之下的RDoc | 屬於Module模組方法
只會作用在關鍵字所屬的scope | 可以重新定義方法，較為彈性

===

Ref：
[一份Ruby面試題](https://ruby-china.org/topics/13272) |
[alias vs alias_method](https://blog.bigbinary.com/2012/01/08/alias-vs-alias-method.html)|
[In Defense of Alias](https://ernie.io/2014/10/23/in-defense-of-alias/)|
[Should I use alias or alias_method?](https://stackoverflow.com/questions/4763121/should-i-use-alias-or-alias-method)|
[Method alias in ruby](https://wikimatze.de/method-alias-in-ruby/)|
[Ruby-doc.org : alias](http://ruby-doc.org/stdlib-1.9.1/libdoc/rdoc/rdoc/RDoc/Alias.html)|
[Ruby-doc.org : alias method](http://ruby-doc.org/core-2.5.1/Module.html#method-i-alias_method)|
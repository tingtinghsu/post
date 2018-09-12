---
layout: post
title:  "30天修煉Ruby面試精選30題 - Day04 玩弄Ruby的方法: instance method與class method"
date:   2018-09-12 09:00:00 +1000
categories: ruby rails interview junior
---

前情提要：
在第三天裡，我們解說了如何在class裡用include與extend使用module的method。 

> Include is for adding methods to an instance of a class. 

> Extend is for adding class methods. [出處](http://www.railstips.org/blog/archives/2009/05/15/include-vs-extend-in-ruby/)

話不多說，進入今天的章節：

Ruby經典面試題目 #04
===

`包含與延伸有什麼不同? What's the Difference Between Include and Extend? `

還記得我們昨天舉的例子：網路圖書館（模組）有很多知識（方法）讓我們取用（include），
讓你與我都能夠突破先天(繼承)的限制，變成更加聰明靈活的IT人。

```
module Library 
  def IThelp 
    p "I'm learning from others' IT articles on IThelp Website!"
  end
end

class EveryoneLearnsRuby
  def initialize(name)
    @name = name
  end
  include Library 
end

Ting = EveryoneLearnsRuby.new("Ting") 
Ting.IThelp  
You = EveryoneLearnsRuby.new("You")
You. IThlep
```

當然，使用類別(class)繼承也有它的好處，

例如：在已有的功能基礎上，再追加擴展本身已有功能。
（龍生龍、鳳生鳳；老鼠生的兒子會打洞！）

或是以相同名稱的方法，重新定義，產生不同的效果。
（王老先生有塊地，~~那王小弟長大後可以把王老先生的那塊地拿去蓋民宿~~。）


但模組(module)的include就像開外掛一樣，讓我們可以在這個星球上學會更多技能。

為了比較include與extend，我們把圖書館模組來稍加改寫：
```
module Library 
  def IThelp 
    p "IThelp helps me!"
  end
end

class NewbieLearnsRuby
  include Library 
end

NewbieLearnsRuby.new.IThelp 
#IThelp helps me!

NewbieLearnsRuby.IThelp
#NoMethodError
```
如果我們把`NewbieLearnsRuby.new.IThelp`誤寫成`NewbieLearnsRuby.IThelp`，就會出現錯誤。
> undefined method `IThelp' for NewbieLearnsRuby:Class (NoMethodError)

奇怪，為什麼會這樣呢？

我們回到改寫前的圖書館例子：我先宣告(new)一個新物件You，
讓「You」這個變數名字指向`EveryoneLearnsRuby.new("You")`

```
You = EveryoneLearnsRuby.new("You")
You.IThlep
```

所以剛剛的`NewbieLearnsRuby.new.IThelp`其實是以下的簡化：
```
You = NewbieLearnsRuby.new
You.IThelp
# [NewbieLearnsRuby.new].IThelp [中括號內的變數就是You!]
```
這就是我們為什麼不能漏掉`.new`的原因。

那，如果改寫成extend的程式碼，會是怎樣的光景呢？

```
module Library 
  def IThelp 
    p "IThelp helps me!"
  end
end

class NewbieLearnsRuby
  include Library 
end

class ExtendRuby
  extend Library
end

NewbieLearnsRuby.new.IThelp 
# IThelp helps me!

ExtendRuby.IThelp
# IThelp helps me!
```

由以上可知，`include`代表Newbie類別學Ruby時需要new一個新的物件實體，然後才能使用方法。
但`extend`不用，在類別中使用它可以讓我們直接把方法拿過來用。

```
ExtendRuby.IThelp
# IThelp helps me!

ExtendRuby.new.IThelp 
# NoMethodError
```

同樣的，想進一步了解為什麼輸入`ExtendRuby.new.IThelp `也是`NoMethodError`。接下來我們要拿關鍵字 `the difference between include and extend in ruby`去請教Google大神：

> Class can use methods from three areas: Instances of class can call methods that are defined as instance methods in their class. Instances of Class have access to the instance methods defined in Module. Or instances can call a singleton method of a class object. [出處](https://medium.com/@lauren.kroner/ruby-class-vs-instance-methods-a5182ce7de49)

為了抽絲剝繭這段話的含義，這裡的`實體方法instance method`和`類別方法class method`將會成為我們下一篇文章的重點囉！



Ref：
[Top 10 Essential Ruby Interview Questions](https://blog.bater.gq/ruby/2018/02/02/top-10-essential-ruby-interview-questions.html) |
[Ruby on Rails Technical Interview Questions](https://github.com/timurcatakli/ruby-on-rails-interview-questions-answers) |
[Ruby: Class vs Instance Methods](https://medium.com/@lauren.kroner/ruby-class-vs-instance-methods-a5182ce7de49) |

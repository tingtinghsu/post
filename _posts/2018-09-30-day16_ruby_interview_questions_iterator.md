---
title:  "Ruby面試精選30題 - Day16 iterator 和 loop 比較"
preview: "Ruby interview question: What's Iterator? How many iterators are there in Ruby?"
permalink: "/articles/2018-09-30-day16_ruby_interview_questions_iterator"
date:   2018-09-30 08:29:00
layout: post
tags: 
  - "interview"
comments: true
---

話說工程師們是一群創新又偷懶的人，能夠只寫一行程式碼，就不用寫兩行。迭代器就是一個能幫助我們達成這個目標的好東西，能把做重複的事情的情況減少到最低，這樣我們就可以省下更多時間去享受人生啦～
<!-- more -->

---

重點摘要:
* abstact
{:toc}

---

# Ruby經典面試題目 #16

* 什麼是迭代器? 請解釋Ruby裡面有哪幾種迭代器?  
What's `Iterator`? How many iterators are there in Ruby?`

# Iterator

迭代器 (iterator) 就是用來重複多次相同的事。(Do one thing many times like a loop.)還記得我們在前天的時候提到`.each`, `.map`和`.collect`時候，有提到迭代器的概念。今天要來接觸更多的迭代器，在文章後半部會提到相似的迴圈！

首先從`.each`開始複習：

```ruby
(collection).each do |variable|
 #做一些重複的事#
end
```

範例:

```ruby
(1...4).each do |i|
   puts "鐵人賽第16天了!!"
end
```

結果:

```ruby
鐵人賽第16天了!!
鐵人賽第16天了!!
鐵人賽第16天了!!
```

這邊的`(1...4)`是`Range（範圍）`的寫法。

`(...)`中間有三個點，代表不包含最後一個數字4。（從1到3而已，所以只印了3次）；

如果是只有兩點的話`(..)`，像這樣子`(1..4`)就會印4次喔。

## String: 字串迭代器: each_line iterator

Each_line和each的方法類似，都可以利用`do...end`或是`{}`大括弧的語法，在`block`區塊內寫下我們想要程式幫忙重複執行的功能。`each_line`可以偵測到空行`\n`段落，印出空行：

```ruby
"[成為工程師]\n專心！\n堅持！\n一定可以\n成功的喔！".each_line do |line|
  puts line
end
```

結果：

```ruby
tingdeMacBook-Air:Ironman tingtinghsu$ ruby iterator.rb
[成為工程師]
專心！
堅持！
一定可以
成功的喔！
```

## Character: 字元迭代器: each_byte iterator

除了字串迭代，字元也可以迭代！`.each_byte`可以幫我們輕鬆達到這個功能，

```ruby
"You can achieve anything you want to".each_byte { #block把do end 換成大括弧也可以
  |c| printf "%c ", c
}
```

`printf`使用格式化的字元`%c`輸出字串，就和c語言一樣；而`print`與`puts`的差別在於`print`不會幫我們自動換行，[參考這裡](http://maku77.github.io/ruby/io/print-to-stdout.html)。（這大概是為什麼`puts`比較常見吧！有沒有換行的視覺效果差很多～）

```ruby
tingdeMacBook-Air:Ironman tingtinghsu$ ruby iterator.rb
[成為工程師]
專心！
堅持！
一定可以
成功的喔！
Y o u   c a n   a c h i e v e   a n y t h i n g   y o u   w a n t   t o tingdeMacBook-Air:Ironman tingtinghsu$
```

信心喊話完成！

接下來我們要介紹各種迭代器運用在數值變數的情形。為了紀念鐵人賽第16天（進度條超過一半了！）我打算利用各種迭代器印出從1到16的偶數，並在文章的最後與loop迴圈做比較：

## each iterator

```ruby
(1..16).each do |i|
 if (i%2 == 0)
  puts i
 end
end #印出1-16之間的偶數 2 4 6 8 10 12 14 16
```

## time iterator

```ruby
16.times do |i|
 if (i%2 == 0)
  puts i
 end
end
# #印出0-15之間的偶數 0 2 4 6 8 10 12 14
```

使用`.time`方法，`i`會從`0`開始跑迭代。

小括號代表的範圍`(..)`又出現了！還記得括弧內如果以兩點`..`表示，會包含頭與尾的數值嗎?

## upto iterator

```ruby
1.upto(16) do |i|
  if (i%2==0)
  puts i
  end
end #印出1-16之間的偶數 2 4 6 8 10 12 14 16
```

## downto iterator

除了往上數`.upto`，iterator也有往下數`.downto`的方法：

```ruby
16.downto(1) do |i|
  if (i%2==0)
  puts i
  end
end  #印出16-1之間的偶數 16 14 12 10 8 6 4 2
```

## step iterator

`.step` iterator就像爬樓梯一樣，一次爬兩格樓梯試試看：

```ruby
2.step(16, 2) do |i|
  puts i
end #印出2-16之間的偶數 2 4 6 8 10 12 14 16
```

樓梯可以往上爬也可以往下走:

```ruby
2.step(16, 2) do |i|
  puts i
end #印出16-2之間的偶數 16 14 12 10 8 6 4 2
```

# Loop

在挑戰今天的鐵人賽的時候，我發現`Loop`和`Iterator`基本上都有類似的功能：`重複做相同的事`。來介紹迴圈`loop`：`for`迴圈， `while`，以及While的孿生兄弟`until`吧！

## 直接把Loop關鍵字加在block之前

如果要達成和迭代器，同樣的功能(印出1到16之間的偶數)，Loop該如何進行呢?

```ruby
i=1 #記得設定初始值!
puts(loop do
  if (i%2==0)
    puts "#{i}"
  end
  i+=1
  break if i>16 #記得加上終止條件
end) #印出2-16之間的偶數 2 4 6 8 10 12 14 16
```

光是看到這樣落落長的寫法就覺得很不美觀，換成while試試:

## while loop

```ruby
i =1 #記得設定初始值
while (i <= 16) do
  if (i%2 == 0)
    puts i
  end
  i +=1
end  #印出2-16之間的偶數 2 4 6 8 10 12 14 16
```

## until loop  

`until`則剛好是`while`的相反，意思是除非條件不符合，否則進行迴圈:

```ruby
i =1 #記得設定初始值
until i > 16
  if (i%2 == 0)
    puts i
  end  
  i += 1
end
```

那，Loop迴圈和Iterator迭代器，到底差在哪裡，且孰優孰劣呢?

如果只是跑單純的Loop話（例如從1加到1百萬），Loop速度會比Iterator快很多。[參考Bater的文章](https://blog.bater.gq/ruby/2017/11/26/ruby-Iterator-and-which-one-is-faster.html)

而[從Codecademy這篇文章](https://www.codecademy.com/en/forum_questions/5294ba4480ff338cc9002444)可以了解，Loop一定要包含終止條件。如果你不能確定迴圈的終止條件（必須為`true`）就執行迴圈，那無窮迴圈（鬼打牆）的情形就有可能發生。

舉個例子🌰來說，我在改while loop程式碼以符合我的邏輯（印出1-16之間的偶數）時，不小心把`i+=1`放進`if條件判斷式`裡，結果程式碼整個當掉（因為i到最後一次執行+1完就大於16了）

```ruby
i =1 #記得設定初始值
while (i <= 16) do
  if (i%2 == 0)
    puts i
    i +=1 #這行放錯地方了!
  end
  #i +=1  應該要放在if...end外才對!
end
```

因此，記得在while的迴圈之前，必須設定`i=1`初始值，且強制讓每次跑完迴圈時，i能夠遞增`i+=1`。

## for loop

如果我們不要每次都設定`i=1`和`i+=1`（好煩啊而且位置容易擺錯、程式就當掉 XD），可以利用`for...end`迴圈，與文章開頭提到的`Range(..)`(範圍)的結合：

```ruby
for i in 1..16 # (1..16)範圍的小括號可省略
 if (i%2 == 0) #加入偶數的條件判斷
  puts i
 end
end #印出1-16之間的偶數 2 4 6 8 10 12 14 16
```

最後，為什麼我們要多使用迭代器呢?因為iterator常常和array或是hash結合，加上許多實用的字串方法，幫我們用簡單、優雅的寫法完成複雜的功能：

### Iterator - `.each`寫法

```ruby
a = [2, 4, 6, 8]
p a.each {|x| x*2} #=> [2,4,6,8]
```

### Loop - `while` 用在 Array的寫法

```ruby
a = [2, 4, 6, 8]
i = 0
while (i < a.length)
  print (a[i]*2).to_s + " "
  i += 1
end
print "\n" #=> 2 4 6 8
```

只要在Array或Hash的結構裡、實現的功能越複雜，iterator就比loop省下更多的程式碼！

如上述顯示，`.each`比起while，僅用了2行程式碼就完成了我們需要的功能。有沒有發現迭代器的強大之處呢？

每天寫完程式文章，都會發現自己對重要觀念的印象又更加深了！或許這就是鐵人賽的魅力吧：）

Ref：

* [Ruby 使用手冊 - 迭代器 Iterators](https://guides.ruby.tw/ruby/iterators.html)
* [Ruby Iterator and which one is faster](https://blog.bater.gq/ruby/2017/11/26/ruby-Iterator-and-which-one-is-faster.html)
* [Loops vs iterators: I don't get the difference](https://www.codecademy.com/en/forum_questions/5294ba4480ff338cc9002444)
* [A Wealth Of Ruby Loops And Iterators](https://www.skorks.com/2009/09/a-wealth-of-ruby-loops-and-iterators/)

---
layout: post
title: 使用 Jekyll 建立自己的 Github Page Blog
tags: [Blog]
---

<div class="message">
用這個 Blog 誕生的過程來作為第一篇文章，我想是再適合不過了。
</div>

    
一直以來我在 computer science 的學習上都沒有做筆記的習慣，因此很多事情其實不是很熟悉或是過沒幾天就忘了。最近受到了學長的啟發，開始想記錄一些東西，看來看去發現 Github Page 搭配 Jekyll 是個免費又很適合工程師的選擇，因此花了幾天看看別人的教學與說明好不容易弄出一個雛形。本篇就是紀錄過程中遇到的一些困難與現在我還不是很懂的地方，希望在日後能夠摸索更深入些。

<!-- more -->  

# <strong>What's Jekyll

其實我在架設完成之後對於 Jekyll 是什麼還是懞懞懂懂，對於幾乎沒接觸過相關網路程式的我來說，他是一個用 Ruby 編寫出來的部落格 framework。他主要的功能是讓使用者可以簡單的建構出靜態頁面的部落格。Github Pages 目前提供方便的功能，只要將我們的 Jekyll 存放到 Github 上面的 reposity，就能完成簡單的部落格設置。 

# <strong>Reference

這邊先放上我參考的一些網站，有些是官方文件有些是個人部落格，都值得參考學習

- [Jekyll 官方網站](https://jekyllrb.com)
- [GitHub Help 的教學文件](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/)
- [網路部落客文章-利用 Jekyll 與 Github Page 建立自己的 Dev-Blog](http://seans.tw/2016/make-own-blog-with-jekyll-and-github-page/)
- [中國部落客文章-用 Jekyll 搭建的 Github Pages 个人博客](http://www.jianshu.com/p/88c9e72978b4)
- [Rhadow's Tech Note](https://github.com/Rhadow/Rhadow.github.io)
- [How to Install Jekyll Themes?](https://www.youtube.com/watch?v=bty7LHm14CA)
- [我自己的 GitHub Pages commit log](https://github.com/nk910216/nk910216.github.io/commits/master) 

# <strong>Preparing

> 在一切開始前，請先確保自己有一個能夠拿來打指令的 Terminal 與一個 GitHub 帳號
<br/>

現在我使用的是 macbook air，因此之後的紀錄都是在 mac os 上面建構的，不過 command line 的指令差別不大，只是環境的安裝上 Windows 系統會複雜許多，linux 系統的話請檢查一下軟體需不需要安裝或者是更新，max os 則是檢查一下版本即可。另外，因為我們想把我們的網頁放在 GitHub Page 上面，因此一個 GitHub 帳號也是必須的。

前面我有提到說 Jekyll 使用到 Ruby, 因此我們還需要先確定有安裝 Ruby。
{% highlight shell %}
$ ruby --version
{% endhighlight %}
如果沒有的話可以到[Ruby 官方網站](https://www.ruby-lang.org/en/downloads/)下載安裝。
接下來我們還得安裝一個叫做 Bundle的軟體。他可以協助我們輕鬆地安裝 Ruby projects，如果你跟我一樣跟 bundle 非常不熟，可以參考 [bundle 的官方網站](http://bundler.io)
{% highlight shell %}
$ gem install bundler
{% endhighlight %}

# <strong>Let's set up our Blog

> 其他的教學通常都會教導你從一個最基礎的 Jekyll 開始設置，不過我最後的做法是直接拿別人設計好的 Blog Template 來套用。因此這邊的方法會直接從別人的 GitHub clone 一份來修改，如果想使用其他方法可以參考官方文件的正規方法。

我自己對於設計網頁樣式非常地不在行，所以一開始我就考慮使用別人幫你設計好的 Template，為了讓接下來的過程比較順利，請在開始前請先參考一下這段教學影片：[How to Install Jekyll Themes?](https://www.youtube.com/watch?v=bty7LHm14CA)
<br/>

首先，我們要去喜愛的 Template Github 網站 Fork 一份到自己的帳號並且修改，我自己使用的是影片中推薦的[Lanyon](https://github.com/poole/lanyon)

![alt text](/assets/posts/2017-02-05-howtosetupblog/lanyon.png "lanyon website")

接著我們回到我們自己的 GitHub ，並找到剛剛 Fork 出來的 reposity。這時我們有兩個選擇：

- 部落格網址為：你的帳號.github.io/你自訂的名稱
- 部落格網址為：你的帳號.github.io

第一個方法的話可以參考網址的做法，fork 回來之後請先刪掉原本網頁的 gh-pages，然後再重新創建一個 gh-pages branch (如影片所示)。這邊要注意名字更改的部分也要改動到 base URL。這個做法的話一個帳號下面可以用不同的名稱來開不同的部落格。

第二種方法的話，fork 回來之後也是一樣刪掉原本的 gh-pages 並且放在 master branch 底下即可。這樣 _config.yml 的 baseurl 也不需要更改了。   

# <strong>Test your blog in local

> 在把文章上傳或是對部落格做更動之前，你一定會先想看看效果如何。因此如何在還沒 commit 上 GitHub 之前就看到效果就變得很重要，因此在 local 端測試結果是很重要的功能。

首先先把你的部落格檔案從 GitHub clone 一份到 local.
{% highlight shell %}
$ git clone 你部落格的GitHub網址 你本地的部落格資料夾
{% endhighlight %}

接下來我們得先安裝 Jekyll 與他的相關檔案，因此我們得先增設一個 Gemfile
{% highlight shell %}
$ cd 你本地的部落格資料夾
$ vim Gemfile
{% endhighlight %}

在 Gemfile中請輸入以下資料
{% highlight shell %}
source 'https://rubygems.org'
gem 'github-pages', group: :jekyll_plugins
{% endhighlight %}

完成後記得儲存然後回到本地部落格的資料夾，然後開始安裝。
{% highlight shell %}
$ bundle install
{% endhighlight %}

最後在本地部落格的資料夾執行Jekyll吧！
{% highlight shell %}
$ bundle exec jekyll serve
{% endhighlight %}

之後開啟你的瀏覽器輸入`http://127.0.0.1:4000/`就能看到本地端的結果了。  

# <strong>Commit your changes

> 因為我是 Git 新手，特別紀錄一下怎麼把我在本地更改的資料夾 commit 上去。

這邊用的都是很簡單的基本指令，若是要使用更多其他指令請上網查詢 Git，教學資源非常豐富。
因為我上面已經從我的 Git Reposity clone 了一份下來，所以這邊不用再重新設定要 commit 到的地方。

簡單的把所有更改都加進來
{% highlight shell %}
$ git add .
{% endhighlight %}
或者可以指定一些檔案
{% highlight shell %}
$ git add file_name
{% endhighlight %}

創建一個 commit 與 log
{% highlight shell %}
$ git commit -m "this is a log"
{% endhighlight %}

push 到你在 GitHub 的 reposity
{% highlight shell %}
$ git push -u origin master
{% endhighlight %}
我目前是很簡單的 push 而已
{% highlight shell %}
$ git push
{% endhighlight %}  

另外，如果想查詢現在的狀態可以使用下列指令
{% highlight shell %}
$ git status
{% endhighlight %}  

# <strong>Fix URL for the post

我在使用 Lanyon 時遇到了一些 Post URL 的問題，點文章標題卻都無法連結到文章。
解法是在部落格 root 資料夾中的 index.html，把 baseurl 跟 post.url 中間的 / 拿掉。
<br/>   
![alt text](/assets/posts/2017-02-05-howtosetupblog/url_adjust.png "adjust the url")

# <strong>Add Read more for each post

現在網頁的一切都就緒了，運作起來也很正常，但是一回到主頁我就呆掉了。為什麼每一篇文章在我的主頁都直接把內容完全顯示出來？以前的網誌不是都會有繼續閱讀的超連結嗎？
為此，我開始了我的 google 之旅，找到了使用官方用法的[這篇](http://www.cnblogs.com/coderzh/p/jekyll-readmore.html)，然後再加上一些自己的 code。
<br/>  
首先，在 _config.yml 檔案中的 setup 中加入你想使用的分隔字串(這個字串會在md中被你使用)
{% highlight shell %}
excerpt_separator:  '<!-- more -->'
{% endhighlight %}

設定好我們所要使用的分隔字串之後，我們接著去 index.html 修改首頁的顯示，一般來說我們看到的程式碼會顯示整個網誌的內容：
{% highlight html %}{% raw %}
<span class="post-date">{{ post.date | date_to_string }}
</span>
{{ post.content }}    
{% endraw %}{% endhighlight %}

我們可以看到第二行的 post.content 就是將我們這篇網誌內容全部顯示出來的程式碼。
為了讓首頁只顯示我們想顯示的內容，其他的都隱藏在繼續閱讀之後，我們需要更改 index.html 的內容。
{% highlight html %}{% raw %}
<span class="post-date">{{ post.date | date_to_string }}
</span>
{{ post.excerpt }}

<a href="{{ site.baseurl }}{{ post.url }}">
Read More ...
</a>
{% endraw %}{% endhighlight %}

最後，我們只需要在我們網誌的 .md 檔案中加入`<!-- more -->`，在這之後的內容就不會在我們的首頁中顯示，而是得靠按下 Read More 後才能看到所有內容。 

# <strong>Tips
- 在 .md 中想要加上區塊間的距離的話，可以使用換行符號`<br/>` ，只是記得後面得多兩個空白才會生效。
- 不知道怎麼實作某些東西時，請去參考別人的 GitHub Pages 的 source code，這些程式碼都是開放的。多看別人的 code 也能慢慢理解整個 Jekyll 是怎麼使用與運作的。

# <strong>Finally, my words

這篇網誌寫起來有點辛苦，因為已經很久沒有打這麼多國字，也很久沒有用文字敘述一件事情。
希望這篇文章不只能幫我記錄整個過程，也能幫助一些跟我一樣想快速弄出一個 Blog 又還沒想完全搞懂 Jekyll 怎麼運作的朋友們。

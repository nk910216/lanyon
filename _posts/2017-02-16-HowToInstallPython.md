---
layout: post
title: 如何在 mac 上面同時安裝 python2 和 python3
---

# <strong>Reference

Mac 在安裝上實在是非常便利又簡單，這次我參考的是下面這篇文章，寫得簡單又精美，非常感謝作者！
- [Mac OSX 正確地同時安裝 Python 2.7 和 Python3](https://stringpiggy.hpd.io/mac-osx-python3-dual-install/)

# <strong>Prepare

第一步，我們先安裝一個好用的套件管理工具 [HowBrew](https://brew.sh/index_zh-tw.html)
官網中提到我們只需要在 terminal 中貼上以下指令即可
{% highlight shell %}
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
{% endhighlight %}

安裝之後檢查一下沒有裝好
{% highlight shell %}
$ brew doctor
{% endhighlight %}
只要出現 `Your system is ready to brew.` 這樣的字眼就是安裝好囉。
 
利用這個方便的套件管理工具，我們接下來就要再其之下安裝 python。

# <strong>Install Python

接下來就是要裝python了，首先我們可以看一下macOS原本就有的預設 python
{% highlight shell %}
$ python --version
{% endhighlight %}
在我的電腦上，使用的是 `Python 2.7.13`

我們現在要做的是利用 homebrew 來裝與系統預設分開的套件，如此一來就算我們把套件搞爛了，也不會影響到系統原生的 python。
{% highlight shell %}
$ brew install python
$ brew install python3
{% endhighlight %}

這兩條指令分別安裝的是 python2 與 python3。
安裝結束之後，我們在 `/usr/local/Cellar` 可以找到一個 python 和一個 python3 的資料夾。

# <strong>Adjust the Environment Path

當我們打開 terminal 要使用某支程式的時候，如果只輸入程式名稱(不包含路徑)，系統怎麼知道我們把程式放在哪裡呢？像是上面的 `$ python --version` ，系統怎麼知道要去哪裡找 python 這支程式呢？
答案是參考系統中的環境變數( Environment Path )中儲存的路徑名稱依序一個一個去搜尋，在該路徑下面第一個找到的程式就是我們的目標。

首先我們先來看看我們的環境變數長成什麼樣子
{% highlight shell %}
$ echo $PATH
{% endhighlight %}
輸入後我得到的結果如下：
{% highlight shell %}
$ /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
{% endhighlight %}

其中冒號是分隔兩個路徑的意思。前面提到說系統會**依照順序**一個一個去找整個資料夾，直到跑完環境變數給的所有資料夾或是找到目標為止。
因此，為了要讓系統優先叫用我們安裝在 homebrew 底下的 python，請將 `/usr/local/bin` 移到 `/usr/bin` 之前，前面的是我們資料夾會存放我們使用 homebrew 安裝的 python binary 檔。而後面的則是存放系統預設安裝的 binary 檔案。
在我的例子中，原本的順序就挺不錯了，因此我就不需要去更改他。更改完之後請重新啟動 terminal，然後再次查看環境變數有沒有被正確地更改。

# <strong>Finish

基本上到此為止我們已經完成 python 2 跟 python 3 的安裝了，可以查看一下他們各自的版號。
利用 `which` 來查看他們的路徑。
{% highlight shell %}
$ which python
/usr/local/bin/python
{% endhighlight %}

最後，如果真的要使用系統預設的 python 的話，就直接輸入路徑吧。
{% highlight shell %}
$ /usr/bin/python
{% endhighlight %}

# <strong>Thanks

這篇只是我自己的一個紀錄，跟我上面參考的網誌基本上一模一樣。在此非常感謝原作者分享這麼棒的文章。




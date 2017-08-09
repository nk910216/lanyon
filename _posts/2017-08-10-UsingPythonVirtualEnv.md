---
layout: post
title: Python Virtual Environment
---

> 這篇主要的目的是記錄如何將 python 開發環境保持簡單，乾淨與彈性。

用到的工具是 **VirtualEnv** 和 **VirtualEnvWrapper**.

<!-- more -->  


[參考這篇文章](http://mkelsey.com/2013/04/30/how-i-setup-virtualenv-and-virtualenvwrapper-on-my-mac/)

[輔助文章](https://github.com/registerguard/registerguard.github.io/wiki/Install-python,-virtualenv,-virtualenvwrapper-in-a-brew-environment)

我自己是用 python3 去安裝，而我的 python3 是用 homebrew 安裝的，所以路徑變得很重要。
**要注意的是 Ubuntu 在使用 pip 時記得要使用 sudo。**

~~~ bash
$ pip3 install virtualenv
$ pip3 install virtualenvwrapper
~~~

安裝之後可以下下指定檢查一下有沒有安裝好

~~~ bash
$ pip3 list virtualenv
$ pip3 list virtualenvwrapper
~~~

如果沒問題的話，接下來需要對 virtualenv 環境做一點設定。  
說到環境變數，當然是要去更改 **.profile** 檔案囉！(在 Mac 底下是如此，在其他環境的話有可能是：.bash_profile)  
在 export PATH 底下加入以下的文字：

~~~ bash
# python usage
# set where virutal environments will live
export WORKON_HOME=$HOME/.virtualenvs
export VIRTUALENVWRAPPER_PYTHON=/usr/local/bin/python3
# ensure all new environments are isolated from the site-packages directory
export VIRTUALENVWRAPPER_VIRTUALENV_ARGS='--no-site-packages'
# use the same directory for virtualenvs as virtualenvwrapper
export PIP_VIRTUALENV_BASE=$WORKON_HOME
# makes pip detect an active virtualenv and install to it
export PIP_RESPECT_VIRTUALENV=true
if [[ -r /usr/local/bin/virtualenvwrapper.sh ]]; then
    source /usr/local/bin/virtualenvwrapper.sh
else
    echo "WARNING: Can't find virtualenvwrapper.sh"
fi
~~~

我也是複製別人的 code 然後做一些小修改。  
首先，WORKON\_HOME 變數代表我們之後創建的 virtual environment 資料存放的地方。  
**這邊有個地方需要注意，我們需要先建立這個資料夾。**

~~~ bash
$ cd
$ mkdir .virtualenvs
~~~

我遇到一點小問題的是 **VIRTUALENVWRAPPER\_PYTHON**，這個變數需要設定成你常用的 python 路徑，因為之後在使用 mkvirtualenv 時，他會預設使用這個路徑的 python。  

需要查詢 python 路徑的話 (Linux 的話可以用 **whereis**)：

~~~ bash
$ which python
/usr/bin/python
$ which python3
/usr/local/bin/python3
~~~

兩個的路徑對我來說有點不同，因為 python 我使用 OS X 預設的，而 python3 則是使用我用 homebrew 安裝的。  
因為我是先學 python3，所以這邊就使用 python3 的路徑。

不過如果你在創建虛擬環境時想使用 python2 也是很 ok 的，只要在創建時使用 **--python=python** 即可，因為這邊的 python 直接連結到的是 /usr/bin/python

~~~ bash
$ mkvirtualenv --python=python testenv
~~~

其他創建的指令就從上面的網頁中看看吧，基本上把常用的用過一下就好。  


## **Reference**
- [virtualenvwrapper 官方文件](http://virtualenvwrapper.readthedocs.io/en/latest/install.html)
- [How I Setup VirtualEnv and VirtualEnvWrapper on My Mac](http://mkelsey.com/2013/04/30/how-i-setup-virtualenv-and-virtualenvwrapper-on-my-mac/)
- [Install python, virtualenv, virtualenvwrapper in a brew environment](https://github.com/registerguard/registerguard.github.io/wiki/Install-python,-virtualenv,-virtualenvwrapper-in-a-brew-environment)
- [Stack Overflow 上解決 module 找不到的問題](https://stackoverflow.com/questions/23997403/installed-virtualenv-and-virtualenvwrapper-python-says-no-module-named-virtuale)
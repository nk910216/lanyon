---
layout: post
title: 為 GitHub 上的 Jekyll 添加 Tags
tags: Blog
---

> 因為 Jekyll 架在 GitHub 上的限制比較多，所以參考別人的 Blog 用很土法煉鋼的方式弄了一個 Tag。雖然不是很方便精緻，但是對現在的我來說夠用了。有機會的話未來網頁知識更豐富之後再回來好好了解一番。

因為弄這個 Blog 只是想紀錄一下自己做過的東西，未來有個紀錄好查詢。因此界面與功能一開始就沒打算弄太繁複，快速方便就好。

所以留言回覆功能可能很久以後再回頭來弄了。至於 Tag 可以幫我把文章分門別類，搜尋起來整齊又一目了然，想了想還是弄個簡單的方法來實現基本功能吧。

<!-- more -->  

又要再度靠別人了，[這篇](https://codinfox.github.io/dev/2015/03/06/use-tags-and-categories-in-your-jekyll-based-github-pages/)寫的滿簡單又好實現，還有他自己的現成網誌可以參考。

# **Generate a Tag page**

首先，我們會需要一個網頁可以將我們的 Tag 分門別類並列出相關文章。所以得先在放 page 的地方產生一個我稱為 **Tags.md** 的檔案。這個網頁做的事情就是暴力去搜你的 post 中的 tag, 然後顯示出來。

~~~html
{%raw%}
<div class="tags-expo">
  <div class="tags-expo-list">
    {% for tag in site.tags %}
    <a href="#{{ tag[0] | slugify }}" class="post-tag">{{ tag[0] }}</a>
    {% endfor %}
  </div>
  <hr/>
  <div class="tags-expo-section">
    {% for tag in site.tags %}
    <h2 id="{{ tag[0] | slugify }}">{{ tag[0] }}</h2>
    <ul class="tags-expo-posts">
      {% for post in tag[1] %}
        <a class="post-title" href="{{ site.baseurl }}{{ post.url }}">
      <li>
        {{ post.title }}
      <small class="post-date">{{ post.date | date_to_string }}</small>
      </li>
      </a>
      {% endfor %}
    </ul>
    {% endfor %}
  </div>
</div>
{%endraw%}
~~~

# **Add metadata to your post : tags**

接下來就可以在你的每一個 post 中加入 tags 囉！增加的方法很簡單，以這個網頁為例子：

~~~ txt
---
layout: post
title: 為 GitHub 上的 Jekyll 添加 Tags
tags: Blog
---
~~~

如果要增加多個 tag 的話:

~~~ txt
---
layout: post
title: 為 GitHub 上的 Jekyll 添加 Tags
tags: [tag0, tag1, tag2]
---
~~~

# **Add tags and its link for every post**

接著我們會希望在每一個文章的開頭(或是結尾)的地方知道這篇文章的 tag 是甚麼，也希望可以藉此連結到那個 tag 的頁面繼續訪問相關議題。因此可以在 post.html 中加入一些更改：

~~~ html
{%raw%}
<div class="post">
    <h1 class="post-title">{{ page.title }}</h1>
    <span class="post-date">{{ page.date | date_to_string }}</span>
    {% if page.tags %}
      {% for tag in page.tags %}
      <a href="{{ site.baseurl }}{{ site.tag_page }}#{{ tag | slugify }}" class="post-tag">
          {{ tag }} 
      </a>
      {% endfor %}
    {% endif %}
    <br/><br/>
    {{ content }}
</div>
{%endraw%}
~~~

這樣你每一篇文章就會自動加入 tag 的相關資訊跟連結了。

依此類推，也可以加在 index.html 上，在還沒點繼續閱讀之前就能知道這篇文章的 tag 是甚麼了！

# **Make it more beautiful**

我不太懂 CSS，所以我這邊直接複製別人的來使用。
我更改了 public/css/lanyon.css 這個檔案，在 post-data 下面增加了：

~~~ txt
.post-tag {
  display: inline-block;
  background: #268bd2;
  padding: 0 .5rem;
  margin-right: .5rem;
  border-radius: 4px;
  color: #ffffff;
  font-family: $font-sans;
  font-size: 90%;
  &:before {
    content: "\f02b";
    font-family: "PT Sans", Helvetica, Arial, sans-serif;
    padding-right: .5em;
  }
  &:hover {
    text-decoration: none;
    background: #268bd2;
    color: $white;
  }
  @include transition(all .1s ease-in-out);
}
~~~

效果還不錯，是我喜歡的藍色。

# **Reference**

- [Use Tags and Categories in your Jekyll based Github Pages without plugins](https://codinfox.github.io/dev/2015/03/06/use-tags-and-categories-in-your-jekyll-based-github-pages/)

---
layout:  post
title:  为jekyll静态博客添加搜索功能
subtitle: jekyll search 
date: 2018-02-24
author: ivo
catalog: true
header-img:
tags:
    - jekyll
    - search
---
我用了一个[Hux](https://github.com/Huxpro/huxpro.github.io) 的模板做jekyll的博客,体验还是不错来的,美中不足的是里面没有搜索功能所以,在网上搜索相关的资料,最后找到了这个[simple-jekyll-search](https://github.com/christian-fei/Simple-Jekyll-Search),但是它是需要用nmp或gem来安装的,最后查看他的示例页面以及在alin的帮助下,成功的将这个功能一直过来到了我自己的博客上面.

### 新建一个json在博客的根目录 

去上面的项目里面自己复制创建,因为解析的原因,我的博客模板已经自己解析了json.地址
[去看readme创建search.json](https://github.com/christian-fei/Simple-Jekyll-Search/blob/master/README.md)
### 复制js

复制上面的项目里面的dest目录下面的simple-jekyll-search.min.js,复制到网站根目录的js文件夹里面.

### 修改页面文件

因为使用了hux的模板,所以不能按照它官方的方式在 `_layouts/default.html` 中做修改.这里我们修改的是`_includes/nav.html`.这个需要懂一些前端的代码来盘算放在哪里.我们这里用的浮动窗口理论上可以放在任意位置,放着里是因为当初实现其来没有使用浮动的窗口有另外的思路,后面没有更改.
具体代码如下
这部分是有效的代码

```
<!-- HTML elements for search-->
<div id="search-container" style="float:right;position: fixed;right:0px; bottom:10px; z-index:999999;background:#eeeeee;padding:10px 10px 0px 10px;">
  <input type="text" id="search-input" placeholder="search..." style="border:2px solid;border-radius:25px;padding-left:10px !important;" >
  <ul id="results-container" style="max-width:300px;"></ul>
</div>

<!-- script pointing to jekyll-search.js-->
<script src="{{ site.baseurl }}/js/simple-jekyll-search.min.js"></script>

 <script>
      window.simpleJekyllSearch = new SimpleJekyllSearch({
        searchInput: document.getElementById('search-input'),
        resultsContainer: document.getElementById('results-container'),
        json: '{{ site.baseurl }}/search.json',
        searchResultTemplate: '<li><a href="{url}" title="{desc}">{title}</a></li>',
        noResultsText: 'No results found',
        limit: 5,
        fuzzy: false,
        exclude: ['Welcome']
      })
    </script>

```

下面是nav.html
```
<!-- Navigation -->
<nav class="navbar navbar-default navbar-custom navbar-fixed-top">
    <div class="container-fluid">
        <!-- Brand and toggle get grouped for better mobile display -->
        <div class="navbar-header page-scroll">
            <button type="button" class="navbar-toggle">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="{{ site.baseurl }}/">{{ site.title }}</a>
        </div>

        <!-- Collect the nav links, forms, and other content for toggling -->
        <div id="huxblog_navbar">
            <div class="navbar-collapse">
                <ul class="nav navbar-nav navbar-right">
                    <li>
                        <a href="{{ site.baseurl }}/">Home</a>
                    </li>
                    {% for page in site.pages %}{% if page.title %}
                    <li>
                        <a href="{{ page.url | prepend: site.baseurl }}">{{ page.title }}</a>
                    </li>
                    {% endif %}{% endfor %}
                </ul>
            </div>
        </div>
        <!-- /.navbar-collapse -->
    </div>
    <!-- /.container -->

</nav>

<!-- HTML elements for search-->
<div id="search-container" style="float:right;position: fixed;right:0px; bottom:10px; z-index:999999;background:#eeeeee;padding:10px 10px 0px 10px;">
  <input type="text" id="search-input" placeholder="search..." style="border:2px solid;border-radius:25px;padding-left:10px !important;" >
  <ul id="results-container" style="max-width:300px;"></ul>
</div>

<!-- script pointing to jekyll-search.js-->
<script src="{{ site.baseurl }}/js/simple-jekyll-search.min.js"></script>

 <script>
      window.simpleJekyllSearch = new SimpleJekyllSearch({
        searchInput: document.getElementById('search-input'),
        resultsContainer: document.getElementById('results-container'),
        json: '{{ site.baseurl }}/search.json',
        searchResultTemplate: '<li><a href="{url}" title="{desc}">{title}</a></li>',
        noResultsText: 'No results found',
        limit: 5,
        fuzzy: false,
        exclude: ['Welcome']
      })
    </script>

<script>
    // Drop Bootstarp low-performance Navbar
    // Use customize navbar with high-quality material design animation
    // in high-perf jank-free CSS3 implementation
    var $body   = document.body;
    var $toggle = document.querySelector('.navbar-toggle');
    var $navbar = document.querySelector('#huxblog_navbar');
    var $collapse = document.querySelector('.navbar-collapse');

    var __HuxNav__ = {
        close: function(){
            $navbar.className = " ";
            // wait until animation end.
            setTimeout(function(){
                // prevent frequently toggle
                if($navbar.className.indexOf('in') < 0) {
                    $collapse.style.height = "0px"
                }
            },400)
        },
        open: function(){
            $collapse.style.height = "auto"
            $navbar.className += " in";
        }
    }

    // Bind Event
    $toggle.addEventListener('click', function(e){
        if ($navbar.className.indexOf('in') > 0) {
            __HuxNav__.close()
        }else{
            __HuxNav__.open()
        }
    })

    /**
     * Since Fastclick is used to delegate 'touchstart' globally
     * to hack 300ms delay in iOS by performing a fake 'click',
     * Using 'e.stopPropagation' to stop 'touchstart' event from
     * $toggle/$collapse will break global delegation.
     *
     * Instead, we use a 'e.target' filter to prevent handler
     * added to document close HuxNav.
     *
     * Also, we use 'click' instead of 'touchstart' as compromise
     */
    document.addEventListener('click', function(e){
        if(e.target == $toggle) return;
        if(e.target.className == 'icon-bar') return;
        __HuxNav__.close();
    })
</script>
```


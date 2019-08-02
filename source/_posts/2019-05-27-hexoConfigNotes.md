---
title: hexo博客配置笔记
tags: 博客优化
date: 2019-05-27 17:04:33
categories: 博客
---

主要是记录一些有用的东西, 方便日后查找.建站是hexo+github+next主题.  

## 站点配置

### 站点配置文件  

在Hexo中有两份主要的配置文件，其名称都是\_config.yml. 其中，一份位于站点根目录下，主要包含 Hexo 本身的配置；另一份位于主题目录下，这份配置由主题作者提供，主要用于配置主题相关的选项.  

#### 站点配置部分

站点配置主要包含以下几个内容:  

- 站点基本信息:配置自己网站的名字,副标题, 描述, 作者,语言,时区
- URL: 配置自己站点域名等信息
- 站点目录
- 写作: 新文章的命名, 默认类别等
- 主页: 主页显示的文章数目, 排序方式
- 类别和标签设置
- 时间格式设置
- 主题、插件
- 站点部署

#### 主题配置部分

[安装next主题](https://theme-next.org/docs/getting-started/)之后, 可通过修改主题配置文件`themes\next\_config.yml`里面的配置来diy自己的博客站点.配置主要包含以下几个内容:  

- 站点信息: 网站浏览器图标、站点页脚、rss订阅、版权信息、github图标
- SEO设置: 一些搜索引擎搜索流量相关的设置
- 菜单设置: 设置管理自己的首页显示的菜单
- 主题选择
- 边栏设置:添加社交联系方式、友方博客链接、头像设置、文章目录、回到文章开头、chat
- 文章设置: 对齐、滚动查看、摘要设置、readmore、文章展示信息、计数、代码块、微信订阅、打赏、相关博客、编辑
- Misc 主题设置
- 字体设置
- 第三方服务设置
- 评论设置
- 内容分享
- 数据统计分析
- 站内搜索
- 聊天
- 标签设置
- 动画效果设置  

### 首页显示内容  

1. 修改博客文章的开头Front-matter中的description字段, 填写相关内容. 如果字段内容非空, 则作为摘要显示在首页.  
2. 通过在文章中插入`<!--more-->`来标记, 标记之前的内容作为摘要显示在首页.  
3. 修改主题配置文件`themes\next\_config.yml`  

```yml themes\next\_config.yml
auto_excerpt:
  enable: true  # 开启自动摘要提取
  length: 150
```

说明: 第3种方式开启可以作为默认的方式, 第1、2种可以为博客定制, 必要的时候可以设置使得自己的博客展现得更加完美.  

### 添加社交链接  

修改主题配置文件`themes\next\_config.yml`, 在`social`里面添加自己的账号并设置图标, 并将`social_icons`里的`enable` 设true.  

```yml themes\next\_config.yml
social:
  GitHub: https://github.com/yourname || github
  E-Mail: mailto:yourname@gmail.com || envelope
social_icons:  
  enable: true  # 显示社交软件图标
```

### 搜索引擎  

本节参考[Hexo 搭建个人博客系列：部署上线篇-Yearito's Blog](http://yearito.cn/posts/hexo-deploy-to-VPS.html).  
搭建自己的私人博客, 一部分是为了记录方便, 一部分是为了分享, 这就需要将个人网站提交给搜索引擎收录.针对不同搜索引擎需要分别提交网址,但是提交的步骤都大同小异:  
**第一步：提交网站域名**  

注意需要区分输入 http 与 https。
**第二步：验证网站所有权**  

一般都包括以下三种验证方法，其本质都是通过一段字符串验证码来验证用户的网站所有权，三种方法任选其一：

- HTML文件验证：将给定的HTML文件上传到站点根目录下（验证码包含在HTML文件中）
- HTML元标签验证：在站点首页中添加给定的 `meta` 标签（验证码包含在 `meta` 标签属性中）
- CNAME验证：添加一个CNAME域名解析记录到指定站点（验证码包含在二级域名中）

Next主题中已经内置集成了各大搜索引擎的HTML元标签验证方案，用户只需获取验证码填写到主题配置文件中并重新打包部署即可完成身份验证。  
**第三步：推送或者提交 sitemap**  

sitemap，又称站点地图，通常是一个xml格式的文件，最早由谷歌提出，现已被多数引擎所支持。里面包含了站点内的页面列表，帮助搜索引擎理解网站内容的组织架构。

在Hexo中可以通过 [hexo-generator-sitemap](https://github.com/hexojs/hexo-generator-sitemap) 和 [hexo-generator-baidu-sitemap](https://github.com/coneycode/hexo-generator-baidu-sitemap) 两个插件帮助生成通用站点地图和百度专用站点地图文件。

在站点根目录下执行以下命令安装相关依赖：

```bash
npm install hexo-generator-sitemap --save-dev
npm install hexo-generator-baidu-sitemap --save-dev
```

之后在执行 `hexo generate` 打包后即可在 public 目录下找到 sitemap.xml 和 baidusitemap.xml 两个文件，将该文件提交到搜索引擎站长后台即可帮助搜索引擎分析收录站点内容，各个搜索引擎收录效率不同，可能需要耐心等上几天。

#### 谷歌

在 [Google Search Console](https://search.google.com/search-console) 中提交站点域名，此时会提供几种验证网站所有权的方法，展开 **其他验证方法** 中的 **HTML 标记**，然后将 `meta` 标签的 `content` 属性值复制到主题配置文件中：

``` yaml themes\next\_config.yml
# Google Webmaster tools verification setting
# See: https://www.google.com/webmasters/
google_site_verification: cEGDN99xe2gtAy97He-NH4ihW3Y4GrGQl_xTxp7p3sg
```

执行 `hexo generate deploy` 重新部署站点，此时网站中就已经自动包含了用于验证身份的 `meta` 标签：

``` html
<meta name="google-site-verification" content="u9qDJxSM-SphoS5GXBhunqp1UXBY5H4FT6J1V2LxXqI" />
```

回到 Search Console 页面点击验证按钮，验证成功后将进入控制台，点击左侧 **站点地图** 菜单，在域名后输入 sitemap.xml 并提交，即可添加新的站点地图。

在新版的 Search Console 中只能添加站点地图，却没有删除站点地图的接口，后来在 [Google Search Console 删除站点地图方法](https://www.dear.today/731.html) 一文中发现了解决办法：需要在 [旧版 Search Console](www.google.com/webmasters/tools/sitemap-list) 中才能删除。

#### 百度

在 [百度搜索资源平台](https://ziyuan.baidu.com/site) 中提交站点域名，勾选站点属性，最后一步中同样会要求验证网站的所有权身份，选择 **HTML标签验证**，然后将 `meta` 标签的 `content` 属性值复制到主题配置文件中：：

``` yaml themes\next\_config.yml
#  Webmaster tools verification setting
# See: https://ziyuan.baidu.com/site/
baidu_site_verification: UHED80Nn65
```

执行 `hexo generate deploy` 重新部署站点，此时网站中就已经自动包含了用于验证身份的 `meta` 标签：

``` html
<meta name="baidu-site-verification" content="UHED80Nn65" />
```

回到百度站长管理平台点击完成验证按钮，验证成功后将进入控制台，在左侧 **数据引入** 菜单下点击 **链接提交**，在此可以看到除了sitemap之外还提供了多种推送站点内容的方案：

- 主动推送：通过API接口推送站点内容，实时性较高
- 自动推送：在网页内添加JS脚本，每当页面被访问的时候会将页面url推送给百度，比较被动
- sitemap：填写站点地图文件地址，百度会周期性的抓取其中的内容进行分析收录，收录效率比较低
- 手动提交：手动填写链接地址进行收录

本站采用主动推送和自动推送相结合的形式推送站点内容。

##### 主动推送

Hexo中可以借助 [hexo-baidu-url-submit](https://github.com/huiwang/hexo-baidu-url-submit) 插件快捷实现主动推送，在根目录下安装相关依赖：

```bash
npm install hexo-baidu-url-submit --save
```

在站点配置文件中添加以下代码：

``` yaml _config.yml
# 百度主动推送
baidu_url_submit:
  count: 100  # 提交最新的一个链接
  host: yearito.cn  # 在百度站长平台中注册的域名
  token: xxxxx  # 请注意这是您的秘钥， 所以请不要把博客源代码发布在公众仓库里!
  path: baidu_urls.txt  # 文本文档的地址， 新链接会保存在此文本文档里
```

然后在站点配置文件中修改部署策略：

``` diff _config.yml
  # Deployment
  ## Docs: https://hexo.io/docs/deployment.html
- deploy:
-   type: git
-   repo: git@yearito.cn:~/blog.yearito.git
+ deploy:
+ - type: git
+   repo: git@yearito.cn:~/blog.yearito.git
+ - type: baidu_url_submitter # 百度
```

每次部署的时候将会自动推送网站内容到百度。

##### 自动推送

Next主题中内置了一键开启百度自动推送的选项：

``` yaml themes\next\_config.yml
# Enable baidu push so that the blog will push the url to baidu automatically which is very helpful for SEO
baidu_push: true
```

开启后将会自动在页面中添加如下脚本用于百度推送：

``` html
<script>
(function(){
  var bp = document.createElement('script');
  var curProtocol = window.location.protocol.split(':')[0];
  if (curProtocol === 'https') {
    bp.src = 'https://zz.bdstatic.com/linksubmit/push.js';
  }
  else {
    bp.src = 'http://push.zhanzhang.baidu.com/push.js';
  }
  var s = document.getElementsByTagName("script")[0];
  s.parentNode.insertBefore(bp, s);
})();
</script>
```

每次访问站点页面都会通过以上脚本推送本页url到百度。

#### 必应

在 [必应网站管理员](https://www.bing.com/toolbox/webmaster) 中提交站点域名，此时可以同时输入 sitemap 文件链接，然后同样会进入网站所有权验证页面，在选择二中复制 `meta` 标签的 `content` 属性值到主题配置文件中：

``` yaml themes\next\_config.yml
# Bing Webmaster tools verification setting
# See: https://www.bing.com/webmaster/
bing_site_verification: 538CCB61234CB4A0B1A71C4581705DCD
```

执行 `hexo generate deploy` 重新部署站点，此时网站中就已经自动包含了用于验证身份的 `meta` 标签：

``` html
<meta name="msvalidate.01" content="538CCB61234CB4A0B1A71C4581705DCD" />
```

回到网站管理员页面点击验证按钮，验证成功后将进入控制台，可以在左侧导航菜单中点击 **仪表板** -> **配置我的网站** -> **Sitemaps** 查看已提交的站点地图文件的解析抓取情况。

### SEO

本节参考[Hexo 搭建个人博客系列：部署上线篇-Yearito's Blog](http://yearito.cn/posts/hexo-deploy-to-VPS.html).
SEO（Search Engine Optimization）意指搜索引擎优化，可以帮助提高目标网站在搜索引擎中的排名，使得在别人搜索相关内容的时候更容易脱颖而出被人所发现，以提高个人站点的存在价值。

在Hexo中可以从以下几个方面进行简单的优化：

- **开启Next主题内置seo优化**

  在主题配置文件中设置 `seo: true`，将会自动部署Next内置的seo优化方案，比如在 themes\\next\\layout\\\_partials\\footer.swig 为链接添加 `rel="external nofollow"` 属性。

- **修改文章url地址格式**

  默认的文章url地址为 `http://yoursite.com/:year/:month/:day/:title/`，这种url格式层级太多，并且如果文章标题是中文的话可能会发生转义而出现一堆乱码，不利于搜索引擎的爬取分析，因此建议在站点配置中修改 `permalink` 的格式来简化页面url，并尽量采用英文命名Markdown文件。

## 文章撰写  

本块内容主要参考`Yearito`的博客[Hexo 搭建个人博客系列：写作技巧篇](http://yearito.cn/posts/hexo-writing-skills.html)  

### 布局(layout)  

布局可以理解为文档的类别或者格式, 不同的类别对应不同的存储位置, 也有不同的作用.  
Hexo 默认有三种布局:`post`、`page`、`draft`,用户可以在`scaffolds`目录下新建文档来自定义布局的模板, 修改站点配置文件中的`default_layout`来指定`hexo new`生成文档的默认布局.  

| 布局  | 路径           | 作用                                                      |
| ----- | -------------- | --------------------------------------------------------- |
| posts | source/_posts  | 目录下的内容作为博客正文显示在网站中                      |
| page  | source         | 用于生成类似**首页**、**归档**这样的页面                  |
| draft | source/_drafts | 草稿文件, 不在网站中显示, 通过`hexo publish`可以转为posts |

### 博客添加菜单  

菜单(menu)就是那个主页上显示的导航栏上面的按钮, 默认只有`首页(home)`和`归档(archives)`, 这个可以设置添加更多链接. 首先通过修改主题配置文件中的`menu`字段可以添加新的菜单选项, 其次通过新建`page`布局来生成对应的链接.  

#### 修改主题配置文件添加菜单选项  

``` diff themes\next\_config.yml
menu:
  home: / || home
  archives: /archives/ || archive
+ about: /about/ || user
+ tags: /tags/ || tags
+ categories: /categories/ || th
```

其中，`||` 之前的值表示菜单链接，之后的值表示所用的 [FontAwesome](https://fontawesome.com) 图标名称.
完成这一步, 就可以看到主页上面多了对应的菜单选项, 但是点击却跳转到错误界面, **只是开放了页面入口, 没有建立对应链接的页面**. 所以, 下一步就是生成内容界面.  

#### 新建page生成对应网页  

```bash
hexo new page <title>
```

通过上诉命令会在在 `source` 目录下新建一个 `<title>` 文件夹，并在该文件夹下创建一个 `index.md` 文件，编辑该文件即可修改页面内容.  
例如，通过 `hexo new page tags` 命令将会生成如下目录.  

```bash
.
└──  source  
  ├── _posts  
  └── tags  
    └── index.md  
```

**注意:** 对`index.md`的修改分两种情况:  

- 情况一: 在文档开头(Front Matter) 指定`type`字段  
  已知`type` 可取 `tages`和`categories`两个值. 这种情况下, 页面会显示便签或者分类的归档, 并且页面不会显示其它内容. 也就是`index.md`正文部分即使写了也不会显示.  
- 情况二: 不指定`type`的自定义页面  
  这种情况下, `index.md`会和其它文章一样显示.  

### Hexo 内置标签插件实现复杂功能  

#### 文本居中标签  

一共有三种方式实现文本居中,如下面代码所示:  

```html 文本居中三种方法

<!-- HTML方式: 直接在 Markdown 文件中编写 HTML 来调用 -->
<!-- 其中 class="blockquote-center" 是必须的 -->
<blockquote class="blockquote-center">blah blah blah</blockquote>

<!-- 标签方式 -->
{% centerquote %}blah blah blah{% endcenterquote %}

<!-- 标签别名 -->
{% cq %} blah blah blah {% endcq %}
```

#### note 颜色块标签

通过 note 标签可以为段落添加背景色，语法如下：

```html
{% note [class] %}
文本内容 (支持行内标签)
{% endnote %}
```

支持的class种类包括 `default` `primary` `success` `info` `warning` `danger`，也可以不指定class。  

各种class种类的效果如下：  

{% note primary %}
**primary** note tag
{% endnote %}

{% note success %}
**success** note tag
{% endnote %}

{% note info %}
**info** note tag
{% endnote %}

{% note warning %}
**warning** note tag
{% endnote %}

{% note danger %}
**danger** note tag
{% endnote %}

{% note %}
undefined class note tag
{% endnote %}

更多配置可在主题配置文件中设置, 建议`style`选择`modern`, 默认是`simple`

``` yaml themes\next\_config.yml
note:
  # Note 标签样式预设
  style: modern  # simple | modern | flat | disabled
  icons: false  # 是否显示图标
  border_radius: 3  # 圆角半径
  light_bg_offset: 0  # 默认背景减淡效果，以百分比计算
```

#### label字体背景色标签标签

通过label标签可以为文字添加背景色，语法如下：

```html
{% label [class]@text  %}
```

支持的class种类包括 `default` `primary` `success` `info` `warning` `danger`，默认使用 `default` 作为缺省。

使用示例如下：

``` plain
I heard the echo, {% label default@from the valleys and the heart %}
Open to the lonely soul of {% label info@sickle harvesting %}
Repeat outrightly, but also repeat the well-being of
Eventually {% label warning@swaying in the desert oasis %}
{% label success@I believe %} I am
{% label primary@Born as the bright summer flowers %}
Do not withered undefeated fiery demon rule
Heart rate and breathing to bear {% label danger@the load of the cumbersome %}
Bored
```

{% centerquote %}
I heard the echo, {% label default@from the valleys and the heart %}
Open to the lonely soul of {% label info@sickle harvesting %}
Repeat outrightly, but also repeat the well-being of
Eventually {% label warning@swaying in the desert oasis %}
{% label success@I believe %} I am
{% label primary@Born as the bright summer flowers %}
Do not withered undefeated fiery demon rule
Heart rate and breathing to bear {% label danger@the load of the cumbersome %}
Bored
{% endcenterquote %}

可在主题配置文件中设置 `label: false` 来取消label标签默认CSS样式。  

#### button按钮

通过button标签可以快速添加带有主题样式的按钮，语法如下：

```html
{% button /path/to/url/, text, icon [class], title %}
```

也可简写为：

```html
{% btn /path/to/url/, text, icon [class], title %}
```

其中， 图标ID来源于 [FontAwesome](https://fontawesome.com/v4.7.0/icons/) 。

使用示例如下：

```html
{% btn #, 文本 %}
{% btn #, 文本 & 标题,, 标题 %}
{% btn #, 文本 & 图标, home %}
{% btn #, 文本 & 大图标 (固定宽度), home fa-fw fa-lg %}
```

<p>{% btn #, 文本 %}</p>  
<p>{% btn #, 文本 & 标题,, 标题 %}</p>
<p>{% btn #, 文本 & 图标, home %}</p>
<p>{% btn #, 文本 & 大图标 (固定宽度), home fa-fw fa-lg %}</p>  

#### tab选项卡标签

tab标签用于快速创建tab选项卡，语法如下

``` html
{% tabs [Unique name], [index] %}
  <!-- tab [Tab caption]@[icon] -->
  标签页内容（支持行内标签）
  <!-- endtab -->
{% endtabs %}
```

其中，各参数意义如下：

- Unique name: 全局唯一的Tab名称，将作为各个标签页的id属性前缀
- index: 当前激活的标签页索引，如果未定义则默认选中显示第一个标签页，如果设为-1则默认隐藏所有标签页
- Tab caption: 当前标签页的标题，如果不指定则会以Unique name加上索引作为标题
- icon: 在标签页标题中添加Font awesome图标

使用示例如下：

``` plain
{% tabs Tab标签列表 %}
  <!-- tab 标签页1 -->
    标签页1文本内容
  <!-- endtab -->
  <!-- tab 标签页2 -->
    标签页2文本内容
  <!-- endtab -->
  <!-- tab 标签页3 -->
    标签页3文本内容
  <!-- endtab -->
{% endtabs %}
```

{% tabs Tab标签列表 %}
  <!-- tab 标签页1 -->
    标签页1文本内容
  <!-- endtab -->
  <!-- tab 标签页2 -->
    标签页2文本内容
  <!-- endtab -->
  <!-- tab 标签页3 -->
    标签页3文本内容
  <!-- endtab -->
{% endtabs %}

#### 突破容器宽度限制的图片  

有的时候需要突出显示图片, 因为图片的扩大与容器的偏差可以从视觉上提升图片的吸引力.主要有两种方式:  

- HTML方式：使用这种方式时，为 img 添加属性 class="full-image"即可。
- 标签方式：使用 fullimage 或者 简写 fi， 并传递图片地址、 alt 和 title 属性即可。 属性之间以逗号分隔。

```html
<!-- HTML方式: 直接在 Markdown 文件中编写 HTML 来调用 -->
<!-- 其中 class="full-image" 是必须的 -->
<img src="/image-url" class="full-image" />

<!-- 标签 方式，要求版本在0.4.5或以上 -->
{% fullimage /image-url, alt, title %}

<!-- 别名 -->
{% fi /image-url, alt, title %}

```

#### 引用站内链接

可以通过如下语法引入站内文章的地址或链接：

```html
{% post_path slug %}
{% post_link slug [title] %}
```

其中，`slug` 表示 `_post` 目录下的Markdown文件名。

`post_path` 标签将会渲染为文章的地址，即 `permalink`；而 `post_link` 标签将会渲染为链接，可以通过 `title` 指定链接标题。

如以下标签将会生成{% post_path 2019-05-27-hexo博客技巧笔记 %}

```html
{% post_path 2019-05-27-hexo博客技巧笔记 %}
```

而以下标签则会生成 {% post_link 2019-05-27-hexo博客技巧笔记 链接标题 %}

```html
{% post_link 2019-05-27-hexo博客技巧笔记 链接标题 %}
```

这种站内引用方式比直接使用url引用的形式更为可靠，因为即使修改了 `permalink` 格式，或者修改了文章的路由地址，只要Markdown文件名没有发生改变，引用链接都不会失效。

#### 插入Swig代码

如果需要在页面内插入Swig代码，包括原生HTML代码，JavaScript脚本等，可以通过 raw 标签来禁止Markdown引擎渲染标签内的内容。语法如下：

```html
{% raw %}
content
{% endraw %}
```

该标签通常用于在页面内引入三方脚本实现特殊功能，尤其是当该三方脚本尚无相关hexo插件支持的时候，可以通过写原生Web页面的形式引入脚本并编写实现逻辑。

#### 插入Gist

如果需要在页面内插入Gist上的代码片段时，可以使用如下标签:

```html
{% gist gist_id [filename] %}
```

其中，各参数意义如下：

- gist_id: Gist仓库页面url中最后一段随机字符串
- filename: Gist中的文件名

如果Gist中只有一个文件，可以不用指定filename，也可以通过JavaScript脚本的形式直接引入，如：

```html
<script src="url"></script>
```

如果Gist中有多个文件，可以在标签内输入filename来指定只引入某个文件，如果没有指定filename，将会引入Gist中的所有文件。另外，引用JavaScript脚本形式无法精确控制只引入某一个文件，将会同时引入Gist中的所有文件。

如果指定了与Gist无法匹配的filename，页面上将不会显示任何标签内容。所以，一般在Gist只有一个文件的情况下无需指定filename。

**在页面中引入Gist代码段将会同时从github服务器上下载脚本与CSS样式文件，由于国内访问github服务器延迟较高，往往资源文件连接和下载的速度很慢，会阻塞页面的渲染进程导致短时白屏.**  

### 代码块高阶使用方法  

代码块进阶语法规则：

<div style="background-color: #f7f7f7; margin: 20px 0; padding: 10px;border-radius: 5px; font-family: consolas;">
  &#x60;&#x60;&#x60; [language] [title] [url] [link text]<br>
  code snippet <br>
  &#x60;&#x60;&#x60;
</div>

其中，各参数意义如下：

- langugae：语言名称，引导渲染引擎正确解析并高亮显示关键字
- title：代码块标题，将会显示在左上角
- url：链接地址，如果没有指定link text则会在右上角显示link
- link text：链接名称，指定url后有效，将会显示在右上角

{% note warning %}
注意\`\`\`后面不要有多余的空格!不然可能导致代码块功能失调。
{% endnote %}

url 必须为有效链接地址才会以链接的形式显示在右上角，否则将作为标题显示在左上角。以url为分界，左侧除了第一个单词会被解析为language，其他所有单词都会被解析为title，而右侧的所有单词都会被解析为link text。

**如果不想填写title，可以在language和url之间添加至少三个空格。**  
**tips:** 如果设置语言为 diff，可以在代码前添加 + 和 - 来实现高亮增删行提示效果，在展示代码改动痕迹时比较实用,如下所示:  

```diff _config.yml
 highlight:
   enable: true
   line_number: false
-  auto_detect: false
+  auto_detect: true #开启自动语言检测高亮
```

更多花里胡哨的代码块功能拓展, 可以看这个[HEXO下的语法高亮拓展修改](https://www.ofind.cn/blog/HEXO/HEXO%E4%B8%8B%E7%9A%84%E8%AF%AD%E6%B3%95%E9%AB%98%E4%BA%AE%E6%8B%93%E5%B1%95%E4%BF%AE%E6%94%B9.html#%E8%AE%BE%E7%BD%AE%E4%BB%A3%E7%A0%81%E6%B7%BB%E5%8A%A0%E5%88%A0%E9%99%A4%E6%A0%87%E8%AE%B0)。  

#### 插入图片

#### TODO

- [ ] Hexo-cos-sync 插件开发

使用腾讯云图床COS, 但是发现没有七牛云一样便捷的插件用于上传博客的图片, 拟自己写一个。下面是几个可能用的着的资源:
[一个python写的同步工具](https://github.com/Erimus-Koo/qCloud-COS-Sync)  
[Hexo cos 部署插件](https://github.com/sdlzhd/hexo-deployer-cos)  
[七牛云hexo资源上传插件](https://github.com/gyk001/hexo-qiniu-sync)  
[cos sdk](https://cloud.tencent.com/document/product/436/14113)

### 写在最后  

所有笔记都是来自官方文档和Yearito's Blog, 如果有错误或者想了解更多的内容, 可以参看参考链接。

---  

### 参考链接  

1. [Next document](https://theme-next.org)
2. [Hexo document](https://hexo.io/zh-cn/docs)
3. [Hexo 搭建个人博客系列：基础建站篇-Yearito's Blog](http://yearito.cn/posts/hexo-get-started.html)  
4. [Hexo 搭建个人博客系列：写作技巧篇-Yearito's Blog](http://yearito.cn/posts/hexo-writing-skills.html)  
5. [Hexo 搭建个人博客系列：部署上线篇-Yearito's Blog](http://yearito.cn/posts/hexo-deploy-to-VPS.html)  
6. [Hexo Theme Next Test - 标签的运用](https://almostover.ru/2016-01/hexo-theme-next-test/)  

---
title: github+hexo搭建博客
top: true
cover: false
toc: true
mathjax: true
date: 2020-09-20 05:28:37
password:
summary:
tags:  博客搭建
categories: Blog—Tips
---

---

### 一、前言

#### 	**在博客搭建的过程中查阅了各种网上的文章，多数写的很cd，费尽周折终于搭建了一个看起来还不错的博客，这里记录下踩过的坑。**



### 二、环境安装

首先安装几个必要的环境，Node.js和Hexo命令行工具和git工具。

Node.js：https://nodejs.org/zh-cn/download/

git工具：https://git-scm.com/download/

安装后，在cmd或其他shell环境输入`npm install -g hexo-cli `命令来安装Hexo命令行工具。

然后注册个github，然后创建个Repository ，注意创建时名称要一致，具体步骤自行百度。



### 三、安装Hexo博客框架

首先在本地使用git的bash命令行来输入以下命令创建项目

```
hexo init {name}	 #创建项目
```

这里的{name}是你要间的项目名称，通俗点讲就是本地存放hexo官方框架源码的文件夹，创建完成后，会在本地生成一个相应的文件夹，大致结构为：

```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```



#### 简单的说下文件及文件夹的作用

*_config.yml：	网站配置文件*

*package.json：	应用程序信息*	

*scaffolds：模板文件夹。当您新建文章时，Hexo 会根据 scaffold 来建立文件，Hexo的模板是指在新建的文章文件中默认填充的内容。例如，如果您修改scaffold/post.md中的Front-matter内容，那么每次新建一篇文章时都会包含这个修改。*

*source：资源文件夹，用来存放资源的地方，除 _posts 文件夹之外，开头命名为 _ (下划线)的文件 / 文件夹和隐藏的文件将会被忽略。Markdown 和 HTML 文件会被解析并放到 public 文件夹，而其他文件会被拷贝过去。*

*themes：主题 文件夹。Hexo 会根据主题来生成静态页面。*



接下来，部署到本地，此时可以在浏览器访问localhost:4000来查看部署。

```
hexo g				#生成html代码
hexo s				#启动本地服务
```

启动成功后，说明创建了一个简单的hexo框架，接下来配置。



### 四、配置、部署到github

首先需要安装插件，使用git的bash来安装：

```
npm install hexo-deployer-git --save
```



安装后，我们来项目根目录来修改_config.yml文件：

首先修改个人信息及标签等：

```
# Site
title: CRLF's Blog
subtitle: "临渊羡鱼,不如退而结网,水至清则无鱼,人至察则无徒。"
description: 'Web安全 内网安全 权限提升 代码审计 RedTeam'
keywords:
author: crlf
language: zh-CN
timezone: ''
language: zh-CN
```



添加下列github关联信息到根目录下的 **_config.yml** 文件：

```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: git@github.com:{username}/{username}.github.io.git
  branch: master
```



然后可以先部署到本地试一下，在项目根目录打开bash：

```
hexo g 
hexo s
```



访问成功后，CTRL+C 停止，使用 下面的命令部署到github。

```
hexo d
```

此时就可以访问自己的博客了。



### 五、换主题、美化

这里我是用的是stun的主题，项目地址：https://github.com/liuyib/hexo-theme-stun.git

使用git命令下载：

```
git clone https://github.com/liuyib/hexo-theme-stun.git  /themes/stun
```

下载后会在本地项目的  **themes** 目录下存在stun主题的源码，要想使用这套主题，我们还是需要去项目根目录下（非主题目录下的）**_config.yml**去找到 **tehme** 这个字段，修改为下面的，然后保存，注意中间必须有空格！

```
theme: stun
```

接下来去修改 **themes/stun/__config.yml** 就好了，送上我修改好的，注意头像和背景自己去修改：

```
# ---------------------------------------------------------------
# Theme Core Configuration Settings
# ---------------------------------------------------------------

# Remove unnecessary files after hexo generate.
shake_file: true

# ---------------------------------------------------------------
# SEO Settings
# ---------------------------------------------------------------

# If true, will add site-subtitle to index page.
# Remember to set up your site-subtitle in Hexo `_config.yml`.
index_subtitle: false

# Set a canonical link tag for your Hexo site.
# See: https://support.google.com/webmasters/answer/139066
# Remember to set up your URL in Hexo `_config.yml` (e.g. url: http://yoursite.com)
canonical: true

# Webmaster tools verification setting
# ---------------------------------------------------------------
# Google Webmaster tools verification setting
# See: https://www.google.com/webmasters/
google_site_verification:

# Bing Webmaster tools verification setting
# See: https://www.bing.com/webmaster/
bing_site_verification:

# Baidu Webmaster tools verification setting
# See: https://ziyuan.baidu.com/site/
baidu_site_verification:

# 360 Webmaster tools verification setting
# see http://zhanzhang.so.com/
qihu360_site_verification:

# Sougou Webmaster tools verification setting
# see http://zhanzhang.sogou.com/
sougou_site_verification:

# ---------------------------------------------------------------
# Menu config
# ---------------------------------------------------------------

# The menu in the website header.
# Value before `||` is the target link, value after `||` is the name of FontAwesome icon.
# Usage(With Icon): `Key: /link/ || fa(s|r|l|d|b) fa-iconname`
# Usage(No Icon)  : `Key: /link/`
# Select your icon name, see: https://fontawesome.com/icons
menu:
  home: / || fas fa-home
  archives: /archives/ || fas fa-folder-open
  categories: /categories/ || fas fa-layer-group
  tags: /tags/ || fas fa-tags
  # You can add a secondary menu like follow.
  # xxx1: javascript:; || fa(s|r|l|d|b) fa-xxx

# Secondary menu
submenu:
    article:
    archives: /archives/ || fas fa-folder-open
    categories: /categories/ || fas fa-layer-group
    tags: /tags/ || fas fa-tags
  # Add item of secondary menu in here.
  # xxx1:
  #   xxx1-1: /xxx1-1/ || fa(s|r|l|d|b) fa-xxx
  #   xxx1-2: /xxx1-2/ || fa(s|r|l|d|b) fa-xxx
  

menu_settings:
  # Only show by icon.
  icon_only: false
  # Only show by text.
  text_only: false

# ---------------------------------------------------------------
# Site config
# ---------------------------------------------------------------

favicon:
  small: /images/icons/favicon-16x16.png
  medium: /images/icons/favicon-32x32.png
  # ! --------------------------------------------------
  # ! If you don't know the following, please ignore it.
  # ! --------------------------------------------------
  # apple_touch_icon: /images/icons/apple-touch-icon.png
  # safari_pinned_tab: /images/icons/stun-logo.svg
  # msapplication: /images/icons/favicon-144x144.png

# Progressive Web Apps
# See: https://github.com/lavas-project/hexo-pwa/
pwa:
  enable: true
  manifest: /manifest.json
  # (Please use "" to wrap the value)
  theme_color: "#ffffff"

# Night mode
night_mode:
  enable: true
  # The button of switching to the night mode.
  button:
    # Color of button (Please use "" to wrap the value).
    color: "#fafafa"
    # Background color of button (Please use "" to wrap the value).
    bg_color: "#8c8a8a"
  # The icon that stand for day and night.
  icon:
    dark: 🌜
    light: 🌞

# The layout for sidebar and content of site.
layout:
  # The width of the content area in website.
  content: 760px
  # The width of the sidebar in website.
  sidebar: 280px
  # The width between the content and the sidebar.
  content_sidebar_gap: 30px
  # The padding property of the main tag.
  # e.g. Usage:
  # 20px
  # 20px 30px
  # 20px 20px 30px
  # 20px 30px 40px 25px
  main_padding:
    # Take effect when screen width in: [768px, Infinity)
    default: 20px
    # Take effect when screen width in: [576px, 768px)
    tablet: 15px
    # Take effect when screen width in: [0px, 576px)
    mobile: 10px

# The header of site.
header:
  enable: true
  show_on:
    # Whether to show on the article page.
    post: true
  # If you set a percentage, it will be calculated based on the height of screen (Supports all CSS units)
  height: 80%
  # Background image of site header.
  bg_image:
    enable: true
    # In theme directory (source/images): /images/avatar.png
    # In site directory (source/uploads): /uploads/avatar.png
    # You can also use a link of image.
    url: /images/icons/stun-header.jpg
  # Mask effect of the background image.
  mask:
    enable: true
    # Opacity of the mask (value: 0 ~ 1)
    opacity: 0.5
  nav:
    # Height of the navigation bar (Supports all CSS units)
    height: 50px
    # Background color of the navigation bar (Please use "" to wrap the value).
    bg_color: "#333"
  # The icon that prompts the user to pull down.
  scroll_down_icon:
    enable: true
    # Icon name in FontAwesome, see: https://fontawesome.com/icons
    name: fas fa-angle-down
    animation: true

# The body of site.
body:
  # Background image of site body.
  bg_image:
    enable: false
    # In theme directory (source/images): /images/avatar.png
    # In site directory (source/uploads): /uploads/avatar.png
    # You can also use a link of image.
    url:
    # Whether to fixed the background image.
    fixed: true
    # Whether to repeat the image as much as possible to cover the entire area.
    repeat: false
  # Mask effect of the background image.
  mask:
    enable: false
    # Opacity of mask (value: 0 ~ 1)
    opacity:
      # Opacity of the mask by default.
      default: 0.1
      # Opacity of the mask in night mode.
      night_mode: 0.6

# The footer of site.
footer:
  # Background image of site footer.
  bg_image:
    enable: false
    # In theme directory (source/images): /images/avatar.png
    # In site directory (source/uploads): /uploads/avatar.png
    # You can also use a link of image.
    url:
  # Mask effect of the background image.
  mask:
    enable: false
    # Opacity of mask (value: 0 ~ 1)
    opacity: 0.5
  # Copyright information.
  copyright:
    enable: true
    # If not set, will be used `author` from Hexo main config (e.g. liuyib All Rights Reserved)
    text: <a href="https://github.com/crlf01" rel="noopener" target="_blank">crlf</a>
    # Start time. If not set, the current year will be used.
    since: 2020
    # End time. If not set, the current year will be used.
    end:
  # The icon between the copyright and the owner.
  icon:
    enable: true
    # Icon name in FontAwesome, see: https://fontawesome.com/icons
    name: fas fa-heart
    # Heartbeat animation.
    animation: true
    # (Please use "" to wrap the value)
    color: "#ff0000"
  # Hexo link
  powered:
    enable: true
    # Whether to show version after Hexo link (e.g. vX.X.X)
    version: true
  # Theme link
  theme:
    enable: true
    # Whether to show version after theme link (e.g. vX.X.X)
    version: true
  # Beian information for Chinese users, see: http://www.miitbeian.gov.cn/
  beian:
    enable: false
    # 备案 XXXXXXXX 号
    icp:
  # Any custom text (e.g. Hosted by <a href="https://pages.github.com/" rel="noopener" target="_blank">Github Pages</a>)
  custom:
    enable: true
    text: 托管于 <a href="https://github.com/crlf01/crlf01.github.io/tree/hexo" rel="noopener" target="_blank">crlf</a>

# Creative Commons 4.0 International License
creative_commons:
  enable: true
  # Optional values: BY | BY-SA | BY-ND | BY-NC | BY-NC-SA | BY-NC-ND
  # For details, please see: https://creativecommons.org/share-your-work/licensing-types-examples/
  license: BY-NC-SA
  # Show the CC license in the sidebar.
  sidebar: true
  # Show the CC license at the post bottom.
  post: true
  # Optional values:
  #   id | ms | ca | da | de | en | es | es_ES | eo | eu | fr | gl | hr
  #   it | lv | lt | hu | nl | no | pt | pt_BR | pl | ro | sl | fi | sv
  #   tr | is | cs | el | be | ru | zh | zh_TW | uk | ar | fa | bn | ja | ko
  # If not set, `en` will be used by default.
  language: zh

# Back to top
back2top:
  enable: true
  icon:
    # Icon name in FontAwesome, see: https://fontawesome.com/icons
    name: fas fa-rocket
    # Rotation Angle of icon.
    rotate: -45deg
    # (Please use "" to wrap the value)
    color: "#49b1f5"
    # Color when the mouse hovers it (Please use "" to wrap the value)
    hover_color: "#fc6423"

# ---------------------------------------------------------------
# Sidebar config
# ---------------------------------------------------------------

# The sidebar of site.
sidebar:
  enable: true
  # Optional values: left | right
  position: left
  # The distance from the top of the page when the sidebar is fixed (Only `px` unit is supported)
  offsetTop: 20px
  # Whether to show horizon line.
  horizon_line: false

author:
  enable: true
  # Avatar in the site sidebar.
  avatar:
    # In theme directory (source/images): /images/avatar.png
    # In site directory (source/uploads): /uploads/avatar.png
    # You can also use a link of image.
    url: /images/icons/stun-logo.jpg
    # If true, the avatar would be displayed in a circle.
    rounded: true
    # Opacity of avatar (value: 0 ~ 1)
    opacity: 1
    # Mouse hover animation
    # Optional values: turn | shake
    animation: shake
  # Your favorite motto.
  motto: 人间值得 未来可期

# Social links
# Value before `||` is the target link, value after `||` is the name of FontAwesome icon.
# Usage: `Key: /link/ || fa(s|r|l|d|b) fa-iconname`
# Select your icon name, see: https://fontawesome.com/icons
# If you can`t find a suitable icon, you can choose to display text by adding the `origin:` prefix.
# e.g. `origin:sf`, so `sf` will be displayed.
social:
  github: https://github.com/crlf01/ || fab fa-github
  #google: https://plus.google.com/ || fab fa-google
  #twitter: https://twitter.com/ || fab fa-twitter
  #youtube: https://youtube.com/ || fab fa-youtube
  # segmentfault: https://segmentfault.com/ || origin:sf
  weibo: https://weibo.com/ || fab fa-weibo
  zhihu: https://www.zhihu.com/ || origin:知
  # wechat: yournumber || fab fa-weixin
  # telegram: yournumber || fab fa-telegram
  # qq: yournumber || fab fa-qq

social_setting:
  enable: true
  # Only show by icon.
  icon_only: true

# Table Of Contents in the Sidebar.
toc:
  enable: true
  # Automatically add list number to toc.
  list_number: true
  # If true, all words will be placed on the next lines when they overflow.
  wrap: true
  # If true, the toc of post will always be displayed, rather than the activated part of it.
  expand_all: false
  # Minimum heading depth of generated toc.
  # You can also set it in Front-matter by `toc_max_depth`.
  min_depth: 2
  # You can also set it in Front-matter by `toc_min_depth`.
  # Maximum heading depth of generated toc.
  max_depth: 4

# Subscribe of email and rss.
feed:
  enable: true
  # Enter your email subscription link (e.g. http://eepurl.com/guAE6j)
  email:
  # Enter the rss address of you set (e.g. /atom.xml)
  # Dependency: https://github.com/hexojs/hexo-generator-feed/
  # !!! Don't enable this before install dependency by `npm install hexo-generator-feed --save` in hexo root directory.
  rss:

# The reading progress of post.
reading_progress:
  enable: true
  # (Please use "" to wrap the value)
  color: "#fc6423"
  height: 1px

# ---------------------------------------------------------------
# Page config
# ---------------------------------------------------------------

codeblock:
  # The style of code block.
  # Optional values: default | simple | carbon
  style: carbon
  # The theme of code highlight.
  # Optional values: light | dark | ocean
  highlight: light
  # Whether to wrap when code overflow.
  word_wrap: false

# Add a line below h1, h2 tags.
heading_line: true

# Reward QR Code
reward:
  enable: false
  # Your QR code for collecting money.
  alipay:
  wechat:

# ---------------------------------------------------------------
# Post config
# ---------------------------------------------------------------

# The information of post.
post_meta:
  # Only show by icon.
  icon_only: false
  # The create time of post.
  created:
    enable: true
    # Icon name in FontAwesome, see: https://fontawesome.com/icons
    icon: far fa-calendar-plus
  # The update time of post.
  updated:
    enable: true
    # Icon name in FontAwesome, see: https://fontawesome.com/icons
    icon: far fa-calendar-check
  # The approximate reading time of post.
  # Dependency: https://github.com/willin/hexo-wordcount/
  # !!! Don't enable this before install dependency by `npm install hexo-wordcount --save` in hexo root directory.
  reading_time:
    enable: false
    # Icon name in FontAwesome, see: https://fontawesome.com/icons
    icon: far fa-clock
    # Set reading speed to count reading time.
    speed:
      # Reading speed of zh-CN.
      zh: 200
      # Reading speed of en-US.
      en: 80
  # Counting the words of post.
  # Dependency: https://github.com/willin/hexo-wordcount/
  # !!! Don't enable this before install dependency by `npm install hexo-wordcount --save` in hexo root directory.
  word_count:
    enable: false
    # Icon name in FontAwesome, see: https://fontawesome.com/icons
    icon: far fa-file-word

# The article list on the homepage or archives page.
post_list:
  # Whether to paginate.
  paginate:
    home: true
    archives: false
  # Whether to show the cover image of post.
  cover_image:
    home: false

# The widget of post.
post_widget:
  # Whether to show tags at the bottom of post.
  tags: true
  # Whether to show "------ END ------" at the bottom of post.
  end_text:
    enable: true
    # Whether to show a horizon line before "------ END ------".
    horizon_line: true
  # Article sharing
  share:
    enable: false
    # The text displayed before the share button.
    label: "Share to: "
    # Optional values: qzone | qq | weibo | wechat | douban | linkedin | facebook | twitter | google
    target: qzone, qq, weibo, wechat, douban, linkedin, facebook, twitter, google

# Stick post to the top
# Dependency: https://github.com/netcan/hexo-generator-index-pin-top/
# !!! Don't enable this before install dependency by `npm install hexo-generator-index-pin-top --save` in hexo directory.
stick_top:
  # Position of icon
  # Optional values: left | right
  position: right
  # Icon name in FontAwesome, see: https://fontawesome.com/icons
  icon: fas fa-thumbtack
  # Rotation Angle of icon.
  rotate: 45deg
  # (Please use "" to wrap the value)
  color: "#999"

# ---------------------------------------------------------------
# Comment config
# ---------------------------------------------------------------

# Gittalk
# See: https://github.com/gitalk/gitalk/
gitalk:
  enable: false
  # Github username.
  owner:
  # Github repository.
  repo:
  # Github Application Client ID.
  client_id:
  # Github Application Client Secret.
  client_secret:
  # GitHub repo owner and collaborators, only these guys can initialize github issues.
  admin:
  # Facebook-like distraction free mode.
  distraction_free_mode: false
  # Gitalk's display language depends on user's browser or system environment.
  # If you want everyone visiting your site to see a uniform language, you can set a force language value.
  # Optional values: en | zh-CN | es-ES | fr | ru | zh-TW
  language:

# Valine
# See: https://valine.js.org/quickstart.html
valine:
  enable: false
  # Your leancloud application appid.
  appid:
  # Your leancloud application appkey.
  appkey:
  # Mail notifier.
  notify: true
  # Verification code.
  verify: true
  # Comment box placeholder.
  placeholder: Just go go
  # Gravatar style.
  avatar: mp
  # Custom comment header.
  meta: nick,mail,link
  # Pagination size.
  pageSize: 10
  # Article reading statistics.
  visitor: false
  # Whether to record the commenter IP.
  recordIP: false
  # Optional values: en | zh-cn
  language: zh-cn

# MiniValine
# See: https://github.com/MiniValine/MiniValine#Options
minivaline:
  enable: false
  appId: # Your leancloud application appid
  appKey: # Your leancloud application appkey
  placeholder: Write a Comment # Comment box placeholder
  adminEmailMd5: # The MD5 of Admin Email to show Admin Flag.
  math: true # Support MathJax.
  md: true # Support Markdown.
  # MiniValine's display language depends on user's browser or system environment
  # If you want everyone visiting your site to see a uniform language, you can set a force language value
  # Optional values: en  | zh-CN | (and many more)
  # More i18n info: https://github.com/MiniValine/minivaline-i18n
  lang:

# Livere
# See: https://www.livere.com/
livere:
  enable: false
  uid:

# Disqus
# See: https://disqus.com/
disqus:
  enable: false
  shortname:
  count: true

# Utterances
# See: https://utteranc.es/
utterances:
  enable: false
  # Github username.
  owner:
  # Github repository.
  repo:
  # Choose the mapping between blog posts and GitHub issues.
  # Optional values: pathname | url | title | og:title
  mapping: title
  # Choose the label that will be assigned to issues created by Utterances.
  # Emoji are supported in label names.
  label: utterances
  # Choose an Utterances theme that matches your blog.
  # Optional values: github-light | github-dark | github-dark-orange | icy-dark | dark-blue | photon-dark
  theme: github-light
  # ! -------------------------------------------------------------------------------
  # ! Don't set this unless the URL of the script in the official website is changed.
  # ! -------------------------------------------------------------------------------
  script_url: https://utteranc.es/client.js

# ---------------------------------------------------------------
# Statistics and Analytics config
# ---------------------------------------------------------------

# Busuanzi statistics
# See: https://busuanzi.ibruce.info/
busuanzi:
  enable: true
  # Only show by icon.
  icon_only: false
  # Number of unique visitor to the entire site.
  site_uv:
    enable: true
    # Icon name in FontAwesome, see: https://fontawesome.com/icons
    icon: fas fa-user
  # Number of page view to the entire site.
  site_pv:
    enable: true
    icon: fas fa-eye
  # Number of page view to a post.
  post_pv:
    enable: true
    icon: fas fa-eye

# Google analytics ID
# See: https://analytics.google.com/
google_analytics:

# Baidu analytics ID
# See: https://tongji.baidu.com/
baidu_analytics:

# Tencent analytics ID
# See: https://v2.ta.qq.com/
tencent_analytics:

# ---------------------------------------------------------------
# Search config
# ---------------------------------------------------------------

# Algolia Search
# Dependency: https://github.com/algolia/instantsearch.js/
algolia_search:
  enable: false
  hits:
    # Number of search results displayed per page.
    per_page: 10
  labels:
    # Whether to show stats of search.
    show_stats: true

# Local Search
# See: https://github.com/wzpan/hexo-generator-search/
local_search:
  enable: false


# ---------------------------------------------------------------
# Background config
# ---------------------------------------------------------------

# Canvas-ribbon
# Dependency: https://github.com/hustcc/ribbon.js
canvas_ribbon:
  enable: true
  # The width of the ribbon.
  size: 120
  # The transparency of the ribbon.
  alpha: 0.6
  # The display level of the ribbon.
  zIndex: -1

# Canvas-nest
# Dependency: https://github.com/hustcc/canvas-nest.js
canvas_nest:
  enable: false
  # Color of lines.
  # RGB values, use `,` to separate.
  color: "0,0,0"
  # The opacity of lines (value: 0 ~ 1)
  opacity: 0.6
  # The number of lines.
  count: 99
  # `z-index` property of the background.
  zIndex: -1

# ---------------------------------------------------------------
# Math and Chart config
# ---------------------------------------------------------------
math:
  enable: false
  # If true, this function will be enabled on every page.
  # If false, this function will be enabled only if the page where `math: true` is set in `Front-matter`.
  per_page: false
  # ! -------------------------------------------------------------------------------
  # ! Please see the documentation before using. See:
  # ! https://liuyib.github.io/hexo-theme-stun/zh-CN/advanced/third-part.html#mathjax
  # ! -------------------------------------------------------------------------------
  # Optional values: mathjax | katex
  engine: katex
  mathjax:
    cdn: https://cdn.jsdelivr.net/npm/mathjax/MathJax.js?config=TeX-AMS-MML_HTMLorMML
    # See: https://mhchem.github.io/MathJax-mhchem/
    mhchem:
      enable: false
      mhchem_js: https://cdn.jsdelivr.net/npm/mathjax-mhchem@3.3.2/mhchem.min.js
  katex:
    cdn: https://cdn.jsdelivr.net/npm/katex@0.10.2/dist/katex.min.css
    # See: https://github.com/KaTeX/KaTeX/tree/master/contrib/copy-tex
    copy_tex:
      enable: true
      copy_tex_js: https://cdn.jsdelivr.net/npm/katex@0.10.2/dist/contrib/copy-tex.min.js
      copy_tex_css: https://cdn.jsdelivr.net/npm/katex@0.10.2/dist/contrib/copy-tex.css

# ---------------------------------------------------------------
# Other config
# ---------------------------------------------------------------

# Fancybox
# See: https://fancyapps.com/fancybox/3/
fancybox: false

# Click to enlarge picture.
zoom_image:
  enable: true
  # The color of mask.
  # (Please use "" to wrap the value)
  mask_color: "rgba(0,0,0,0.6)"
  # The gap between image and a edge of screen when image is enlarged.
  # (Only `px` unit is supported)
  gap_aside: 20px

# If you are use "photos" attribute in the Front-matter,
#   you can enable this to show images in waterfalls flow.
# See: https://github.com/desandro/masonry/
gallery_waterfall:
  enable: false
  col_width: 220px
  gap_x: 10px
  gap_y: 10px

# Lazy load the images of post.
# See: https://github.com/tuupola/lazyload
lazyload:
  enable: false
  # Optional values: gif | block
  placeholder: gif

# Quicklink support
# See: https://github.com/GoogleChromeLabs/quicklink/
quicklink:
  enable: false
  # Quicklink (quicklink.umd.js script) is loaded on demand.
  # Add `quicklink: true` in Front-matter of the page or post you need.
  # Home page and archive page can be controlled through home and archive options below.
  home: true
  archive: true
  # Initialize quicklink after the load event fires.
  delay: true
  # Custom a time in milliseconds by which the browser must execute prefetching.
  timeout: 10000
  # Enable fetch() or falls back to XHR.
  priority: true
  # For more flexibility you can add some patterns (RegExp, Function, or Array) to ignores.
  # See: https://github.com/GoogleChromeLabs/quicklink#custom-ignore-patterns
  # ! --------------------------------------------------
  # ! If you don't know the following, please ignore it.
  # ! --------------------------------------------------
  ignores:
    - /\/api\/?/
    - uri => uri.includes('.xml')
    - uri => uri.includes('.zip')
    - (uri, el) => el.hasAttribute('nofollow')
    - (uri, el) => el.hasAttribute('noprefetch')

# Pjax
# See: https://github.com/MoOx/pjax/
pjax:
  enable: true
  # When switching pages, scroll to the bottom of the top image.
  avoid_banner: true
  # ! --------------------------------------------------
  # ! If you don't know the following, please ignore it.
  # ! --------------------------------------------------
  # Please see: https://github.com/MoOx/pjax/#options
  elements:
  selectors:
  switches:
  switchesOptions:
  history: true
  # If you enable this, you must set `avoid_banner: false` firstly.
  scrollTo: false
  scrollRestoration: false
  cacheBust: false
  debug: false
  currentUrlFullReload: false
  timeout: 0

# Google AdSense
google_adsense:
  enable: false
  client:
  js_src: https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js

# The links with `target="_blank"` attribute.
external_link:
  # Adding an icon make it easier for users to know that this is an external link.
  icon:
    enable: true
    # Icon name in FontAwesome, see: https://fontawesome.com/icons
    name: fas fa-external-link-alt
    # (Please use "" to wrap the value)
    color: "#aaa"

# The shortcuts of the site.
shortcuts:
  # Switch to the prev / next post by key.
  # `Ctrl + ←` is the shortcuts to prev post.
  # `Ctrl + →` is the shortcuts to next post.
  switch_post:
    enable: true

# Tag cloud
# If not used, comment it or ignore it.
tag_cloud:
  # (Please use "" to wrap the value)
  start_color: "#a4d8fa"
  end_color: "#49b1f5"
  # Size for tag.
  min_size: 16
  max_size: 26
  # Maximum number of tags displayed. Change it if you have more than 200 tags.
  max_amount: 200

# Eliminate the old version of IE browser (IE6~11).
# See: https://support.dmeng.net/upgrade-your-browser.html
kill_old_ie:
  enable: false
  # The URL of warning page (Please use "" to wrap the value).
  # ! ----------------------------------------------------------------
  # ! Usually you don't need to set this option, just use the default.
  # ! ----------------------------------------------------------------
  warning_url: https://support.dmeng.net/upgrade-your-browser.html

# Assets
# In theme directory (source/css)
css: css
# In theme directory (source/js)
js: js
# In theme directory (source/images)
images: images

# Set icon for some components.
# Icon name in FontAwesome, see: https://fontawesome.com/icons
# ! -------------------------------------------------------------------
# ! Do not edit the follow configs, unless you know what you are doing.
# ! -------------------------------------------------------------------
icon:
  search: fas fa-search
  localsearch_empty: far fa-meh
  menu_btn: fas fa-bars
  feed_email: fas fa-envelope
  feed_rss: fas fa-rss
  paginator_prev: fas fa-angle-left
  paginator_next: fas fa-angle-right
  read_more_btn: fas fa-long-arrow-alt-right
  post_tags: fas fa-tag
  copy_btn: fas fa-copy
  prompt_success: fas fa-check-circle
  prompt_info: fas fa-arrow-circle-right
  prompt_warning: fas fa-exclamation-circle
  prompt_error: fas fa-times-circle
  valine_visitor: fas fa-eye
  post_heading: fas fa-link
  notetag_default: fas fa-arrow-circle-right
  notetag_success: fas fa-check-circle
  notetag_info: fas fa-info-circle
  notetag_warning: fas fa-exclamation-circle
  notetag_danger: fas fa-minus-circle

# Set a CDN address for the vendor you want to customize.
# ! -------------------------------------------------------------------
# ! Do not edit the follow configs, unless you know what you are doing.
# ! -------------------------------------------------------------------
cdn:
  # Using version: 5.12.1
  # See: https://fontawesome.com/
  # Example:
  # fontawesome: //cdn.jsdelivr.net/npm/font-awesome@4.7.0/css/font-awesome.min.css
  # fontawesome: //cdn.jsdelivr.net/npm/@fortawesome/fontawesome-free@5.12.1/css/all.min.css
  # fontawesome: //cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css
  # fontawesome: //cdnjs.cloudflare.com/ajax/libs/font-awesome/5.12.1/css/all.min.css
  fontawesome:

  # Using version: 3.4.1
  # Example:
  # jquery: //cdn.jsdelivr.net/npm/jquery@v3.4.1/dist/jquery.min.js
  # jquery: //cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.min.js
  jquery:

  # Using version: 1.5.2
  # See: http://velocityjs.org/
  # Example:
  # velocity: //cdn.jsdelivr.net/npm/velocity-animate@1.5.2/velocity.min.js
  # velocity: //cdnjs.cloudflare.com/ajax/libs/velocity/1.5.2/velocity.min.js
  # velocity_ui: //cdn.jsdelivr.net/npm/velocity-animate@1.5.2/velocity.ui.min.js
  # velocity_ui: //cdnjs.cloudflare.com/ajax/libs/velocity/1.5.2/velocity.ui.min.js
  velocity:
  velocity_ui:

  # gitalk & js-md5
  # Using version: latest & latest
  # See: https://github.com/gitalk/gitalk/, https://github.com/emn178/js-md5/
  # Example:
  # gitalk_js: //cdn.jsdelivr.net/npm/gitalk@latest/dist/gitalk.min.js
  # gitalk_css: //cdn.jsdelivr.net/npm/gitalk@latest/dist/gitalk.css
  # md5: //cdn.jsdelivr.net/npm/js-md5@latest/src/md5.min.js
  gitalk_js:
  gitalk_css:
  gitalk_md5:

  # valine & leancloud-storage
  # Using version: latest & latest
  # See: https://github.com/xCss/Valine/, https://www.npmjs.com/package/leancloud-storage/
  # Example:
  # valine: //cdn.jsdelivr.net/npm/valine@latest/dist/Valine.min.js
  # leancloud_storage: //cdn.jsdelivr.net/npm/leancloud-storage@latest/dist/av-min.js
  valine:
  leancloud_storage:

  # minivaline
  # See: https://github.com/MiniValine/MiniValine
  # Example:
  # minivaline: //cdn.jsdelivr.net/npm/minivaline/dist/MiniValine.min.js
  minivaline:

  # busuanzi
  # Using version: latest
  # See: https://busuanzi.ibruce.info/
  # Example:
  # busuanzi: //cdn.jsdelivr.net/gh/sukkaw/busuanzi@latest/bsz.pure.mini.js
  busuanzi:

  # Using version: 2.1.1
  # See: https://busuanzi.ibruce.info/
  # Example:
  # instantsearch_js: //cdn.jsdelivr.net/npm/instantsearch.js@2.1.1/dist/instantsearch.min.js
  # instantsearch_css: //cdn.jsdelivr.net/npm/instantsearch.js@2.1.1/dist/instantsearch.min.css
  instantsearch_js:
  instantsearch_css:

  # Using version: latest
  # See: https://github.com/hustcc/ribbon.js
  # Example:
  # canvas_ribbon: //cdn.jsdelivr.net/npm/ribbon.js@latest/dist/ribbon.min.js
  canvas_ribbon:

  # Using version: latest
  # See: https://github.com/hustcc/canvas-nest.js
  # Example:
  # canvas_nest: //cdn.jsdelivr.net/npm/canvas-nest.js@1.0.1/dist/canvas-nest.min.js
  canvas_nest:

  # Using version: 3.5.7
  # See: https://www.fancyapps.com/fancybox/3/
  # Example:
  # fancybox_js: //cdn.jsdelivr.net/gh/fancyapps/fancybox@3.5.7/dist/jquery.fancybox.min.js
  # fancybox_css: //cdn.jsdelivr.net/gh/fancyapps/fancybox@3.5.7/dist/jquery.fancybox.min.css
  fancybox_js:
  fancybox_css:

  # Using version: 4.2.2
  # See: https://masonry.desandro.com/
  # Example:
  # masonry: //cdn.jsdelivr.net/npm/masonry-layout@4.2.2/dist/masonry.pkgd.min.js
  masonry:

  # Using version: 2.0.0-rc.2
  # See: https://github.com/tuupola/lazyload/
  # Example:
  # lazyload: //cdn.jsdelivr.net/npm/lazyload@2.0.0-rc.2/lazyload.min.js
  lazyload:

  # Using version: latest
  # See: https://github.com/GoogleChromeLabs/quicklink/
  # Example:
  # quicklink: //cdn.jsdelivr.net/npm/quicklink@latest/dist/quicklink.umd.js
  quicklink:

  # Using version: latest
  # See: https://github.com/MoOx/pjax/
  # Example:
  # pjax: //cdn.jsdelivr.net/npm/pjax@latest/pjax.min.js
  pjax:

  # Using version: 1.0.16
  # See: https://github.com/overtrue/share.js
  # Example:
  # share_js: //cdn.jsdelivr.net/npm/social-share.js@1.0.16/dist/js/social-share.min.js
  # share_css: //cdn.jsdelivr.net/npm/social-share.js@1.0.16/dist/css/share.min.css
  share_js:
  share_css:

```

然后，保存起来，部署到github就ok了！

### 六、其他Tiips

(1.) 创建 **github soure **分支，备份原代码到github

- 分支创建，记得切换回master分支：

  ```
  git init
  git checkout -b source
  git add -A
  git commit -m "init blog"
  git remote add origin git@github.com:{username}/{username}.github.io.git
  git push origin source
  git checkout -b master
  ```

- 根目录 **__config.yml** 文件添加下列内容，注意改成自己的用户名、主题、分支名：

  ```
  # backup
  backup:
    type: git
    theme: stun
    message: Back up my blog's source
    repository:
      github: git@github.com:crlf01/crlf01.github.io.git,source
  ```



- 安装相关插件工具，然后备份：

  ```
  npm install hexo-git-backup --save
  hexo back up
  ```

(2.) 报以下错误时

```
extends includes/layout.pug block content #recent-posts.recent-posts include includes/recent-posts.pug include includes/pagination.pug 
```

使用以下命令解决：

```
npm install --save hexo-renderer-jade hexo-generator-feed hexo-generator-sitemap hexo-browsersync hexo-generator-archive
```



(3.)  hexo相关命令：

```
hexo init xxx 	 		 #创建命令
hexo g					#生成静态页面
hexo s					#启动本地环境
hexo d					#部署到github
hexo clean				#清除缓存
git checkout -b maste	 #切换带master分支
hexo new "hello-world"	 #在\source\_posts目录hello-world.md文件
```



(4.) 修默认文件头配置，/scaffolds/post.md：

```
--- 
title: {{ title }}
date: {{ date }} 
top: false 
cover: false 
password: 
toc: true 
mathjax: true 
summary: 
tags: 
categories: 
---
```



(5.) 在项目目录创建一键上传、更新文章脚本，创建 **deploy.sh** 脚本：

```
hexo clean
hexo generate
hexo deploy
hexo back up
```



---



### **OK，终于写完了，写的有点乱，如有错误，欢迎斧正。**
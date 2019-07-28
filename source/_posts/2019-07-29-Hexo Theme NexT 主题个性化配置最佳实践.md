---
title: Hexo Theme NexT 主题个性化配置最佳实践
tags:
  - Hexo
  - 博客
categories: Hexo
copyright: true
abbrlink: 1700540306
date: 2019-07-29 01:42:28
updated: 2019-07-29 01:42:28
---
一般情况下，当我们在使用 Hexo 的 NexT 主题时，都希望把博客改造成自己喜欢的风格。NexT 主题经过不断的迭代积累，目前提供了非常丰富的配置来满足使用者的个性化需求。

经过一段时间的摸索，我总结了一些有关 NexT 主题配置的最佳实践方案，能够优雅的对博客进行个性化改造及持续升级。核心思想就是，使用官方的推荐的方式配置主题，多挖掘博客自带的功能，尽可能少得修改源码。下面分享我的做法。

# 版本

- Hexo 3.9.0
- NexT 7.2.0

# 一些踩过的坑
目前，网上有很多 Hexo NexT 个性化配置资料，比如像 [博客的美化配置（NexT主题）](http://amosmeer.cn/2019/01/09/%E5%8D%9A%E5%AE%A2%E7%9A%84%E7%BE%8E%E5%8C%96%E9%85%8D%E7%BD%AE%EF%BC%88NexT%E4%B8%BB%E9%A2%98%EF%BC%89/) 这样的方案。

起初，我根据自己的需要按照上面博客里的方法进行配置是没什么太大问题的。但是当我尝试升级 NexT 主题的时候，问题就来了。上述博客里的方案，很多功能的实现需要修改 NexT 源码，当 pull NexT 最新代码与本地分支 merge 时，会产生大量的冲突，非常不方便。

后来经过查阅官方博客 [NexT - Docs](https://theme-next.org/docs/)，我发现随着 NexT 版本的迭代，现如今的 NexT 已经集成了很多上面博客里提到的功能，我们通过修改配置文件即可使用，绝大部分的功能已经不再需要修改源码实现了。

另外，NexT 也建议大家使用 Hexo 官方推荐的 Data Files 系统（Hexo 3.x 即以上）来分离个人配置，稍后我会详细介绍。这样就可以在尽可能少地修改 NexT 工程代码的情况下进行个性化配置，方便主题升级。

# 改造前的工作
在 hexo 和 next 的根目录下，都存在一个叫做 `_config.yml` 的配置文件。在改造之前，让我们来规定一下两个文件的叫法以方便区分。

- `hexo/_config.yml`：site config file
- `next/_config.yml`：theme config file

## Clone NexT
直接把 NexT 工程从 GitHub 上克隆下来放在 Hexo 的 theme/next 中，这样方便未来主题的升级工作。

NexT 工程地址：[hexo-theme-next](https://github.com/theme-next/hexo-theme-next)

## 个性化配置分离

如果能把个性化的配置内容分离出来，也就是说在其他地方通过某种方式配置个性化的设置而不直接修改主题的 `theme config file` 的话，那么我们在 pull 最新的 NexT 代码时，就不会对 `theme config file` 产生冲突。

NexT 官方博客中的 [Data Files](https://theme-next.org/docs/getting-started/data-files) 一文详细说明了如果使用 Data Files 系统进行个性化配置。

不过 Data Files 需要 Hexo 的版本在 3.x 之上，所以文中提供两种配置方法供大家选择。

### Hexo 2.x
如果是 Hexo 2.x 版本（当然 3.x 也支持这种方式），可以通过在 `site config file` 中编写主题的配置，而不需要修改 `theme config file`，也不需要添加任何新的文件。

步骤：
1. 检查一下是否存在 `hexo/source/_data/next.yml` 文件，如果存在则删除。
2. 在 `site config file` 添加 `theme_config:` 节点。
3. 需要修改的配置内容从 `theme config file` 文件中拷贝到 `site config file` 的 `theme_config:` 节点下。注意缩进。

### Hexo 3.x
如果是 Hexo 3.x 的话，可以将 `theme config file` 需要修改的配置放入 `hexo/source/_data/next.yml` 中，不需要修改 `theme config file`。

步骤：
1. 确定是否使用的是 Hexo 3.x 及以上的版本。
2. 在 `hexo/source/_data/` 目录中新建 `next.yml` 文件（如果 `_data` 文件夹不存在，则新建一个）。
3. 如果 `next.yml` 中设置 `override: false`，那么只需要将需要的配置项从 `site config file` 和 `theme config file` 文件中拷贝过来。
4. 如果 `next.yml` 中设置 `override: ture`，那么需要将所有 `theme config file` 中的内容拷贝过来。

# 个性化改造
有关配置文件各项配置的使用，官方博客 [NexT - Docs](https://theme-next.org/docs/) 中给出了详细的阐述，这里我只记录了一些比较好玩的功能。有关博客名称、头像等这种基本配置我就不再说明了。

## 第三方库使用 CDN
第三方库可以放在 `next/source/lib/` 目录下，也可以使用 CDN 在加载这些库。如果想要减少服务器流量压力的话，可以通过配置 CDN 地址，使第三方库通过 CDN 加载，提高站点打开速度。

NexT 提供了 CDN 的配置地址，并给出了推荐的 URL，例子如下。

```yml
# Script Vendors. Set a CDN address for the vendor you want to customize.
# Be aware that you would better use the same version as internal ones to avoid potential problems.
# Please use the https protocol of CDN files when you enable https on your site.
vendors:
  # Internal path prefix. Please do not edit it.
  _internal: lib

  # Internal version: 3.4.1
  # Example:
  # jquery: //cdn.jsdelivr.net/npm/jquery@3/dist/jquery.min.js
  # jquery: //cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.min.js
  jquery:

  # Internal version: 4.7.0
  # See: https://fontawesome.com
  # Example:
  # fontawesome: //cdn.jsdelivr.net/npm/font-awesome@4/css/font-awesome.min.css
  # fontawesome: //cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css
  fontawesome:
```


## Footer / 页脚设置

```yml
footer:
  # © 和年份中间的图标
  icon:
    # 图标名
    name: user
    # 图标的一个动画效果，类似于心跳
    animated: true
    # 图标颜色，可格局需要自行修改
    color: "#808080"

  # Powered by Hexo 字样，不喜欢可以设置为 false
  powered:
    enable: true
    # Version info of Hexo after Hexo link (vX.X.X).
    version: true
    
  # 主题字样，不喜欢可以 false
  theme:
    # Theme & scheme info link (Theme - NexT.scheme).
    enable: true
    # Version info of NexT after scheme info (vX.X.X).
    version: true

  # 备案信息，如果网站有备案号，可以在这里填写备案号
  beian:
    enable: false
    icp:
```

## Creative Commons / 文章版权

```yml
# Creative Commons 4.0 International License.
# See: https://creativecommons.org/share-your-work/licensing-types-examples
# Available values of license: by | by-nc | by-nc-nd | by-nc-sa | by-nd | by-sa | zero
# You can set a language value if you prefer a translated version of CC license, e.g. deed.zh
# CC licenses are available in 39 languages, you can find the specific and correct abbreviation you need on https://creativecommons.org
creative_commons:
  license: by-nc-nd
  post: true
  language: deed.zh
```

在文章 `.md` 文件中的上部，添加 `copyright: true`。

有关 Creative Commons 大家可以到 [creativecommons](https://creativecommons.org) 中查看。

## 返回顶部按钮

```yml
back2top:
  enable: true
  # 是否在侧边栏显示
  sidebar: false
  # 是否显示页面浏览百分比
  scrollpercent: false
```

## Tag 标签前图标修改
文章标签的显示默认前面“#”号，可以通过设置将“#”换为图标。

```yml
tag_icon: true
```

## 界面加载进度条
NexT 集成了 [theme-next-pace](https://github.com/theme-next/theme-next-pace)。资源库需要自行下载或者使用 CDN 的方式。

```yml
pace:
  enable: false
  # Themes list:
  # big-counter | bounce | barber-shop | center-atom | center-circle | center-radar | center-simple
  # corner-indicator | fill-left | flat-top | flash | loading-bar | mac-osx | material | minimal
  theme: minimal

vendors:
  # Internal version: 1.0.2
  # See: https://github.com/HubSpot/pace
  # Example:
  # pace: //cdn.jsdelivr.net/npm/pace-js@1/pace.min.js
  # pace: //cdnjs.cloudflare.com/ajax/libs/pace/1.0.2/pace.min.js
  # pace_css: //cdn.jsdelivr.net/npm/pace-js@1/themes/blue/pace-theme-minimal.css
  # pace_css: //cdnjs.cloudflare.com/ajax/libs/pace/1.0.2/themes/blue/pace-theme-minimal.min.css
  pace:
  pace_css:
```

## 访问量统计
NexT 集成了已经集成好了不蒜子。

```yml
# Show Views / Visitors of the website / page with busuanzi.
# Get more information on http://ibruce.info/2015/04/04/busuanzi
busuanzi_count:
  enable: false
  total_visitors: true
  total_visitors_icon: user
  total_views: true
  total_views_icon: eye
  post_views: true
  post_views_icon: eye
```

如果对显示的文案不满意的话，可以修改 `/next/layout/_third-party/analytics/busuanzi-counter.swig` 文件中的相关内容。

## 背景彩带

```yml
# Canvas-ribbon
# Dependencies: https://github.com/theme-next/theme-next-canvas-ribbon
# size: The width of the ribbon.
# alpha: The transparency of the ribbon.
# zIndex: The display level of the ribbon.
canvas_ribbon:
  enable: true
vendors:
  # Internal version: 1.0.0
  # See: https://github.com/zproo/canvas-ribbon
  # Example:
  canvas_ribbon: //cdn.jsdelivr.net/gh/theme-next/theme-next-canvas-ribbon@1/canvas-ribbon.js
```

## 字数统计
NexT 集成了 [hexo-symbols-count-time](https://github.com/theme-next/hexo-symbols-count-time) 插件。

步骤：
1. `npm install hexo-symbols-count-time --save`
2. 在 `site config file` 中添加 `symbols_count_time` 配置。

```yml
symbols_count_time:
  # 文章上部是否显示字数
  symbols: true
  # 文章上部是否显示阅读时间
  time: true
  # 站点底端是否显示站点总字数
  total_symbols: true
  # 站点底端是否显示总阅读时间
  total_time: false
  # 是否移除代码块
  exclude_codeblock: false
```
3. 在 `next.yml` 中配置 `symbols_count_time` 节点。

```yml
# Post wordcount display settings
# Dependencies: https://github.com/theme-next/hexo-symbols-count-time
symbols_count_time:
  separated_meta: true
  item_text_post: true
  item_text_total: false
  awl: 4
  wpm: 275
  suffix: mins.
```

其中：
- awl：平均字符长度，默认为 4。
    - 汉字 ≈ 2
    - 英文 ≈ 4
    - 俄文 ≈ 6
- wpm：阅读速度。
    - 慢 ≈ 200
    - 正常 ≈ 275
    - 快 ≈ 350
- suffix：后缀，默认为 `mins.`。

**对中文用户来说**：汉字的平均长度 ≈ 1.5，如果仅用中文书写没有英文的话，建议 `awl` 和 `wmp` 分别设置为 `2` 和 `300`。如果中英混合，建议 `awl` 和 `wmp` 分别设置为 `4` 和 `275`。

## GitHub Fork Me
在站点右上角添加 GitHub 标识，例如“Fork me on GitHub”。


```yml
# `Follow me on GitHub` banner in the top-right corner.
github_banner:
  enable: false
  permalink: https://github.com/yourname
  title: Follow me on GitHub
```

## 图片延迟加载
对于图片进行延迟加载，访问到图片位置时才去请求图片资源，这样可以提高博客的访问速度，节省流量。

步骤：
1. `npm install hexo-lazyload-image --save`
2. 在 `site config file` 中添加 `lazyload` 配置。

```yml
# Lazyload
## Depends on hexo-lazyload-image
lazyload:
  enable: true
  onlypost: false
  # 图片尚未加载完时，显示指定图片。目录地址为博客根目录下的 source/
  loadingImg: /uploads/loading.gif
```

## 修改配色
NexT 主题默认色系是黑白色系。目前官方尚未提供颜色修改的配置，所以我们可以通过修改相关 `.styl` 文件来修改主题颜色。

相关文件：
- `themes/next/source/css/_common/components/post/post-title.styl`
- `themes/next/source/css/_schemes/Pisces/_brand.styl`
- `themes/next/source/css/_variables/base.styl`
- `themes/next/source/css/_variables/Pisces.styl`

这其中比较复杂，这里我就不一一介绍了（我也没完全弄清楚这些参数到底对应主题哪个部分的颜色），大家一点点尝试修改相关配色。

## 链接持久化
Hexo 默认的文章链接是“年/月/日/标题”。之所以要做链接持久化是因为，中文 url 不利于 SEO，另外如果标题修改了，会导致链接发生变化，不利于文章的推广。所以我们要做的就是把标题转成唯一的英文或数字字符串。这里推荐 rozbo 大神的 [hexo-abbrlink](https://github.com/Rozbo/hexo-abbrlink)。

步骤：
1. `npm install hexo-abbrlink --save`
2. 在 `site config file` 中添加

```yml
permalink: posts/:abbrlink/

# abbrlink config
abbrlink:
  alg: crc32  #support crc16(default) and crc32
  rep: dec    #support dec(default) and hex
```

- alg 是算法。有 crc16 和 crc32 两种。
- rep 是进制。有 dec（十进制） 和 hex（十六进制） 两种。

样例：
```text
crc16 & hex
https://post.zz173.com/posts/66c8.html

crc16 & dec
https://post.zz173.com/posts/65535.html

crc32 & hex
https://post.zz173.com/posts/8ddf18fb.html

crc32 & dec
https://post.zz173.com/posts/1690090958.html
```

crc16 算法下的十进制编码最大为 65535，这对个人博客来说足够了。

有关作者写的详细介绍 [hexo-abbrlink介绍](https://post.zz173.com/detail/hexo-abbrlink.html) 感兴趣可以看下。

---
想要对 NexT 主题配置更深入的了解及进阶使用，推荐阅读官方博客 [NexT](https://theme-next.org/)，写得非常全面。
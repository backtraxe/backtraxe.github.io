# Hugo 博客搭建


使用 Hugo 搭建一个博客，并使用 Github Action 自动部署到 Github Pages。

<!--more-->

## 1.安装

- Windows
  1. 到 [Github](https://github.com/gohugoio/hugo/releases/latest) 下载`hugo_extended_0.XX.X_Windows-64bit.zip`，解压并将`hugo.exe`所在目录添加到系统环境变量。
  2. 到 [Git 官网](https://git-scm.com/downloads) 下载安装 Git。
- Chocolatey (Windows)
  - `choco install hugo-extended git`
- Scoop (Windows)
  - `scoop install hugo-extended git`
- Homebrew (macOS)
  - `brew install hugo git`


<br>

## 2.创建网站

1. 打开命令行，输入以下内容，其中`MyBlog`可修改。
2. 在 [Hugo Themes](https://themes.gohugo.io/) 寻找更多主题。若更换别的主题，将 git 地址和 `DoIt` 替换。

```bash
hugo new site MyBlog
cd MyBlog
git init
git submodule add https://github.com/HEIGE-PCloud/DoIt.git themes/DoIt
```

<br>

## 3.添加内容

### 3.1 新建博客

输入`hugo new posts/My-First-Blog.md`，然后打开`My-First-Blog.md`，显示如下：

```markdown
---
title: "My First Blog"
date: 2021-02-04T16:18:47+08:00
draft: true
---
```

Markdown 语法见 <a href="../markdown-基本语法" target="_blank">Markdown 基本语法</a>。

<br>

### 3.2 在文章中添加图片

将图片存放于`static`文件夹。

若图片路径为`static/xxx/yyy.jpg`，在博客中使用`/xxx/yyy.jpg`来显示图片。

<br>

### 3.2 Shortcodes

Hugo 专属，非 markdown 语法，参考 [扩展 Shortcodes](https://hugodoit.pages.dev/zh-cn/theme-documentation-extended-shortcodes/)。

<br>

#### 3.2.1 图片

```markdown
{{</* image src="" caption="" height="" width="" */>}}
{{</* figure src="" link="" target="_blank" title="" caption="" height="" width="" */>}}
```

<br>

#### 3.2.2 横幅

- 横幅类型：`note`、`abstract`、`info`、`tip`、`success`、`question`、`warning`、`failure`、`danger`、`bug`、`example`、`quote`
- 横幅标题
- 横幅是否默认展开

```markdown
{{</* admonition note "" false */>}}
横幅内容
{{</* /admonition */>}}
```

{{< admonition note "note" false >}}{{< /admonition >}}
{{< admonition abstract "abstract" false >}}{{< /admonition >}}
{{< admonition info "info" false >}}{{< /admonition >}}
{{< admonition tip "tip" false >}}{{< /admonition >}}
{{< admonition success "success" false >}}{{< /admonition >}}
{{< admonition question "question" false >}}{{< /admonition >}}
{{< admonition warning "warning" false >}}{{< /admonition >}}
{{< admonition failure "failure" false >}}{{< /admonition >}}
{{< admonition danger "danger" false >}}{{< /admonition >}}
{{< admonition bug "bug" false >}}{{< /admonition >}}
{{< admonition example "example" false >}}{{< /admonition >}}
{{< admonition quote "quote" false >}}{{< /admonition >}}

<br>

#### 3.2.3 公式

```markdown
{{</* math */>}}$ 行内公式 ${{</* /math */>}}
```

```markdown
{{</* math */>}}
$$
公式块
$$
{{</* /math */>}}
```

<br>

#### 3.2.4 代码

- 文件类型
- `linenostart`：起始行号
- `hl_lines`：高亮行号（从 1 开始）

```markdown
{{</* highlight java "linenostart=5, hl_lines=5 7-9" */>}}
代码块
{{</* /highlight */>}}
```

{{< highlight markdown >}}
```java {linenostart=5, hl_lines=[5,"7-9"]}
代码块
```
{{< /highlight >}}

<br>

#### 3.2.5 gist

- 用户名
- gist ID

```markdown
{{</* gist backtraxe 9457ba6238b0a98237a17dae16c006b4 */>}}
```

<br>

## 4.本地部署

```bash
hugo server/serve
hugo server -D  # 渲染草稿，即也渲染 draft: true 的内容
```

浏览器打开 [localhost:1313](http://localhost:1313/) 即可看到渲染后的网站。

<br>

## 5.主题自定义

主题配置文件为根目录下的`config.toml`文件。（也可以是`config.yaml`、`config.json`）

<br>

### 5.1 简单配置

```toml
# 域名
baseURL = "https://backtraxe.github.io/"

# 默认语言 [en, zh-cn, ...]
defaultContentLanguage = "zh-cn"

# 语言 [zh-CN, en-us, ...]
languageCode = "zh-CN"

# 是否包括中日韩文字
hasCJKLanguage = true

# 标题
title = "traXe"

# 主题
theme = "DoIt"

[params]
  # 主题版本
  version = "0.2.X"
  # 网站描述
  description = "这是Backsided的博客"

# 作者配置
[author]
  name = "Backsided"
  email = ""
  link = ""

[menu]
  [[menu.main]]
    identifier = "posts"
    # 你可以在名称 (允许 HTML 格式) 之前添加其他信息, 例如图标
    pre = ""
    # 你可以在名称 (允许 HTML 格式) 之后添加其他信息, 例如图标
    post = ""
    name = "文章"
    url = "/posts/"
    # 当你将鼠标悬停在此菜单链接上时, 将显示的标题
    title = ""
    weight = 1

  [[menu.main]]
    identifier = "tags"
    pre = ""
    post = ""
    name = "标签"
    url = "/tags/"
    title = ""
    weight = 2

  [[menu.main]]
    identifier = "categories"
    pre = ""
    post = ""
    name = "分类"
    url = "/categories/"
    title = ""
    weight = 3

# Hugo 解析文档的配置
[markup]
  # 语法高亮设置 (https://gohugo.io/content-management/syntax-highlighting)
  [markup.highlight]
    # false 是必要的设置 (https://github.com/dillonzq/LoveIt/issues/158)
    noClasses = false
```

<br>

### 5.2 高级配置

### 5.3 [params]

```toml
[params]
  # 主题版本
  version = "0.2.X"
  # 描述
  description = "这是Backsided的博客"
  # 关键词
  keywords = ["Theme", "Hugo"]
  # 网站默认主题样式 ("light", "dark", "auto")
  defaultTheme = "auto"
  # 公共 git 仓库路径，仅在 enableGitInfo 设为 true 时有效
  gitRepo = ""
  # 选择哈希函数用来 SRI, 为空时表示不使用 SRI
  # ("sha256", "sha384", "sha512", "md5")
  fingerprint = ""
  # 日期格式
  dateFormat = "2006-01-02"
  # 网站图片, 用于 Open Graph 和 Twitter Cards
  images = ["/logo.png"]
```

#### [params.app]

```toml
# 应用图标配置
[params.app]
  # 当添加到 iOS 主屏幕或者 Android 启动器时的标题, 覆盖默认标题
  title = "LoveIt"
  # 是否隐藏网站图标资源链接
  noFavicon = false
  # 更现代的 SVG 网站图标, 可替代旧的 .png 和 .ico 文件
  svgFavicon = ""
  # Android 浏览器主题色
  themeColor = "#ffffff"
  # Safari 图标颜色
  iconColor = "#5bbad5"
  # Windows v8-10磁贴颜色
  tileColor = "#da532c"
```

#### [params.search]

```toml
# 搜索配置
[params.search]
  enable = true
  # 搜索引擎的类型 ("lunr", "algolia")
  type = "lunr"
  # 文章内容最长索引长度
  contentLength = 4000
  # 搜索框的占位提示语
  placeholder = ""
  # 最大结果数目
  maxResultLength = 10
  # 结果内容片段长度
  snippetLength = 50
  # 搜索结果中高亮部分的 HTML 标签
  highlightTag = "em"
  # 是否在搜索索引中使用基于 baseURL 的绝对路径
  absoluteURL = false

  [params.search.algolia]
    index = ""
    appID = ""
    searchKey = ""
```

#### [params.header]

```toml
# 页面头部导航栏配置
[params.header]
  # 桌面端导航栏模式 ("fixed", "normal", "auto")
  desktopMode = "fixed"
  # 移动端导航栏模式 ("fixed", "normal", "auto")
  mobileMode = "auto"
  # 页面头部导航栏标题配置
  [params.header.title]
    # LOGO 的 URL
    logo = ""
    # 标题名称
    name = "Backsided's World"
    # 你可以在名称 (允许 HTML 格式) 之前添加其他信息, 例如图标
    pre = ""
    # 你可以在名称 (允许 HTML 格式) 之后添加其他信息, 例如图标
    post = ""
    # 是否为标题显示打字机动画
    typeit = true
```

#### [params.footer]

```toml
# 页面底部信息配置
[params.footer]
  enable = true
  # 自定义内容 (支持 HTML 格式)
  custom = ''
  # 是否显示 Hugo 和主题信息
  hugo = true
  # 是否显示版权信息
  copyright = true
  # 是否显示作者
  author = true
  # 网站创立年份
  since = 2021
  # ICP 备案信息，仅在中国使用 (支持 HTML 格式)
  icp = ""
  # 许可协议信息 (支持 HTML 格式)
  license = '<a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>'
```

#### [params.section]

```toml
# Section (所有文章) 页面配置
[params.section]
  # section 页面每页显示文章数量
  paginate = 20
  # 日期格式 (月和日)
  dateFormat = "01-02"
  # RSS 文章数目
  rss = 10
```

#### [params.list]

```toml
# List (目录或标签) 页面配置
[params.list]
  # list 页面每页显示文章数量
  paginate = 20
  # 日期格式 (月和日)
  dateFormat = "01-02"
  # RSS 文章数目
  rss = 10
```

#### [params.home]

```toml
# 主页配置
[params.home]
  # RSS 文章数目
  rss = 10

  # 主页个人信息
  [params.home.profile]
    enable = true
    # Gravatar 邮箱，用于优先在主页显示的头像
    gravatarEmail = ""
    # 主页显示头像的 URL
    avatarURL = "/images/avatar.png"
    # 主页显示的网站标题 (支持 HTML 格式)
    title = ""
    # 主页显示的网站副标题
    subtitle = "这是我的全新 Hugo 网站"
    # 是否为副标题显示打字机动画
    typeit = true
    # 是否显示社交账号
    social = true
    # 免责声明 (支持 HTML 格式)
    disclaimer = ""

  # 主页文章列表
  [params.home.posts]
    enable = true
    # 主页每页显示文章数量
    paginate = 6
```

#### [params.social]

```toml
# 作者的社交信息设置
[params.social]
  GitHub = "xxxx"
  Linkedin = ""
  Twitter = "xxxx"
  Instagram = "xxxx"
  Facebook = "xxxx"
  Telegram = "xxxx"
  Medium = ""
  Gitlab = ""
  Youtubelegacy = ""
  Youtubecustom = ""
  Youtubechannel = ""
  Tumblr = ""
  Quora = ""
  Keybase = ""
  Pinterest = ""
  Reddit = ""
  Codepen = ""
  FreeCodeCamp = ""
  Bitbucket = ""
  Stackoverflow = ""
  Weibo = ""
  Odnoklassniki = ""
  VK = ""
  Flickr = ""
  Xing = ""
  Snapchat = ""
  Soundcloud = ""
  Spotify = ""
  Bandcamp = ""
  Paypal = ""
  Fivehundredpx = ""
  Mix = ""
  Goodreads = ""
  Lastfm = ""
  Foursquare = ""
  Hackernews = ""
  Kickstarter = ""
  Patreon = ""
  Steam = ""
  Twitch = ""
  Strava = ""
  Skype = ""
  Whatsapp = ""
  Zhihu = ""
  Douban = ""
  Angellist = ""
  Slidershare = ""
  Jsfiddle = ""
  Deviantart = ""
  Behance = ""
  Dribbble = ""
  Wordpress = ""
  Vine = ""
  Googlescholar = ""
  Researchgate = ""
  Mastodon = ""
  Thingiverse = ""
  Devto = ""
  Gitea = ""
  XMPP = ""
  Matrix = ""
  Bilibili = ""
  Email = "xxxx@xxxx.com"
  RSS = true
```

#### [params.page]

```toml
# 文章页面配置
[params.page]
  # 是否在主页隐藏一篇文章
  hiddenFromHomePage = false
  # 是否在搜索结果中隐藏一篇文章
  hiddenFromSearch = false
  # 是否使用 twemoji
  twemoji = false
  # 是否使用 lightgallery
  lightgallery = false
  # 是否使用 ruby 扩展语法
  ruby = true
  # 是否使用 fraction 扩展语法
  fraction = true
  # 是否使用 fontawesome 扩展语法
  fontawesome = true
  # 是否在文章页面显示原始 Markdown 文档链接
  linkToMarkdown = true
  # 是否在 RSS 中显示全文内容
  rssFullText = false
```

##### [params.page.toc]

```toml
# 目录配置
[params.page.toc]
  # 是否使用目录
  enable = true
  # 是否保持使用文章前面的静态目录
  keepStatic = true
  # 是否使侧边目录自动折叠展开
  auto = true
```

##### [params.page.code]

```toml
# 代码配置
[params.page.code]
  # 是否显示代码块的复制按钮
  copy = true
  # 默认展开显示的代码行数
  maxShownLines = 10
```

##### [params.page.math]

```toml
# KaTeX 数学公式
[params.page.math]
  enable = true
  # 默认块定界符是 $$ ... $$ 和 \\[ ... \\]
  blockLeftDelimiter = ""
  blockRightDelimiter = ""
  # 默认行内定界符是 $ ... $ 和 \\( ... \\)
  inlineLeftDelimiter = ""
  inlineRightDelimiter = ""
  # KaTeX 插件 copy_tex
  copyTex = true
  # KaTeX 插件 mhchem
  mhchem = true
```

##### [params.page.mapbox]

```toml
# Mapbox GL JS 配置
[params.page.mapbox]
  # Mapbox GL JS 的 access token
  accessToken = ""
  # 浅色主题的地图样式
  lightStyle = "mapbox://styles/mapbox/light-v9"
  # 深色主题的地图样式
  darkStyle = "mapbox://styles/mapbox/dark-v9"
  # 是否添加 NavigationControl
  navigation = true
  # 是否添加 GeolocateControl
  geolocate = true
  # 是否添加 ScaleControl
  scale = true
  # 是否添加 FullscreenControl
  fullscreen = true
```

##### [params.page.share]

```toml
# 文章页面的分享信息设置
[params.page.share]
  enable = true
  Twitter = false
  Facebook = false
  Linkedin = false
  Whatsapp = false
  Pinterest = false
  Tumblr = false
  HackerNews = false
  Reddit = false
  VK = false
  Buffer = false
  Xing = false
  Line = false
  Instapaper = false
  Pocket = false
  Digg = false
  Stumbleupon = false
  Flipboard = false
  Weibo = false
  Renren = false
  Myspace = false
  Blogger = false
  Baidu = false
  Odnoklassniki = false
  Evernote = false
  Skype = false
  Trello = false
  Mix = false
```

##### [params.page.comment]

```toml
# 评论系统设置
[params.page.comment]
  enable = true

  # Disqus 评论系统设置
  [params.page.comment.disqus]
    enable = false
    # Disqus 的 shortname，用来在文章中启用 Disqus 评论系统
    shortname = ""

  # Gitalk 评论系统设置
  [params.page.comment.gitalk]
    enable = false
    owner = ""
    repo = ""
    clientId = ""
    clientSecret = ""

  # Valine 评论系统设置
  [params.page.comment.valine]
    enable = false
    appId = ""
    appKey = ""
    placeholder = ""
    avatar = "mp"
    meta= ""
    pageSize = 10
    lang = ""
    visitor = true
    recordIP = true
    highlight = true
    enableQQ = false
    serverURLs = ""
    # emoji 数据文件名称, 默认是 "google.yml"
    # ("apple.yml", "google.yml", "facebook.yml", "twitter.yml")
    # 位于 "themes/LoveIt/assets/data/emoji/" 目录
    # 可以在你的项目下相同路径存放你自己的数据文件:
    # "assets/data/emoji/"
    emoji = ""

  # Facebook 评论系统设置
  [params.page.comment.facebook]
    enable = false
    width = "100%"
    numPosts = 10
    appId = ""
    languageCode = "zh_CN"

  # [Telegram Comments](https://comments.app/) 评论系统设置
  [params.page.comment.telegram]
    enable = false
    siteID = ""
    limit = 5
    height = ""
    color = ""
    colorful = true
    dislikes = false
    outlined = false

  # [Commento](https://commento.io/) 评论系统设置
  [params.page.comment.commento]
    enable = false

  # [Utterances](https://utteranc.es/) 评论系统设置
  [params.page.comment.utterances]
    enable = false
    # owner/repo
    repo = ""
    issueTerm = "pathname"
    label = ""
    lightTheme = "github-light"
    darkTheme = "github-dark"
```

##### [params.page.library]

```toml
# 第三方库配置
[params.page.library]
  [params.page.library.css]
    # someCSS = "some.css"
    # 位于 "assets/"
    # 或者
    # someCSS = "https://cdn.example.com/some.css"

  [params.page.library.js]
    # someJavascript = "some.js"
    # 位于 "assets/"
    # 或者
    # someJavascript = "https://cdn.example.com/some.js"
```

##### [params.page.seo]

```toml
# 页面 SEO 配置
[params.page.seo]
  # 图片 URL
  images = []

  # 出版者信息
  [params.page.seo.publisher]
    name = ""
    logoUrl = ""
```

#### [params.typeit]

```toml
# TypeIt 配置
[params.typeit]
  # 每一步的打字速度 (单位是毫秒)
  speed = 100
  # 光标的闪烁速度 (单位是毫秒)
  cursorSpeed = 1000
  # 光标的字符 (支持 HTML 格式)
  cursorChar = "|"
  # 打字结束之后光标的持续时间 (单位是毫秒, "-1" 代表无限大)
  duration = -1
```

#### [params.verification]

```toml
# 网站验证代码，用于 Google/Bing/Yandex/Pinterest/Baidu
[params.verification]
  google = ""
  bing = ""
  yandex = ""
  pinterest = ""
  baidu = ""
```

#### [params.seo]

```toml
# 网站 SEO 配置
[params.seo]
  # 图片 URL
  image = ""
  # 缩略图 URL
  thumbnailUrl = ""
```

#### [params.analytics]

```toml
# 网站分析配置
[params.analytics]
  enable = false

  # Google Analytics
  [params.analytics.google]
    id = ""
    # 是否匿名化用户 IP
    anonymizeIP = true

  # Fathom Analytics
  [params.analytics.fathom]
    id = ""
    # 自行托管追踪器时的主机路径
    server = ""
```

#### [params.cookieconsent]

```toml
# Cookie 许可配置
[params.cookieconsent]
  enable = true

  # 用于 Cookie 许可横幅的文本字符串
  [params.cookieconsent.content]
    message = ""
    dismiss = ""
    link = ""
```

#### [params.cdn]

```toml
# 第三方库文件的 CDN 设置
[params.cdn]
  # CDN 数据文件名称, 默认不启用
  # ("jsdelivr.yml")
  # 位于 "themes/LoveIt/assets/data/cdn/" 目录
  # 可以在你的项目下相同路径存放你自己的数据文件:
  # "assets/data/cdn/"
  data = ""
```

#### [params.compatibility]

```toml
# 兼容性设置
[params.compatibility]
  # 是否使用 Polyfill.io 来兼容旧式浏览器
  polyfill = false
  # 是否使用 object-fit-images 来兼容旧式浏览器
  objectFit = false
```

### [markup]

```toml
# Hugo 解析文档的配置
[markup]
  # 语法高亮设置
  [markup.highlight]
    codeFences = true
    guessSyntax = true
    lineNos = true
    lineNumbersInTable = true
    # false 是必要的设置
    # (https://github.com/dillonzq/LoveIt/issues/158)
    noClasses = false

  # Goldmark 是 Hugo 0.60 以来的默认 Markdown 解析库
  [markup.goldmark]
    [markup.goldmark.extensions]
      definitionList = true
      footnote = true
      linkify = true
      strikethrough = true
      table = true
      taskList = true
      typographer = true

    [markup.goldmark.renderer]
      # 是否在文档中直接使用 HTML 标签
      unsafe = true

  # 目录设置
  [markup.tableOfContents]
    startLevel = 2
    endLevel = 6
```

### [author]

```toml
# 作者配置
[author]
  name = "xxxx"
  email = ""
  link = ""
```

### [sitemap]

```toml
# 网站地图配置
[sitemap]
  changefreq = "weekly"
  filename = "sitemap.xml"
  priority = 0.5
```

### [Permalinks]

```toml
# Permalinks 配置
[Permalinks]
  # posts = ":year/:month/:filename"
  posts = ":filename"
```

### [privacy]

```toml
# 隐私信息配置
[privacy]
  [privacy.twitter]
    enableDNT = true
  [privacy.youtube]
    privacyEnhanced = true
```

### [mediaTypes]

```toml
# 用于输出 Markdown 格式文档的设置
[mediaTypes]
  [mediaTypes."text/plain"]
    suffixes = ["md"]
```

### [outputFormats.MarkDown]

```toml
# 用于输出 Markdown 格式文档的设置
[outputFormats.MarkDown]
  mediaType = "text/plain"
  isPlainText = true
  isHTML = false
```

### [outputs]

```toml
# 用于 Hugo 输出文档的设置
[outputs]
  home = ["HTML", "RSS", "JSON"]
  page = ["HTML", "MarkDown"]
  section = ["HTML", "RSS"]
  taxonomy = ["HTML", "RSS"]
  taxonomyTerm = ["HTML"]
```

<br>

## 6.发布

### 6.1 静态页面发布

输入`hugo`，渲染后的静态页面在`public`文件夹中，可将该文件夹中的内容复制到 Web 服务器进行发布。

可用`hugo -d/--destination`或在`config.toml`中修改`publishdir`来指定输出的文件夹。

<br>

### 6.2 Github Pages 发布

1. 在 Github 新建两个仓库：
    - `<USERNAME>.github.io.data` 用于存放内容，仓库名称随意，可设置为私有仓库。
    - `<USERNAME>.github.io` 用于部署页面。

2. 在`MyBlog`根目录下运行命令行，输入以下内容：

```bash
git remote add origin https://github.com/backtraxe/backtraxe.github.io.data.git
git add --all
git commit -m "init blog"
git push --set-upstream origin master -f
```

3. 在 Github 中创建一个 [Personal access token](https://github.com/settings/tokens)，命名随意，勾选`repo`，复制其中内容。

4. 进入`<USERNAME>.github.io.data`仓库，点击`Settings`->`Secrets`，名称为`ACCESS_TOKEN`，内容填入刚才复制的`token`。

5. 然后点击`Actions`->`New workflow`->`set up a workflow yourself`，输入以下内容：

```yml
name: Hugo Deploy

on:
  push:
    branches: [ main ] # 指定分支

  workflow_dispatch: # 允许手动启动

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: 1. Checkout # 克隆仓库
        uses: actions/checkout@v2
        with:
          submodules: true # 启用子模块
          fetch-depth: 0

      - name: 2. Disable quotePath # 解决中文显示问题
        run: git config --global core.quotePath false

      - name: 3. Setup Hugo # 安装 hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          extended: true

      - name: 4. Build Hugo # 渲染页面
        run: hugo --gc --minify --cleanDestinationDir

      - name: 5. Deploy Hugo # 将渲染后的页面复制到指定仓库，然后发布
        uses: peaceiris/actions-gh-pages@v3
        with:
          personal_token: ${{ secrets.ACCESS_TOKEN }}  # 与 Secrets 的名称相同
          external_repository: backtraxe/backtraxe.github.io  # 指定发布的仓库
          publish_branch: main  # 指定发布仓库的分支
          publish_dir: ./public  # 指定要发布的目录
```

<br>

### 6.3 环境迁移

在新设备克隆仓库，同步所有内容。

```bash
git clone --recursive https://github.com/backtraxe/backtraxe.github.io.data.git
```

<br>

## 7.全局详细配置

```toml
# 域名
baseURL = ""

# 构建时包含草稿
buildDrafts = false

# 内容文件夹
contentDir = "content"

# 数据文件夹
dataDir = "data"

# 内容默认语言（中文：zh-cn）
defaultContentLanguage = "en"

# 根目录跳转到默认语言目录
defaultContentLanguageInSubdir = false

# 禁用指定类型页面：page, home, section, taxonomy, term, RSS, sitemap, robotsTXT, 404
disableKinds = []

# 禁用实时重载
disableLiveReload = false

# 禁用将 url/path 转小写字母
disablePathToLower = false

# 启用 Emoji
enableEmoji = false

# 使用文件的最后 git 提交日期更新 Lastmod 参数
enableGitInfo = false

# 启用 inline shortcode
enableInlineShortcodes = false

# 是否生成 robots.txt 文件
enableRobotsTXT = false

# 日期设置
[frontmatter]

# 脚注锚的前缀
footnoteAnchorPrefix = ""

# 脚注返回链接显示的文本
footnoteReturnLinkContents = ""

# Google Analytics 跟踪 ID
googleAnalytics = ""

# 自动检测内容中的中文/日文/韩文
hasCJKLanguage = false

# 图片设置
[imaging]

# 语言设置
[languages]

# 启用日志
log = false

# 日志保存目录
logFile = ""

# 主题设置
[markup]

# 目录设置
[menu]

# 最小化构建设置
[minify]

# 模块设置
[module]

# 每页的默认文章数量
paginate = 10

# 固定链接
[permalinks]

# 生成静态网页的目录
publishDir = "public"

# 相关设置
[related]

# 网站地图设置
[sitemap]

# 静态文件目录设置
staticDir = "static"

# 分类设置
[taxonomies]

# 主题
theme = ""

# 主题目录
themesDir = "themes"

# 标题
title = ""
```

<br>

## 参考

1. [Quick Start | Hugo](https://gohugo.io/getting-started/quick-start/)
2. [GitHub Pages 文档 - GitHub Docs](https://docs.github.com/cn/pages)
3. [Host on GitHub - Hugo](https://gohugo.io/hosting-and-deployment/hosting-on-github/)
4. [开始使用 DoIt - 系列 - DoIt](https://hugodoit.pages.dev/zh-cn/series/getting-start/)
5. [How to Create Your First Hugo Blog: a Practical Guide](https://www.freecodecamp.org/news/your-first-hugo-blog-a-practical-guide/)
6. [创建 GitHub Pages 站点 - Github](https://docs.github.com/cn/github/working-with-github-pages/creating-a-github-pages-site/)
7. [使用Hugo和GitHub搭建博客 - Félix | Medium](https://zhangfelix.medium.com/%E4%BD%BF%E7%94%A8hugo%E5%92%8Cgithub%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2-cbd1d57fcfbf)
8. [Configure Hugo | Hugo](https://gohugo.io/getting-started/configuration/)
9. [Github Action 自动修改文章的更新日期 - 点半九](https://www.dianbanjiu.com/post/github-action-%E8%87%AA%E5%8A%A8%E4%BF%AE%E6%94%B9%E6%96%87%E7%AB%A0%E7%9A%84%E6%9B%B4%E6%96%B0%E6%97%A5%E6%9C%9F/)


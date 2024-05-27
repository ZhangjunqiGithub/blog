---
title: hexo个人博客搭建
categories: hexo个人博客搭建
tags: hexo个人博客搭建
date: 2023/3/18
cover: https://www.sumedu.com/files/default/2018/10-17/0918328d3db4808410.png
---

### hexo个人博客搭建

前提：已安装node.js+git+npm,有gitee/github账号且有自己的**公开的**仓库

1、npm install -g cnpm --registry=https://registry.npm.taobao.org (可不执行此步骤。作用：改为淘宝镜像，加快npm下载速度)

2、npm install hexo-cli -g (安装hexo脚手架，即安装hexo)

3、hexo init blog (通过hexo生成博客项目文件夹。此步骤可以不执行，可以自己在某个位置创建一个空文件夹,在空文件夹目录下打开cmd/git bash执行：hexo init)

4、cd blog (此步骤可以不执行 。在自己创建的博客项目文件夹的目录下打开cmd终端或git bash)

5、hexo server (在本地运行博客项目，自动打开一个服务器。可以通过在浏览器中输入终端生成的地址访问自己的博客项目)

6、npm install hexo-deployer-git --save (注意：要在博客项目文件夹中打开cmd终端或git bash执行该命令。此步骤如果未执行，将无		法将项目部署到gitee/github上)

7、以gitee为例：登录并进入到自己对应的博客的gitee仓库，点击上方导航栏中的”服务“—”Gitee Pages“,然后点击”启动“，就会生成网站		地址

8、打开_config.yml配置文件，进行配置的修改

9、修改对应的配置：(下面为已修改后的配置文件，请自行对照相应的位置进行修改)

```yml
# Hexo Configuration
## Docs: https://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/

# Site
title: 编程分享讨论栈
subtitle: ''
description: ''
keywords:
author: JQ Zhang
language: en
timezone: ''

# URL
## Set your site url here. For example, if you use GitHub Page, set url as 'https://username.github.io/project'
url: https://zhangjunqigithub.github.io/blog/
root: /blog/
permalink: :year/:month/:day/:title/
permalink_defaults:
pretty_urls:
  trailing_index: true # Set to false to remove trailing 'index.html' from permalinks
  trailing_html: true # Set to false to remove trailing '.html' from permalinks

# Directory
source_dir: source
public_dir: public
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
skip_render:

# Writing
new_post_name: :title.md # File name of new posts
default_layout: post
titlecase: false # Transform title into titlecase
external_link:
  enable: true # Open external links in new tab
  field: site # Apply to the whole site
  exclude: ''
filename_case: 0
render_drafts: false
post_asset_folder: false
relative_link: false
future: true
highlight:
  enable: true
  line_number: true
  auto_detect: false
  tab_replace: ''
  wrap: true
  hljs: false
prismjs:
  enable: false
  preprocess: true
  line_number: true
  tab_replace: ''

# Home page setting
# path: Root path for your blogs index page. (default = '')
# per_page: Posts displayed per page. (0 = disable pagination)
# order_by: Posts order. (Order by date descending by default)
index_generator:
  path: ''
  per_page: 10
  order_by: -date

# Category & Tag
default_category: uncategorized
category_map:
tag_map:

# Metadata elements
## https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta
meta_generator: true

# Date / Time format
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD
time_format: HH:mm:ss
## updated_option supports 'mtime', 'date', 'empty'
updated_option: 'mtime'

# Pagination
## Set per_page to 0 to disable pagination
per_page: 10
pagination_dir: page

# Include / Exclude file(s)
## include:/exclude: options only apply to the 'source/' folder
include:
exclude:
ignore:

# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: butterfly

# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: git
  repo: https://github.com/ZhangjunqiGithub/blog.git
  branch: master

# Algolia API配置
algolia:
  applicationID: 'DOUQTLBQKA'
  apiKey: 'a9673e873d0aa7a0ce75624b7fb70120'
  indexName: 'my_hexo_blog'
```

10、准备部署工作。hexo cl (清理缓存)

11、hexo g (生成对应的网站的文件)

12、hexo s (再推送前可以启动服务器，查看自己的网站是否有瑕疵)

13、hexo d (将自己的博客网站部署到gitee服务器上)（如果有弹窗出现输入自己gitee的账号和密码即可，下次 就不会出现弹窗了）

14、更新或发布自己的文章。进入到项目根目录中的source/_posts，在此位置写文章，新建文本文档，把后缀名改为.md，在该文件中		写内容即可。推荐用Typora打开，然后输入  ---  +  enter(回车)，然后输入：   title: 标题名   然后换一行输入：   date: 日期

​		然后再灰色区域下写文章的内容即可。

15、执行hexo cl    ，hexo g     ，hexo d      即可在gitee中更新你的博客项目了。

16、进入Gitee Pages中，更新一下网站地址即可。

17、博客文章开头格式

---

title: welcome
categories: welcome2
tag: welcome2
date: 2022/9/26

cover: 封面图片链接地址
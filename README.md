[![Build Status](https://www.travis-ci.org/williamHappy/williamHappy.github.io.svg?branch=blog_source)](https://www.travis-ci.org/williamHappy/williamHappy.github.io)
# 版本更新说明

---


 **注意说明**
1. 更新主题仓库，需要先备份配置文件等，如`_config.yml`,`images`文件等
2. 更新blog源码（如更新主题）会将上一版本的blog源码打`tag`, 可供小伙伴下载某个`tag`正常blog源码，编译运行测试。
3. blog的主题资源文件（个人文件，如images的相关制作原件，备用文件）
	会备份在百度网盘。我的资源`/myblog_file`

---

### 2018.05.08
更新主题：theme: `hexo-theme-miho`

1. 文件：`themes/hexo-theme-miho/source/css/_partial/article.styl`
```css
{
  .article-entry
    background: #fff                  //will自定义 #f9f9f9修改为#fff
}
```

### 2018.05.07
更新`.travis.yml`配置文件， git仓库梳理。

1. 百度链接的主动推送方式，使用了插件，需要在`hexo d`是推送，所以要更改travisCI的流程，要改配置文件，踩到坑了，详情[查看][2]文章的第六节-->站点收录的**坑二**。
2. 由于使用了travisCI自动构建，自动推送部署，所以会出现主分支和blog分支不一致的情况，看着不舒服，其实没什么影响，以后环境的更换的话，只需要clone博客源分支，写文章提交皆可，不需要clone主分支。

### 2018.05.04

更新主题：theme: `hexo-theme-miho`

1. 主题添加了在线联系daovoice插件，鼠标点击出现桃心插件，rss订阅，sitemap等安装配置详情，请查看我的博客文章[主题配置升级][2]
2. 说明：自此， 以后编写博客文章使用atom， 不再使用作业部落， 作业部落以后可作为记载文章摘要等， 之前文章都与作业部落云端进行了同步（以后有更改不再同步）， 作业部落上有些半成品文章未作处理。

### 2018.05.03

1. 原blog目录文件`blog_readme.txt`，使用`readme.md`文件(即此文件)来替换，使用`readme.md`文件记录博客的重要更新日志。暂不删除`blog_readme.txt`文件。
2. 博客全部的图片文件完成迁移，从github的仓库`FileRepo`迁移到七牛云oss对象存储。github仓库保留，暂不删除。2018.04.27更新作废， 之前的博客文章图片链接已全部更新为七牛云的外链。

---

### 2018.04.27
blog中所有图片文件仓库的迁移，从github的FileRepo迁移到七牛云oss对象存储.
~~从第22篇blog（[git养成日记的脑图][1]）开始，脑图存放在了七牛云存储仓库中，之后blog中所有图片文件或者其他文件都存放在七牛云。~~

---

### 2018.04.25

此版本为v1.0后稳定版本v1.1，自此（18/04/25）后为自定义主题更新，带稳定后立tag版本为v2.0

更新主题：theme: `hexo-theme-miho`

有自定义的部分是有标注注释的，可查看具体的代码文件。

1. 文件`/themes/hexo-theme-miho/source/css/_extend.styl`
```css
{
  h3
  	font-size: 2rem       //will自定义 1.5rem 改为2rem
  h4
  	font-size: 1.5rem       //will自定义 1.2rem改为1.5rem
  h5
  	font-size: 1.3rem       //will自定义 1rem改为1.3rem
}
```

```css
{
  $block
    background: #fff
    box-shadow: 3px 6px 9px #ddd		//will自定义 1px 2px 3px改为3px 6px 9px
    border: 1px solid color-border
    border-radius: 9px		//will自定义 3px改为9px
}
```
<br>

2. 文件`/themes/hexo-theme-miho/source/css/_partial/article.styl`

```css
{
  p, table
  	line-height: line-height
  	//font-size: 1rem       //will自定义 注释掉
  	margin: line-height-margin 0
  	word-wrap: break-word

  h4, h5, h6
  	font-weight: bold;
  修改为：
  h1, h2, h3, h4, h5, h6
  	font-weight: bold;

.article
	border-radius: 9px		//will自定义 0改为9px
}
```
<br>

3. 文件`/themes/hexo-theme-miho/source/css/_variables.styl`

```
  // 主色调
  main-color = #51acf9		//自定义 #0cc修改为#51acf9
```
<br>

4. 文件`/themes/hexo-theme-miho/source/css/_partial/highlight.styl`
```
  //修复部分代码块颜色，具体查看代码文件
```
<br>

5. 文件`/themes/hexo-theme-miho/source/css/_partial/post.styl`
```
  //修改了部分样式，具体查看文件标注
```

---

### 2018.01.19

更换主题：theme: `hexo-theme-miho`， 安装配置说明[点击][3]

1. 原blog目录文件`修改主题样式说明.txt`（即此文件），更名为blog说明文件`blog_readme.txt`
2. 更换主题`hexo-theme-miho `
3. 修改样式
文件：`/hexo-theme-miho/source/css/_variables.styl`(样式值表)
由`color-background = #eee` 改为`color-background = #fff `

---

### 2016.11.27

更新主题：theme: `yelee`, 安装配置[点击][4]

1. 修改了`/yelee/source/css/_variables.styl`
```
mid-col-color = rgba(99,99,99,.8)
article-color = rgba(255,255,255,.75)
```

2. 修改了`/yelee/source/css/article.styl`
```
box-shadow: 3px 2px 8px rgba(0,0,0, .8);
```

[1]: https://github.com/williamHappy/williamHappy.github.io/blob/blog_source/source/_posts/22_2018-04-26_git%E5%85%BB%E6%88%90%E6%97%A5%E8%AE%B0.md
[2]: http://blog.willhappy.cn/2018/05/06/27_2018-05-06_hexo%E4%B8%BB%E9%A2%98%E9%85%8D%E7%BD%AE%E5%8D%87%E7%BA%A7/
[3]: https://github.com/WongMinHo/hexo-theme-miho
[4]: https://github.com/MOxFIVE/hexo-theme-yelee

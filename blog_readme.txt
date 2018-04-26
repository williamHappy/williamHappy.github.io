***************注意说明*********************
1. 更新主题仓库，需要先备份配置文件等，如_config.yml,images文件等
2. 更新blog源码（如更新主题）会将上一版本的blog源码打tag,
	可供小伙伴下载某个tag正常blog源码，编译运行测试。
3. blog的主题资源文件（个人文件，如images的相关制作原件，备用文件）
	会备份在百度网盘。我的资源/myblog_file


2016.11.27---theme: yelee
1.修改了/yelee/source/css/_variables.styl
mid-col-color = rgba(99,99,99,.8)
article-color = rgba(255,255,255,.75)

2.修改了/yelee/source/css/article.styl
box-shadow: 3px 2px 8px rgba(0,0,0, .8);

*********************************************************************************************************************
2018.01.19----theme: hexo-theme-miho
1. 原blog目录文件‘修改主题样式说明.txt’（即此文件），更名为blog说明文件‘blog_readme.txt’

2. 更换主题hexo-theme-miho 

3. 修改样式
文件：\hexo-theme-miho\source\css\_variables.styl(样式值表)
由color-background = #eee 改为color-background = #fff 

***************此版本为v1.0后稳定版本v1.1，自此（18/04/25）后为自定义主题更新，带稳定后立tag版本为v2.0***************
2018.04.----theme: hexo-theme-miho

1. \themes\hexo-theme-miho\source\css\_extend.styl
	h3
		font-size: 1.5rem
	h4
		font-size: 1.2rem
	h5
		font-size: 1rem
	修改为：
	h3
		font-size: 2rem
	h4
		font-size: 1.5rem
	h5
		font-size: 1.3rem
		
	$block
	  background: #fff
	  box-shadow: 3px 6px 9px #ddd		//will自定义 1px 2px 3px改为3px 6px 9px
	  border: 1px solid color-border
	  border-radius: 9px		//will自定义 3px改为9px

2. \themes\hexo-theme-miho\source\css\_partial\article.styl
	p, table
		line-height: line-height
		font-size: 1rem
		margin: line-height-margin 0
		word-wrap: break-word
	修改为：
	p, table
		line-height: line-height
		//font-size: 1rem
		margin: line-height-margin 0
		word-wrap: break-word
		
	h4, h5, h6
		font-weight: bold;
	修改为：
	h1, h2, h3, h4, h5, h6
		font-weight: bold;
		
	.article
		border-radius: 9px		//will自定义 0改为9px
		
3. \themes\hexo-theme-miho\source\css\_variables.styl
	// 主色调
	main-color = #51acf9		//自定义 #0cc修改为#51acf9
	
4. \themes\hexo-theme-miho\source\css\_partial\highlight.styl
	//修复部分代码块颜色，具体查看代码
***************ע��˵��*********************
1. ��������ֿ⣬��Ҫ�ȱ��������ļ��ȣ���_config.yml,images�ļ���
2. ����blogԴ�루��������⣩�Ὣ��һ�汾��blogԴ���tag,
	�ɹ�С�������ĳ��tag����blogԴ�룬�������в��ԡ�
3. blog��������Դ�ļ��������ļ�����images���������ԭ���������ļ���
	�ᱸ���ڰٶ����̡��ҵ���Դ/myblog_file


2016.11.27---theme: yelee
1.�޸���/yelee/source/css/_variables.styl
mid-col-color = rgba(99,99,99,.8)
article-color = rgba(255,255,255,.75)

2.�޸���/yelee/source/css/article.styl
box-shadow: 3px 2px 8px rgba(0,0,0, .8);

*********************************************************************************************************************
2018.01.19----theme: hexo-theme-miho
1. ԭblogĿ¼�ļ����޸�������ʽ˵��.txt���������ļ���������Ϊblog˵���ļ���blog_readme.txt��

2. ��������hexo-theme-miho 

3. �޸���ʽ
�ļ���\hexo-theme-miho\source\css\_variables.styl(��ʽֵ��)
��color-background = #eee ��Ϊcolor-background = #fff 

***************�˰汾Ϊv1.0���ȶ��汾v1.1���Դˣ�18/04/25����Ϊ�Զ���������£����ȶ�����tag�汾Ϊv2.0***************
2018.04.----theme: hexo-theme-miho

1. \themes\hexo-theme-miho\source\css\_extend.styl
	h3
		font-size: 1.5rem
	h4
		font-size: 1.2rem
	h5
		font-size: 1rem
	�޸�Ϊ��
	h3
		font-size: 2rem
	h4
		font-size: 1.5rem
	h5
		font-size: 1.3rem
		
	$block
	  background: #fff
	  box-shadow: 3px 6px 9px #ddd		//will�Զ��� 1px 2px 3px��Ϊ3px 6px 9px
	  border: 1px solid color-border
	  border-radius: 9px		//will�Զ��� 3px��Ϊ9px

2. \themes\hexo-theme-miho\source\css\_partial\article.styl
	p, table
		line-height: line-height
		font-size: 1rem
		margin: line-height-margin 0
		word-wrap: break-word
	�޸�Ϊ��
	p, table
		line-height: line-height
		//font-size: 1rem
		margin: line-height-margin 0
		word-wrap: break-word
		
	h4, h5, h6
		font-weight: bold;
	�޸�Ϊ��
	h1, h2, h3, h4, h5, h6
		font-weight: bold;
		
	.article
		border-radius: 9px		//will�Զ��� 0��Ϊ9px
		
3. \themes\hexo-theme-miho\source\css\_variables.styl
	// ��ɫ��
	main-color = #51acf9		//�Զ��� #0cc�޸�Ϊ#51acf9
	
4. \themes\hexo-theme-miho\source\css\_partial\highlight.styl
	//�޸����ִ������ɫ������鿴����
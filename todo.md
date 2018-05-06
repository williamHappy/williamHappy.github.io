### 2018/04/25
1. 设置blog可切换主题（MiHo和skapp）—----暂缓,skapp还有些差异
2. 图片url关联七牛云，不在存储在github仓库------finished

### 2018/05/04

1. 博客主题更新说明（写blog文章学习）---finished
- live2d 安装配置
- daovoice 安装配置 http://kunkun5love.club/2018/01/25/Hexo-%E4%B8%AA%E6%80%A7%E5%8C%96%E9%85%8D%E7%BD%AE(%E4%B8%89)/#more
- 鼠标点击桃心配置
- more https://segmentfault.com/a/1190000009544924

2. 整理作业部落markdown的文章， 和blog文章数据同步----finished
3. 整理关于我的介绍模块（可附加建立）

测试travisCI提交是否正常

# 使用语言
language: node_js
# node版本(由于依赖nodejieba不支持node稳定版本（stable）,所以使用9.6.1)
#node_js: stable
node_js:
  - '9.6.1'

# 设置只监听哪个分支
branches:
  only:
  - blog_source

# 缓存，可以节省集成的时间，这里我用了yarn，如果不用可以删除
cache:
#  apt: true
#  yarn: true
  directories:
    - node_modules

# 构建提交方式改变， 由原来hexo只负责生成，travis推送git仓库，更改为通过hexo d来推送git仓库，是为了同时主动推送百度链接
before_install:
  - npm install -g hexo-cli

# Start: Build Lifecycle
install:
  - npm install
  - npm install hexo-deployer-git --save

# 执行清缓存，生成网页操作
script:
  - hexo clean
  - hexo generate

# 设置git提交名，邮箱；替换真实token到_config.yml文件，最后depoy部署
after_script:
  - git config user.name "williamhappy"
  - git config user.email "ctwang1994@163.com"
  # 替换同目录下的_config.yml文件中gh_token字符串为travis后台刚才配置的变量，注意此处sed命令用了双引号。单引号无效！
  - sed -i "s/gh_token/${GH_TOKEN}/g" ./_config.yml
  - sed -i "s/baidu_token/${Baidu_Url_Submit_Token}/g" ./_config.yml
  - cat ./_config.yml
  - hexo deploy
# End: Build LifeCycle


# 2018/05/05之前的配置
# tarvis生命周期执行顺序详见官网文档
# before_install:
#   - git config --global user.name "williamhappy"
#   - git config --global user.email "ctwang1994@163.com"
#   # 由于使用了yarn，所以需要下载，如不用yarn这两行可以删除
#   - curl -o- -L https://yarnpkg.com/install.sh | bash
#   - export PATH=$HOME/.yarn/bin:$PATH
#   - npm install -g hexo-cli
#   #- npm install -g node-gyp
#   #- npm install nodejieba
#
# # 安装依赖
# install:
# # 不用yarn的话这里改成 npm i 即可
#   - yarn
#   #- npm i
#
# # 部署命令
# script:
#   - hexo clean
#   - hexo generate
#
# # 提交内容及git 用户信息
# after_success:
#   - cd ./public
#   - git init
#   - git add --all .
#   - git commit -m "Travis CI Auto Builder"
#   # 这里的 GH_TOKEN 即之前在 travis 项目的环境变量里添加的
#   - git push --quiet --force https://$GH_TOKEN@github.com/williamhappy/williamhappy.github.io.git
#     master

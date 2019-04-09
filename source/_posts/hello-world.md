---
title: hexo+github搭建个人博客
---
使用github pages服务搭建博客的好处有：
1、全是静态文件，访问速度快；
2、免费方便，不用花一分钱就可以搭建一个自由的个人博客，不需要服务器不需要后台；
3、数据绝对安全，基于github的版本管理，想恢复到哪个历史版本都行；
4、博客内容可以轻松打包、转移、发布到其它平台；等等；


### 安装所需环境

``` LYD
安装Git
安装Node.js
安装Hexo
GitHub创建个人仓库
生成SSH添加到GitHub
将hexo部署到GitHub
设置个人域名
发布文章
```

### 1、安装Git

windows：到git官网上下载,Download git,下载后会有一个Git Bash的命令行工具，用这个工具来使用git。

linux：只需要一行代码就搞定了：
  ``` LYD
  sudo apt-get install git
  ```


### 2、安装nodejs
windows：nodejs选择LTS版本就行了。

linux：
  ``` LYD
  sudo apt-get install nodejs
  sudo apt-get install npm
  ```

### 3、安装hexo

打开git bash，输入命令：
``` LYD
npm install -g hexo-cli
# 初始化
hexo init myBlog
cd myBlog
npm install
```

新建完成后会生成文件目录如下：

- node_modules: 依赖包
- public：存放生成的页面
- scaffolds：生成文章的一些模板
- source：用来存放你的文章
- themes：主题
- _config.yml: 博客的配置文件

``` LYD
hexo g
hexo server # 或者 hexo s
```

在浏览器输入localhost:4000就可以看到你生成的博客了。

<img src="/img/hexo.jpg" class="full-image" />


### 4、GitHub创建个人仓库

首先New repository，新建一个仓库

创建一个和你用户名相同的仓库，xxx.github.io，只有这样，将来要部署到GitHub page的时候，才会被识别，其中xxx就是你注册GitHub的用户名。

<img src="/img/github_name.png" class="full-image" />

点击create repository。


### 5、生成SSH添加到GitHub

在git bash中输入：

``` LYD
git config --global user.name "yourname"
git config --global user.email "youremail"
```
然后创建SSH,一路回车

``` LYD
ssh-keygen -t rsa -C "youremail"
```
这个时候就会生成.ssh文件夹。

<img src="/img/git_bash.jpg" class="full-image" />

id_rsa是你这台电脑的私人秘钥，不能给别人看的，id_rsa.pub是公共秘钥，可以随便给别人看。把这个公钥放在GitHub上，这样当你链接GitHub自己的账户时，它就会根据公钥匹配你的私钥，当能够相互匹配时，才能够顺利的通过git上传你的文件到GitHub上。
在github上找到Settings。把公钥放在Key里，Title随便起个就行。

<img src="/img/ssh.png" class="full-image" />

检测是否成功：
``` LYD
ssh -T git@github.com
```

### 6、将hexo部署到GitHub

打开已经装好的hexo。打开站点配置文件 _config.yml找到deploy修改：

``` LYD
deploy:
  type: git
  repo: https://github.com/leiyunduo/leiyunduo.github.io.git  #此地址为github仓储地址
  branch: master
```

这个时候需要先安装deploy-git ,这样你才能用命令部署到GitHub。

``` LYD
npm install hexo-deployer-git --save
```

安装完成后输入命令：

``` LYD
hexo clean # 清除之前生成的东西
hexo generate #或 hexo g  生成静态文章
hexo deploy #或 hexo d  部署文章
```
部署成功后，就可以在http://yourname.github.io 看到你的博客了。


### 7、设置个人域名

如果是已有域名，可以直接进行解析

<img src="/img/yu.png" class="full-image" />

登录GitHub，进入之前创建的仓库，点击settings，设置Custom domain，输入你的域名

<img src="/img/github_pages.png" class="full-image" />

然后在你的hexo文件source中创建一个名为CNAME文件，不要后缀。写上你的域名。

<img src="/img/chame.png" style="width:300px;height:120px;" />

最后输入命令

``` LYD
hexo clean
hexo g
hexo d
```

接下来再打开你的浏览器，输入你自己的域名，就可以看到搭建的网站啦。

如果你想给hexo更换主题，那么继续。
hexo官网有很多主题列表，选择一个自己喜欢的。我选择的是Annie

``` LYD
git clone https://github.com/Sariay/hexo-theme-Annie.git
```

将克隆下的代码放在theme中。theme目录是用来放主题的。
剩余的配置每个主题都有说明，可以自己浏览一下。


### 8、git分支进行多终端工作

现在的问题是由于`hexo d`上传部署到github的其实是hexo编译后的文件，是用来生成网页的，不包含源文件。如果我们换一个电脑就没有源文件，无法进行更改。所以现在要上传源文件使文件同步。

首先在github存储库中新建分支hexo。

<img src="/img/branch.png" />

在Settings下Branches中修改默认分支为hexo分支（这样每次同步源文件的时候就不用指定分支了）。

<img src="/img/moren.png" />

然后在本地新建一个文件夹，打开git bash，克隆hexo分支下的代码。

``` LYD
git clone https://github.com/leiyunduo/leiyunduo.github.io.git
```

接下来在克隆到本地的文件夹`leiyunduo.github.io`中，把除了.git 文件夹外的所有文件都删掉（注意是除了.git）。
把之前我们写的博客源文件全部复制过来，除了`.deploy_git`。

然后

``` LYD
git add .
git commit –m "xxxx"
git push 
```

OK了。

在github上查看。其中node_modules、public、db.json已经被忽略掉了，没有关系，不需要上传，因为在别的电脑上需要重新输入命令安装 。


### 9、更换电脑

换了电脑步骤还是跟上面一样。但是不需要初始化`hexo init`。直接`git clone https://github.com/leiyunduo/leiyunduo.github.io.git`

然后

``` LYD
cd xxx.github.io
npm install
npm install hexo-deployer-git --save
hexo g # 生成
hexo d # 部署
```

每次结束之前记得提交源文件

``` LYD
git add .
git commit –m "xxxx"
git push 
```

### 10、coding page上部署实现国内外分流

之前我们已经把hexo托管在github了，但是github是国外的，而且百度的爬虫是不能够爬取github的，所以如果你希望你做的博客能够在百度引擎上被收录，而且想要更快的访问，那么可以在国内的coding page做一个托管，这样在国内访问就是coding page，国外就走github page。

#### 1.申请coding账户，新建项目

<img src="/img/coding.png" />

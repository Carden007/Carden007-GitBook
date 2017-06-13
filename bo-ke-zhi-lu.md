### 

> # 博客之路

---

![](http://upload-images.jianshu.io/upload_images/1881355-511b93da3e3d303c.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/1080/q/50)

本来觉得写简书也挺好，但是看得太久，始终觉得还是自己造一个吧，毕竟自己造的博客主题偏好通通自己定，更加简约大方呀，最重要的是，反正又不要钱，哈哈！

* ### Git

![](https://cl.ly/3n1p0I0Z1J12/201e5b6628576d77d356d017a61790e8_b.png)

* ### Node.js

1、下载nvm

```
$ curl https://raw.github.com/creationix/nvm/master/install.sh | sh
```

2、下载python 3.5的环境变量

```
$ curl -o-https://raw.githubusercontent.com/creationix/nvm/v0.25.4/install.sh | bash
```

3、重启Terminal

```
$ nvm install stable
```

* ## Hexo

1、Terminal

```
$ sudo npm install -g hexo-cli
```

2、安装完成

![](https://cl.ly/1F471Y1Y2A1t/8e3cb09a3401bd4a253423d9035b1844_b.png)

3、建站

```
$ sudo hexo init <自己指定的文件夹>
```

4、初始npm

```
$ cd <folder>
```

```
$ npm install
```

5、编译文件

```
$ hexo g
```

6、web查看

```
$ hexo s
```

7、打开Terminal下方网址

```
Ctr+C关闭测试
```



* ## GitHub



1、 修改\_config.yml



根据第一篇教程的进度，在这里应该已经创建了Hexo，在文件夹中找到 \_config.yml 文件，我是用Sublime Text打开的。



!\[\]\(https://cl.ly/3f2X0s382O0F/v2-4ccd648c24f4a275fe70210f084a5ce5\_b.png\)



找到 \_config.yml 中的 deploy 标签，改成下图所示形式并保存。注意我这里写的是自己的repository，你也要写你自己的。另外，冒号后面必须加一个英文空格，否则会报错——虽然这里原教程就提到了，但我一开始做的时候依然疏忽了一次。



!\[\]\(https://cl.ly/0E1h1H053o0I/v2-5b2ef14a3d8770b10b988d5f5a7ee501\_b.png\)



\#\# 将其 deploy 到仓库中



打开terminal，一次输入下面三行，记得先cd到原先创建的文件夹，我这里是Hexo。



\`\`\`

hexo clean

hexo generate

hexo deploy

\`\`\`



你将会看到下图的反馈（忽略图中“hexo hexo generate ”这个低级错误）。



!\[\]\(https://cl.ly/0T262M1i3e0o/v2-8d3f1304ad9f51fc1cdde9773f5c4bf5\_b.png\)



然后将会看到报错，这是正常的：



!\[\]\(https://cl.ly/043w2p0l1o2w/v2-b103b494ff4cf7ae88c1d186109b3268\_b.png\)



此时回到刚才的 \_config.yml 文件，再次找到 deploy 标签，将deploy 的 type 改成 git：



!\[\]\(https://cl.ly/1N3f432m2j1g/v2-b1734ac0e79d7a22cfe10df0db22fccf\_b.png\)



回到terminal，依次输入：



\`\`\`

npm install hexo-deployer-git --save

hexo clean

hexo generate

hexo deploy

\`\`\`



不出意外的话将会看到这个（第一次部署的时候，会让你输入自己的账号和密码，这里忘记截图了，注意是账号，不是后面所说的GitHub仓库地址）：



!\[\]\(https://cl.ly/043w2p0l1o2w/v2-b103b494ff4cf7ae88c1d186109b3268\_b.png\)



这个时候blog就已经部署到 GitHub 上了，直接可以查看GitHub仓库——形如\[http://your\_user\_name.github.io\]\(http://your\_user\_name.github.io\)。例如我的是\[ipreacher.github.io\]\(ipreacher.github.io\)，这个时候已经变成了下图这个样子（下面这幅图实际上是\[GitHub - ipreacher/ipreacher.github.io\]\(GitHub - ipreacher/ipreacher.github.io\)，而不是\[ipreacher.github.io\]\(ipreacher.github.io\)）。



!\[\]\(https://cl.ly/3x2N1W1N3n2S/v2-95d00cc0cb8d5ff8113feb41b656ab28\_b.png\)



\# 发表博文



\#\# 新建博文



打开terminal，cd到对应的文件夹，例如我是Hexo，输入：



\`\`\`

hexo new '文章题目'

\`\`\`



我创建博文标题为“wonderful”：



!\[\]\(https://cl.ly/2b053e3V3538/v2-52ead6297a3af394a7bbae5bacfe56f4\_b.png\)



然后在 Hexo/source/\_posts 中就可以找到刚才新建的博文：



!\[\]\(https://cl.ly/120x1c3F143s/v2-262f7cccb3bc11397fe6cb9179a0adf3\_b.png\)



当然可以在 Hexo/source/\_posts 中直接新建 .md 文件，效果是一样的。



\#\# 新建页面



输入这一句：



\`\`\`

hexo new page '页面名称'

\`\`\`



!\[\]\(https://cl.ly/151T1y2d2j1g/v2-fc89db2ba39a86e89116f132f8405694\_b.png\)



然后在 /Hexo/source 中就可以看到刚才新建的页面，例如我新建了“wonderful”这个页面：



!\[\]\(https://cl.ly/1k451X3O403A/v2-d38408fd9f2910e8ba315bb6dd1d2469\_b.png\)



\#\# 写博文



用 Sublime Text 打开新建的博文，是下图这个样子的，“---”就是博文的正文：



!\[\]\(https://cl.ly/313x2f2n1K0b/v2-5cd254f09861faf45519eb75349fab07\_b.png\)



\#\# Markdown



&gt; 因为我们的博文都是用Markdown语言写的，所以首先，你需要一个好用的Markdown编辑器。其实好用的Markdown编辑器一大堆，这里就给大家推荐两个，如果你用的不习惯也可以换其它的。



&gt; 本地编辑器：Haroopad，非常小众的一款Markdown编辑器，左边编辑右边实时预览效果，非常轻便；



&gt; 在线编辑器：MaHua，也是比较小众的一款Markdown编辑器，但效果确实很棒，我的这篇博文就是用MaHua写的。



&gt; 作者：Crossin

&gt; 链接：Hexo\(2\)-部署博客及更新博文 - Crossin的编程教室 - 知乎专栏

&gt; 来源：知乎

&gt; 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

&gt; 原教程推荐了Markdown——入门指南，我还只是草草地扫了几眼，没有仔细看，后面有跟进的话会单独写一篇文章的。



\#\# 发博文



实际上到第三步为止，博文就已经写好了（尽管正文是空的），然而写好的文博需要发表才能被别人看到，因而输入：



\`\`\`

hexo clean

hexo generate

\(若要本地预览就先执行 hexo server\)

hexo deploy

\`\`\`



打开自己的GitHub仓库地址就可以看到发表的博文了，下图是我实践的成果（主题有变换，这会在下一篇文章中叙述）：



!\[\]\(https://cl.ly/0c2y0J0c222F/v2-f439328b620c5f7ba255c021f0e38e75\_b.png\)


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

![](/assets/屏幕快照 2017-06-13 下午4.16.33.png)

2、 deploy到仓库\(第一次部署需要输入GitHub账号和密码\)

```
$ npm install hexo-deployer-git --save
```

```
$ hexo clean
```

```
$ hexo generate
```

```
$ hexo deploy
```

3、部署成功，web访问

```
http://your_user_name.github.io
```

* # Blog

1、新建

```
$ cd <floder>
```

```
$ hexo new '文章题目'
```

2、编辑

```
<floder>/source/_posts
```

3、发布

```
$ hexo clean
```

```
$ hexo generate
```

```
$ hexo deploy
```

* # Theme

1、clone

```
$ cd <floder>
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```

2、修改\_config.yml

```
theme: next
language: en/zh-Hans
title: Carden's Blog
author: Carden
avatar: http://upload-images.jianshu.io/upload_images/645301-f2f93c2f3f4319cc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240
```

3、验证

```
$ hexo s —debug
```

4、配置themes/next/\_config.yml，搜索Scheme

```
·    Muse - 默认 Scheme，这是 NexT 最初的版本，黑白主调，大量留白
·    Mist - Muse 的紧凑版本，整洁有序的单栏外观
·    Pisces - 双栏 Scheme，小家碧玉似的清新
```




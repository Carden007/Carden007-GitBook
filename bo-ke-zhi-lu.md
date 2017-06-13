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



* ##  Hexo

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



然后问题就来了，Terminal没反应，包括随后的几个步骤都是得不到正确回应的。

!\[\]\(https://cl.ly/0e1M0s2N2l2Q/3ff92fe772838c963c2fd0a3f711dfe0\_b.png\)

!\[\]\(https://cl.ly/213R3n1y1I26/8954b39b2859f8ffe38b94039b71877c\_b.png\)



4、和上面的问题一样，改进代码，得到

!\[\]\(https://cl.ly/2g442o22403R/b5960ac46961922c957c2d5d1e96845a\_b.png\)



5、完成时应该是这样。

!\[\]\(https://cl.ly/2c452n2V1Y0q/f80fab4db83a750ca490b3485eb0fb34\_b.png\)



本地测试



1、cd到刚才创建的文件夹，否则将会是这样。

!\[\]\(https://cl.ly/2w3G3A3D0b3E/8954b39b2859f8ffe38b94039b71877c\_b-2.png\)



2、cd到刚才创建的文件夹，例如我就是



\`\`\`

$ cd hello

\`\`\`



回车后输入



\`\`\`

$ hexo g

\`\`\`



!\[\]\(https://cl.ly/0G0I2x001A0K/c32299cd174dde5306e883d204cc4403\_b.png\)



3、再输入



\`\`\`

$ hexo s

\`\`\`



!\[\]\(https://cl.ly/3Y0E2K2w0h3B/8641db60d6dda131db6fb71439a51d03\_b.png\)



4、查看成果的时候到了，打开Terminal下方的网址，大功告成。当然，这步可能会非常慢，我等了好久，以至于以为又出了什么bug。

!\[\]\(https://cl.ly/3Q2j0J2j1214/c279db8f0cb58ec64942829b1b894a2b\_b.png\)



5、最后关闭测试。

!\[\]\(https://cl.ly/141K2B0B3a0K/cedc017f6f3982838de0912f552edcec\_b.png\)



\# 最后



最后就先写到这吧，早上起来再修改。



各位有任何疑问或者批评建议，欢迎留言


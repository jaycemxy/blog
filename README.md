# quick start
## 安装hexo
```
$ sudo npm install -g hexo  // 不加sudo可能安装失败
```

## 部署hexo
```
$ hexo init
```

## 文章相关
创建一篇文章
```
$ hexo new "my new blog"
```
可以在_config.yml下配置post_asset_folder: true，这样在生成一篇新文章的同时会生成一个同名文件夹，可用来存放该篇博客使用到的图片

生成文章
```
$ hexo generate 简写为 hexo g
```

开启本地服务
```
$ hexo server 简写为 hexo s
$ hexo s -p 5000  // 开启本地服务端口号为5000
```

复合命令
```
$ hexo s -g  生成并开启本地服务
$ hexo d -g  生成并部署到线上
```

发布到线上
```
$ hexo deploy 简写为 hexo d
```

生成的网页出错，而生成的rss没有清除，可以重新生成
```
$ hexo clean  // 清理已经生成的静态文件后重试
```

## hexo原理
hexo在执行 $ hexo generate 时会在本地先把博客生成的一套静态站点放到public文件夹中，在执行 $ hexo deploy 时将其复制到.deploy文件夹中。Github的版本库通常建议同时附上README.md说明文件，但是hexo默认情况下会把所有md文件解析成html文件，所以即使在线生成了README.md，它也会在你下一次部署时被删去。解决方法是：在执行 $ hexo deploy 前把在本地写好的README.md文件复制到.deploy文件夹中

## 配置
全局设置：在博客目录下有一个_config.yml的文件，里面可以配置信息
局部配置：在 \themes\博客主题\_config.yml 目录下

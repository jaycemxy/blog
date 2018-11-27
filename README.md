# quick start
安装hexo
```
$ npm install -g hexo-cli
```

创建一篇文章
```
eg:
hexo new "my new blog"
```
可以在_config.yml下配置post_asset_folder: true，这样在生成一篇新文章的同时会生成一个同名文件夹，可用来存放该篇博客使用到的图片

生成文章
```
hexo generate 简写为 hexo g
```

开启本地服务
```
hexo server 简写为 hexo s
```

上面两步也可直接写为一句
```
hexo s -g  意为生成并开启本地服务
```

发布到线上
```
hexo deploy 简写为 hexo d
```
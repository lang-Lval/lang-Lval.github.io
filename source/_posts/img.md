---
title: hexo文章中插入图片问题
categories: 主题相关
date: 2019-03-04 14:26:42
---
### hexo内本地图片直接引入无法显示问题
1. 把主页配置文件_config.yml 里的post_asset_folder:这个选项设置为true
2. 在根目录下输入npm install hexo-asset-image --save，这是下载安装一个可以上传本地图片的插件
3. 再运行hexo new "xxxx"来生成md博文时，/source/_posts文件夹内除了xxxx.md文件还有一个同名的文件夹
4. 最后在xxxx.md中想引入图片时，先把图片复制到xxxx这个文件夹中，然后只需要在xxxx.md中按照markdown的格式引入图片即可

如：
```
//![你想输入的替代文字](xxxx/图片名.jpg)
```
5. hexo d 上传后就能看到图片了
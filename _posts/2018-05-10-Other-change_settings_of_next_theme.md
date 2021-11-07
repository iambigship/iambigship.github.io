---
layout: post
title:  "【其他】Hexo框架 Next 主题的一些设置细节"
date:   2018-05-10 20:42:04
categories: Other
---

最近准备给个人站点增加畅言评论功能，就在 Next 设置中修改，也不知道修改错什么地方了，最后生成站点时，炸了，只好重新建了一次站点。

还好 Hexo 建站很方便，但是之前 Next 主题好多设置都要一一修改，好多都忘了怎么改了，并且最新的 Next 已经升级到了 6.0，只好对比之前的配置文件，慢慢摸索。

为了防止下次再遇到站点炸了的情况，记录下一些细节。

### 以下设置，主要是在 Hexo\themes\next 的 _config.yml 做修改：

1. 侧边栏头像

   avatar: /images/luFei.jpg

   > 图片路径：Hexo\themes\next\source\images\luFei.jpg

2. 关闭加载动画

   以前是 use_motion，现在（6.0）改为 motion 了，设置更详细了，有个总开关叫 enable，把 true 改为 false 就可以关闭加载动画了；

3. 显示语言

   在 Hexo\themes\next\languages 找到需要的语言，比如 zh-CN.yml（中文简体）；

   在 Hexo\\_config.yml 中设置 language 为 zh-CN；


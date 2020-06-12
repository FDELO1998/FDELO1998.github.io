---
title: hexo主题搭建博客主题文件不生效
date: 2020-03-15
tags: HEXO
---

## 建立hexo主题搭建博客的注意的地方：
一开始搭建的时候按照网上教程一步步来，都很顺利。直到我直接把主题文件放进去，发现第一主题没生效，二主题文件是灰的，修复了之后网页又是404了。
主题没生效：把原有主题文件里的config.yml文件删掉
主题文件灰的：删除后通过git重新上传
404：在主目录，是主目录下的config配置文件下面这段话一定要配置，因为忽视这里真是耽误了我好几个小时。
```
Docs: https://hexo.io/docs/deployment.html 
     deploy: 
     type: git 
     repo: git@github.com:FDELO1998/FDELO1998.github.io.git 
     branch: master 
     message:
```


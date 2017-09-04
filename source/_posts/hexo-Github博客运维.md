---
title: hexo+Github博客运维
date: 2017-09-04 21:26:34
tags:
  - Hexo
  - Github
categories: 
  - blog-management
description: 
---
# hexo+GitHub博客运维
---
## 新建博客
```
hexo new title
```
&ensp;&ensp;hexo会生成 `./source/_posts/demo.md`
&ensp;&ensp;打开该文件，我们会发现hexo为我们自动生成了文章的标题和创建时间，另外可以自己添加标签和分类
```
---
title: demo
date: 2017-08-29 21:40:27
tags:
---
```
&ensp;&ensp;其实在`./scaffold`下，我们可以修改模板文件，hexo可以自动根据模板生成文章的题头。
&ensp;&ensp;打开`./scaffold/post.md`文件，显示如下
```
---
title: {{ title }}
date: {{ date }}
tags:
---
```
&ensp;&ensp;我们将它修改为
```
---
title: {{ title }}
date: {{ date }}
tags:
categories:
description:
---
```
&ensp;&ensp;这个时候我们再新建一个文章
```
hexo new demo2
```
&ensp;&ensp;可以看到此时`./sourse/_post/demo2.md`内容如下：
```
---
title: demo2
date: 2017-08-29 21:53:06
tags:
categories:
description:
---
```
&ensp;&ensp;我们就可以手动在此添加 **标签** 和 **分类** 了。具体如何添加标签和分类，参考Next官方文档

 - [添加【分类】页面](http://theme-next.iissnan.com/theme-settings.html#categories-page)
 - [添加【标签】页面](http://theme-next.iissnan.com/theme-settings.html#tags-page)

---
## 管理我的博客
### 多台PC上同步管理
```
git pull #同步更新 
hexo new post "新建文章" #简写形式 hexo n "新建文章" 
hexo clean #清除旧的public文件夹 
hexo generate #生成静态文件 简写形式 hexo g 
hexo deploy #发布到github上 简写形式 hexo d 
git add . #添加更改文件到缓存区 
git commit -m "更新说明" #提交到本地仓库 
git push -u origin master #推送到远程仓库进行备份
```
&ensp;&ensp;每次都这样手动来部署静态博客会感觉非常麻烦，参考[Hexo的版本控制和持续集成](https://formulahendry.github.io/2016/12/04/hexo-ci/)一文，我实现了对我的博客的自动部署。
&ensp;&ensp;至此`hexo d -g`这个命令已经无需在本地运行了。

### 如何删除文章并同步
&ensp;&ensp;Q：在使用的过程中，我发现虽然上述操作在添加文章时非常方便，但是一旦需要删除一篇文章，从一台PC上`push`到`GitHub`后，在另一PC`git pull`的时候会产生冲突。如何解决这一问题呢？
&ensp;&ensp;A：没有什么好的办法，利用`git diff`命令找到冲突的原因后在本地删除之，然后再进行`git pull`

### 如何从Github上删除一个文件/文件夹，而不影响本地文件
&ensp;&ensp;首先将该文件/文件夹加入`.gitignore`，然后执行以下命令，将其从`暂存区域`中删除。（不影响本地文件）

    git rm -r --cached some-directory

&ensp;&ensp;然后执行以下命令提交到本地的`Git仓库`中

    git commit -m "Remove the now ignored directory some-directory"
    
&ensp;&ensp;最后`push`到`Github`上的仓库中。

    git push origin master

## 其它功能的探索
### 为我的Hexo添加注脚功能
&ensp;&ensp;参见[hexo-footnotes](https://github.com/LouisBarranqueiro/hexo-footnotes)，注意注脚只能用数字添加，此插件不能识别字母。

### 利用Hexo画流程图和序列图
&ensp;&ensp;3.3.8版本的hexo其实是支持流程图和序列图的，但是其代码块的标记分别是`flowchart`和`sequence`，和`cmd markdown`的语法有一些不同。
&ensp;&ensp;除此以外，有两个插件可供使用：

 - [hexo-filter-flowchart](https://github.com/bubkoo/hexo-filter-flowchart)
 - [hexo-filter-sequence](https://github.com/bubkoo/hexo-filter-sequence)

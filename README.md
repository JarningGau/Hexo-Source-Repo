# Hexo-Source-Repo
Source-Repo

## Hexo的一些常用命令

```shell
# step1 同步远程仓库
git pull #同步更新

# step2 新建文章
hexo new "新建文章" # -- hexo n "新建文章"

# step3 检查排版 (optional)
hexo server # 启动本地服务 -- hexo s
hexo s -p 4444 # 遇到端口占用时更改端口号

# step4 清除旧的public文件夹
hexo clean 

# step5 生成今天文件并部署
hexo generate # -- hexo g
hexo deploy # -- hexo d
# or
hexo d -g

# step6 提交更新至github
git add . #添加更改文件到缓存区
git commit -m "更新说明" 
git push -u origin master #推送到远程仓库进行备份
```



## Hexo之next主题设置首页不显示全文(只显示预览)

两种方式：

- 进入themes/next目录，修改配置文件`__config.yml`

```
# Automatically Excerpt. Not recommend.
# Please use <!-- more --> in the post to control excerpt accurately.
auto_excerpt:
  enable: false # 在这里，将false改为ture，但是不推荐这样做
  length: 150
```

- 在文章中通过插入`<!-- more -->`手动控制，推荐这样做。

  


## git笔记

### git工作原理

**Workspace** :工作区，就是你平时存放项目代码的地方将自己的文件，添加到缓存区（断网和不断网都可以操作，想知道原理可以自己深挖）
**Index / Stag**e:暂存区，用于临时存放你的改动，事实上它只是一个文件，保存即将提交到文件列表信息
**Repository** :仓库区（或本地仓库），就是安全存放数据的位置，这里面有你提交到所有版本的数据。其中HEAD指向最新放入仓库的版本
**Remote**: 远程仓库，托管代码的服务器，可以简单的认为是你项目组中的一台电脑用于远程数据交换

### git全局设置

```
git config --global user.name "黄意雯"
git config --global user.email "lishucodehyw@gmail.com"
```

### git提交到远程仓库

```
# 将文件提交到暂存区
git add .
# 提交到本地仓库
git commit -m "${describtion}"
# 链接远程仓库
git remote add origin https://gitee.com/hyw_lishu/traning.git
# 上传到远程main分支上
git push -u origin "main" 
```

### git下载某个文件

```
# 允许克隆子目录
git config core.sparsecheckout true

```


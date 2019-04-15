# Git常用指令

## 提交并推送到服务器

$ git add .
$ git commit -m "提交说明"
$ git push -u origin master

## 添加忽略文件并删除远程对应文件

1. 移除git管理  
git rm -r --cached 特定文件，如**/application.properties
2. 在.gitignore中添加忽略
3. 更新git管理  
4. git add .
5. 提价修改到本地  
git commit -m '修改忽略文件'
6. 推送到远程仓库
git push original master

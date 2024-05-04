## Github远程仓库与本地关联

#### 远程-->本地

1. 在GitHub上创建一个新的远程仓库（无法在本地创建远程仓库）。

2. 将本地Git仓库链接到远程仓库，使用

   ```shell
   git remote add origin <分支名>	#origin是别名，可以设置为其他名称
   
   git remote -v			#检查，列出所有远程仓库别名及URL
   ```

3. 对特定分支进行跟踪

   ```shell
   #添加关联：
   git branch --set-upstream-to <远程分支名> []
   	branch -u <远程分支名>			#指定本地分支与远程分支关联
   	（不建议）branch --track			#（指定远程分支关联本地分支
   #取消映射：
   git branch --unset-upstream <本地分支名>	#取消本地映射到远程
   			--unset-upstream  <远程分支名> #取消远程映射到本地（用于取消--track）
   
   git branch -vv			#列出所有分支的与上游的关联情况
   ```

4. 远程内容拉取到本地：

   ```shell
   git fetch 		#到本地仓库
   git diff		#比较不同
   git pull		#拉取合并
   ```

   

#### 本地-->远程

1. 分支：

   ```shell
   #通过本地创建远程分支：
   git push <repo> <new_branch>#将本地的分支推送到远程，如果不存在new_branch会自动创建
   ```

2. 本地内容推送到远程

   ```shell
   git push -u origin main   	#仅在首次连接时需要-u（--set-upstream-to)
   #注意，新分支不要-u，如果远程没有新分支会报错，见1.
   git push 					#git会记住上一次push的仓库
   
   ```

3. 







#### 问题汇总

- refs：指针，设计git的数据结构
- ssl注册


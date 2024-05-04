## Git本地仓库的基本使用方法

[TOC]

[^提要]: 该文未加入远程仓库内容，仅作为本地仓库入门使用

#### 一，身份创建

设置用户名

```shell
git config --global user.name "Your Name"
```

设置电子邮件地址

```shell
git config --global user.email "your.email@example.com"
```

1. --global代表在所有本地仓库都使用该身份
2. 要对单个仓库设定身份，去掉--global，*但是邮箱不会变？存疑，影响不大*

#### 二，初始化 以及常用基本命令

```shell
git init		//创建.git，即仓库，将该目录下的文件提交后可以进行版本控制，git会追踪更新

touch file_name //创建新文件或更新时间戳
echo ["anything"] >file_name.md 	    //tip:重定向输出文件，详见下文

cd /dir			//转移到路径，非常用
git status		//显示当前状态与help
git log 		//工作日志，显示commit记录
git ls-files 	//输出当前分支（可选指定目录）下被追踪的文件列表
echo "anything"	//输出文本到终端
CTRL+C 或CTRL+D //输错后卡死的解决办法
git clear 		//清屏
q  				//在END页面退出
```

1. echo和touch的区别：

   ~echo 主要用于输出文本或`重定向文本`到文件，而 touch 主要用于`创建空文件`或`更新文件的时间戳`。
   ~echo 可以与重定向操作符（如 `>` 和 `>>`）一起使用，将`覆盖文件`或`输出追加`到文件内容，而 touch 主要是与文件的存在性和时间戳相关。
   ~在创建文件方面，echo 可以在创建文件的同时指定文件的内容，而 touch 只能创建一个空文件。

2. CTRL+C/D的区别，+C只退出当前命令，+D退出整个终端

#### 三，工作区  暂存区 本地仓库 远程仓库之间的流转

> ![image-20240502191340516](C:\Users\86198\AppData\Roaming\Typora\typora-user-images\image-20240502191340516.png)*图片来源于B站技术蛋老师*

```shell
git add <name .>        //送入暂存区
git commit <name .>		//送入本地仓库,此时会进入vim编辑器，i进入编辑，Esc退出编辑，：wq退出写页
    commit -m "anything"//同上，但是直接提交了信息不进入vim
    commit -a -m"anything"//一步到位，不需要额外写add，可写-am"anything"
```

#### 四，选择性忽略功能：`.gitignore`

1. 目的：有选择性的上传目标文件

2. 创建`.gitignore`文件，将需要忽略的文件或目录名称输入`.gitignore`

3. 示例：

   ```shell
   echo "ignore_name">>.gitignore
   ```

4. 详细使用方法：见大佬研究：[Git 开发必备 .gitignore 详解！【建议收藏】-CSDN博客](https://blog.csdn.net/nyist_zxp/article/details/119887324)

   - `folderName`文件和目录都忽略
   - `folderName `加上反向操作后忽略 *`folderName`* 文件，而不忽略 *`folderName`* 目录

问题：

- 全局性：`.gitignore`文件是仓库级别的，而不是分支级别的。这意味着无论你在哪个分支上工作，`.gitignore`文件的内容都会生效

- 对已追踪文件无效：如果被忽略的文件已经纳入到了版本控制管理中（即已受控状态，文件被add后就表示已经受控）时，即使在`.gitignore`文件中设置了忽略也不会起作用

- 文件追踪规则：`.gitignore`文件中定义的规则会应用于所有分支，因为Git在判断哪些文件需要追踪时，会查看`.gitignore`文件来决定哪些文件或目录应该被忽略

- 存在缓存：如果之前提交过git文件，那么对应的[Track]中包含之前文件的缓存，`.gitignore`是不对缓冲中的文件进行过滤的。因此，先将之前提交跟踪缓存进行删除，然后`.gitignore`文件才能达到想要的作用

  ```shell
  git rm -r -f --cached .
  ```

- 对于分支的影响详解，移步另文。

#### 五，分支创建与合并

目的：各分支相互独立，协同开发

创建一个新的分支时，你实际上是在创建一个指向当前提交的新引用（或者说是一个`新的指针`）

```shell
git branch		 #当前Git仓库中所有的分支列表，*代表当前所处分支
git branch branch_name		#以当前为原本创建新分支
git checkout name
	switch name				#切换到name分支
git checkout -b name
	switch -c name 			#创建并切换
git branch -d name 			#删除分支，需要先离开该分支
	branch -D name			#无视风险强行删除
git merge name				#合并name到当前分支
```

合并冲突手动处理，其他方法略

```shell
#打开包含冲突的文件，手动解决冲突之后，再执行如下的命令
 git add .
 git commit -m“解决了分支合并冲突的问题"
 git merge 分支名称
```

1. 对于ignore：新分支的创建并不会改变你当前工作区（working directory）或暂存区（staging area）中的文件状态，但是如果ignore的文件已经被commit到原来的分支，新建的分支在工作区虽然可以”看到“该文件，但是无法识别status。

2. 切换分支时未提交的修改可能导致冲突

   ```shell
   M   被修改文件名称 #该提示可能会对其他分支造成影响，例如.gitignore
   ```

   

ignore实验结果：

 ~仅add而不commit，新建分支不会ignore

~在新分支取消ignore可以再次识别到隐藏内容（未删除工作区），如果不保存修改，会出现如下情况，将于`.gitignore的学习模块`进行讨论

<img src="C:/Users/86198/AppData/Roaming/Typora/typora-user-images/image-20240502225656799.png" alt="image-20240502225656799" style="zoom: 33%;" />
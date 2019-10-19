## Git提交代码到远程库

#### 关于Git

> Git是分布式版本控制软件，可以指定和若干不同的远端代码仓库进行交互

- 安装GIt： 可以在 Git 官方网站下载 [http://git-scm.com/download/](http://git-scm.com/download/win)
- 查看安装版本：` $ git --version` （在Git安装目录下的git-bash.exe进入）

- 使用前配置：

```
$ git config --global user.name "Your Name"
$ git config --global user.email email@example.com
```

- 查看配置：` $ git config --list  `



#### Repository（仓库）

> 创建仓库常用的两个第三方托管平台

- 码云（ http://git.oschina.net）

  - 在登录之后，点击右上角的 “+” 号，选择新建仓库，填写仓库的相关信息之后，就可以得到一个远程仓库的地址（ http://git.oschina.net/ [用户名]/[仓库地址]）

  

- github（http://github.com/）

  - 同样是在登录之后，点击右上角“+”号，选择“New respository”，然后填写相关信息，得到一个远程仓库地址（http://github.com/[用户名]/[仓库地址]）

    

> 两种取得Git项目仓库的方法

- 第一种是从一个服务器克隆一个现有的 Git 仓库

  - 克隆仓库： ` $ git clone [url]`
    - 这会在当前目录下创建一个名为 “git-start.git” 的目录，并在这个目录下初始化一个 .git 文件夹，从远程仓库拉取下所有数据放入 .git 文件夹（建议进入具体目录再clone如： ` cd e:`  ` cd clone/test `）
  - 如果想在克隆远程仓库的时候，自定义本地仓库的名字

  ```
  $ git clone [url] user-defined-name
  ```

  

- 第二种在现有目录中初始化仓库
  - 进入项目文件夹，右击选择" Git Bash Here" 或者直接在上述打开的 git-bash.exe 中 cd 到项目目录
  - 初始化仓库：` $ git init`    （移除初始化的本地仓库： ` find . -name ".git" | xargs rm -Rf`)
  - 通过`  git add` 命令来实现对指定文件的跟踪，然后执行 ` git commit` 提交

```
$ git add [当前目录下的文件名]    //  $ git add . 对所有文件进行跟踪
$ git commit -m '提交说明，一般是修改内容'   //不添加注释进入vim可以shift + : 输入wq保存
```



#### 更新提交到仓库

- 跟踪新文件（add可以用于开始跟踪新文件，或者把已跟踪的文件放到暂存区）

```
$ git add [fileName]     //在add之前需要进入初始化的目录中，
$ git status      //查看文件跟踪状态（是否暂存）
```



- 忽略文件（无需纳入 Git 的管理，也不希望它们总出现在未跟踪文件列表，比如编译过程出现的文件）

```
$ cat .gitignore
*~         //忽略所有以波浪符(~)结尾的文件
```



- 提交更新（git status确认已经暂存了所有修改的文件）

```
$ git status
$ git add .
$ git commit -m '提交说明'
$ git commit    //跳过git add自动暂存所有已跟踪的文件
```



#### 更新到远程仓库

- 查看远程仓库

  - 使用的 Git 保存的简写与其对应的 URL  ` $ git remote -v`   （克隆了仓库，那么至少应该能看到 origin - 这是 Git 给你克隆的仓库服务器的默认名字）

    

- 添加远程仓库
  
  - ` git remote add <shortname> <url>`  为添加的远程库url添加可引用的简写shortname



- 取回远程主机某个分支的更新，再与本地的指定分支合并

  ```
  $ git pull <远程主机名> <远程分支名>:<本地分支名>     //当与当前分支合并时可以省略冒号后的内容
  $ git pull origin  master
  ```

  注：当你希望将本地创建的Git仓库（没有clone)合并到远程仓库时，需要合并两个独立启动的仓库

  `$git pull origin master --allow-unrelated-histories`



- 推送到远程仓库（推送完可登陆到远程库查看）

  ```
  $ git push [remote-name] [branch-name]`     //推送到远程库remote-name的branch-name分支
  $ git push origin master`   //克隆时通常会自动帮你设置好这两个名字，可以直接使用
  ```

  

  

- 移除与重命名远程库

  ```
  $ git remote rename [oldname] [newname]    //重命名
  $ git remote rm [remote-name]`   //移除
  ```



#### 分支管理

- 创建分支

  ```
  $ git branch <branch name>
  ```

- 查看分支

  ```
  $ git branch 
  ```

- 切换分支

  ```
  $ git checkout [branch name]
  $ git checkout -b [newbranch name]  //-b 创建新分支并切换到该分支下
  ```

- 删除分支（在删除现有分支之前，请切换到其他分支）

  ```
  $ git branch -D [branch_name]
  ```

- 重命名分支

  ```
  $ git branch -m [new_branch] [old_branch]
  ```

- 合并两个分支

  ```
  $ git merge [branch_name]    //将新分支合并到当前分支  
  $ git diff   //可以查看合并冲突
  ```

  

文章参考自：https://www.yiibai.com/git/git-quick-start.html












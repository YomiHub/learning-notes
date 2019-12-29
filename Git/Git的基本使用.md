#### 安装Git
- 版本控制工具 Git：可以在 Git 官方网站下载，网址为 https://git-scm.com/downloads （下载后直接next默认设置安装即可）
- 安装 GitHub for Windows。 该安装程序包含图形化和命令行版本的 Git，下载网址：http://windows.github.com
- 注：安装成功后，在 任意目录下右击会在菜单中显示“Git GUI Here”（图形化界面）、“Git Bash Here”（命令窗口）选项。也可以使用本地cmd窗口敲命令（按住shift+鼠标右击=》w=>enter）

</br>

#### 设置用户信息
- 每一个 Git 的提交都会使用这些信息，并且它会写入到每一次提交中，不可更改
- 使用 `--global` 参数，命令只需要执行一次，Git在该系统就会一直保存设置。当在特定目录需要使用到不同的配置信息时，就在项目下运行不带参数`--global`的命令
```
$ git config --global user.name "maxsu"
$ git config --global user.email maxsu@yiibai.com
```
- 使用 `git config --list` 命令来列出所有 Git 当时能找到的配置

</br>

#### 创建一个空的Git仓库或重新初始化一个现有仓库
- 创建目录codebase，在codebase下右击选择Git Bash Here，通过命令`git init`初始化一个Git仓库
- 初始化完成后会在该项目目录下生成一个隐藏文件.git

</br>

#### 执行更新
- 项目文件夹内的内容发生改变之后，将更改添加到Git库中
  + `git status`查看当前工作区的状态
  + `git add ./filename.md`将更改或者新增的文件filename添加到暂存区域
  + `git commit -m "更新说明"`提交更改，省略-m则会打开vim编辑器按i可输入多行备注信息，按esc之后:wq回车退出(:q!强制退出)
  + add和commit合并命令：`git commit --all -m "更新说明"`表示提交所有修改

</br>

#### 查看提交记录
- `git log`显示提交日志信息
- `git log --oneline` 精简显示日志信息
- `git log --no-merges`.显示整个提交历史记录，但跳过合并
- `git log master include/scsi drivers/scsi`显示自v2.6.12版以来所有提交更改include/scsi或drivers/scsi子目录中的任何文件的所有提交
- `git log --since="2 weeks ago" -- gitk`显示最近两周的更改文件
- 注：参数可以根据需要进行调整，具体参考[易百](https://www.yiibai.com/git/git_log.html)

</br>

#### 版本回退
> git reset命令用于将当前HEAD复位到指定状态。一般用于撤消之前的一些操作(如：git add,git commit等)，可参考[易百](https://www.yiibai.com/git/git_reset.html)

- `git status`查看add的记录、`git log --online`查看提交记录，确定需要回退到的版本
- `git reset --hard Head~0` 回退到上一个版本，参数为1表示回退到上上个版本以此类推（也可以使用git log后显示的版本号代替索引值，可以通过`git reflog`查看版本切换记录，找到所有版本号）
- `git reset --soft HEAD^`回滚最近一次commit
- 回滚master分支上最近几次提交，并把这几次提交放到指定分支`topic/wip`中
```
$ git branch topic/wip      
$ git reset --hard HEAD~3   
$ git checkout topic/wip   
```
- `git reset --hard`或者`git reset --hard HEAD`回滚merge和pull操作,清除索引和工作区中被搞乱的东西，比如pull冲突但是尚未提交
- `git reset --hard ORIG_HEAD`回滚pull/merge操作，回到git pull之前的状态，且清空工作区即丢弃本地未使用git add的那些改变
- 在污染的工作区中回滚合并或者拉取`git reset --merge ORIG_HEAD`可以避免在回滚时清除工作区
- 中断的工作流程处理，例如正工作在分支：feature 中，但工作区内容尚未成型，此时需要切换到另外的分支修改bug
```
$ git checkout feature ;# you were working in "feature" branch and 
$ work work work       ;# got interrupted 
$ git commit -a -m "snapshot WIP"                 
$ git checkout master 
$ fix fix fix 
$ git commit ;# commit with real log 
$ git checkout feature 
$ git reset --soft HEAD^ ;# go back to WIP state  
$ git reset  
```
- 重置单独的一个文件`git reset -- filename.md`  例如：已经add了一个文件进入索引，但是不想commit
- 保留工作区并丢弃一些之前的提交`git reset --keep start`把在`git tag start`之后的提交清除，但保持工作区不变

</br>

#### 切换分支
> 一般用于提交尚未完成的功能，即不希望被其他开发者拉下来使用的代码
- `git branch dev`创建名为dev的分支，可以通过`git beanch`查看所有分支
- `git checkout dev`将当前分支切换到dev
- `git merge dev`在master分支下用此命令将dev分支合并到master，当在两个分支都修改代码的时候，可能会出现冲突，这时候需要手动删除代码处理冲突再提交结果
- `git branch -d dev`删除dev分支

#### 将本地仓库代码提交到github
> 首先需要在github创建仓库，获取远程仓库地址，详细操作见另一篇[笔记](https://github.com/YomiHub/learning-notes/blob/master/%E5%B7%A5%E5%85%B7%E7%9A%84%E4%BD%BF%E7%94%A8/Git%E6%8F%90%E4%BA%A4%E4%BB%A3%E7%A0%81%E5%88%B0%E8%BF%9C%E7%A8%8B%E5%BA%93.md)，在执行commit之后再push
- `git push 远程仓库https地址 远程分支名称`
- 当多人合作，同时修改一份代码时，可能会出现代码conflict，所以在push代码前需要先pull拉取一下远程代码，有冲突则需要手动删除或修改代码

#### 从远程库拉取代码
- 方法一：在本地创建文件夹，init初始化本地仓库后,`git pull 远程仓库地址 远程分支名称`拉取代码
- 方法二：在本地创建文件夹，直接拷贝远程项目`git clone 远程仓库地址`，多次执行会覆盖本地内容


#### 上传代码到远程仓库的方式
- ssh：通过公钥和私钥进行验证（github上放的是公钥）
  + 在任何目录下进入git命令窗口，执行`ssh-keygen -t rsa -C "youremail@mail.com"`，指定加密方式为rsa
  + 直接点回车，说明会在默认文件id_rsa上生成ssh key，提示设置密码可以不设直接回车，重复密码也直接回车
  + 打开默认存储的位置（Mac可以命令行打开vim ~/.ssh/id_rsa.pub ），其中.pub后缀的文件是公钥，复制内容，在github仓库进入Personal settings，选择SSH and GPG keys，点击 New SSH key 填入title(mywindow)和公钥key
  + `git push 远程库ssh地址 远程库分支名称`
- https

#### 管理跟踪的远程库
> 为了避免每次pull和push都需要复制粘贴远程库的地址，可以用remote配置跟踪的远程库简称
- `git remote add 简称 远程分支`，设置后就可以用简称代替链接
- `git remote -v`查看跟踪的远程库
- `git push 简称 -u 远程分支`将当前分支与远程库、远程分支关联起来，可以在下次提交的时候就可以使用简写`git push`和`git pull`

参考自[易百教程](https://www.yiibai.com/git)
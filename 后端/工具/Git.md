#### git操作流程
* 安装git

* 配置git 

```
    git config --global user.name 你的英文名
    git config --global user.email 你的邮箱
```
```
    git config --global push.default simple 
```
matching（匹配所有分支）
matching 参数是 Git 1.x 的默认参数，也就是老的执行方式。其意是如果你执行 git push 但没有指定分支，它将 push 所有你本地的分支到远程仓库中对应匹配的分支。

simple（匹配单个分支）
simple参数是 Git 2.x 默认参数，意思是执行 git push 没有指定分支时，只有当前分支会被 push 到远程仓库。
```
    git config --global core.quotepath false // 关闭路径中文转义
    git config --global core.editor "code --wait" // 使用vscode作为默认编辑器
    git config --global core.autocrlf input // 提交时自动将行结束符CRLF换成LF
```

配置完成后使用
```
git config --list --show-origin
```
查看全部配置

* 配置登陆信息
可以使用两种方式：

1.账号和Tokens（github已经禁用密码方式）方式
token来源为：github仓库settings->developer settings->tokens(classic)->generate a personal access token 

2. ssh key

  ssh生成密钥方式 -t表示生成密钥算法 -b设置大小 -C添加评论 
  ```
  ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
  ```
  提示生成描述随便输入，提示输入密码可直接回车
  之后进入 ~/.ssh目录可以看到其中有一个id_rsa(私钥),is_rsa.pub(公钥)
  ```
  cat id_rsa
  ```
  获取私钥内容，添加到github的SSH and GPG keys中，之后使用
  ```
  ssh -T git@github.com
  ```
  测试是否成功。


##### 常用命令

* git status 
执行该命令会返回三部分信息：

拟提交的变更：这是已经放入暂存区，准备 commit 提交的变更
未暂存的变更：这是工作目录和暂存区快照之间存在差异的文件列表
未跟踪的文件：这类文件是被版本管理忽略的文件
git status 不显示已经 commit 到项目历史中去的信息。看项目历史的信息要使用 git log。

查看忽略文件
可以查看添加到 .gitignore 的文件的状态信息。

git status --ignored

* git remote 查看远程分支
git remote -v 列出远程分支的详细信息
git remote show <remote> 查看指定远程仓库信息
git remote add orgin <shortname> <url> 添加新的远程仓库
git pull <remote> <branch> 获取远程仓库变化

* git push 将本地的 master 分支推送到 origin 主机的 master 分支。如果 master 不存在，则会被新建。

git push origin master
可以将 master 换成你想要推送的任何分支,origin是远程仓库分支，master是本地分支。另外：
使用`git push -u origin master`设置默认推送方式后，后面用git push则默认使用这种方式。正常情况下，push代码都需要鉴权，鉴权有两种方式：

* git reset三种常用模式(soft、mixed、hard)的用法
首先我们知道git中存在三棵文件树：

工作目录->git add -> 暂存区(Index)，可使用`git ls-files -s`查看 -> git commit -> Head 可使用`git ls-tree -r HEAD`命令查看

git reset用于文件维度的场景可用于将文件移出暂存区(撤回git add命令)
用于提交维度则用于重制提交，可选方式包括以下几种：
```
git reset <file-name>
git reset <commit-id>
git reset <tag-name>
git reset HEAD~<n>
```
git reset分五种模式，soft、mixed、hard、merge、keep，常用的为前三种：
在执行一次commit之后，文件工作目录、暂存区、HEAD中均从V1版本更新到V2版本，对于soft模式:
`git reset --soft HEAD~1` 将已经commit的文件，重置到add之后的状态，此时HEAD为V1版本，其余为V2版本
`git reset --mixed HEAD~1`将文件重置到未add的版本，本地工作目录保留了改动，暂存区及Head均为V1版本，reset的默认模式
`git reset --hard HEAD~1`将本地工作目录的修改也重置到V1版本，类似于在ctrl+z，谨慎使用

* git branch 带*的表示当前分支
也可以git branch feature1 新建分支

* git checkout <branchName>切换分支
或者使用git checkout -b <branchName> 新建分支并切换到新分支上

* git log 查看提交记录
--all 查看全部分支提交记录
--all --graph 查看图形化的提交记录
##### 常见问题
* git初始化后没有master分支

git 第一次初始化时一定要先使用git add .和git commit -m " 提交信息"这两个命令后才会出现master分支，并且可以创建其它分支。

* .gitignore文件常见设置

```
### IntelliJ IDEA ###
.idea
*.iws
*.iml
*.ipr
*.log
modules.xml

target/

**/.idea
**/*.iws
**/*.iml
**/*.ipr
**/modules.xml


**/mvnw
**/mvnw.cmd
**/.mvn
**/target/
**/.gitignore

### Maven ###
pom.xml.tag
pom.xml.releaseBackup
pom.xml.versionsBackup
pom.xml.next
release.properties
dependency-reduced-pom.xml
buildNumber.properties
.mvn/timing.properties
.mvn/wrapper/maven-wrapper.jar

### Java ###
# Compiled class file
*.class
```
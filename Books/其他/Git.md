# Git

- 安装目录无中文或空格
- Linux命令和Git命令通用

# 命令

| 命令                                  | 说明                      |
| :----------------------------------- | :------------------------ |
| git config --global user.name 用户名  | 设置用户名 (设置一次即可)   |
| git config --global user.email 邮箱名 | 设置用户签名 (设置一次即可) |
| git init                             | 初始化本地库               |
| git status                           | 查看本地库状态             |
| git add 文件名                        | 添加到暂存区               |
| git commit -m "日志信息" 文件名        | 提交到本地库               |
| git reflog                           | 查看历史记录               |
| gir reset --hard 版本号               | 版本穿梭                   |

## 设置用户签名

```
git config --global user.name 用户名
git config --global user.email 邮箱(不会检验邮箱是否正确)
```

- 首次安装Git时必须设置用户签名，否则无法提交代码。

```
zjk10@DESKTOP-HSVIOTH MINGW64 ~/Desktop
$ git config --global user.name scott

zjk10@DESKTOP-HSVIOTH MINGW64 ~/Desktop
$ git config --global user.email zjk1054860443@163.com
```

## 初始化本地库

```git
git init
```

- 可以到目标文件目录位置再右键打开Git Bash，自动切换到该路径

```git
zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo
$ git init
Initialized empty Git repository in G:/GitDemo/.git/
```

## 查看本地库状态

```git
zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git status
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

## 添加到暂存区

```git
git add 文件
```

```git
zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git status
On branch master

No commits yet

Untracked files:  ##未添加到暂存区的文件
  (use "git add <file>..." to include in what will be committed)
        hello.txt

nothing added to commit but untracked files present (use "git add" to track)

zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git add hello.txt
warning: in the working copy of 'hello.txt', LF will be replaced by CRLF the next time Git touches it

zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   hello.txt
```

## 从暂存区移除

```git
git -rm --cached 文件
```

```git
zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git rm --cached hello.txt
rm 'hello.txt'
```

## 提交到本地库

```git
git commit 文件

git commit -m "日志信息" 文件
```

```git
zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git commit -m "first commit 日志信息" hello.txt
warning: in the working copy of 'hello.txt', LF will be replaced by CRLF the next time Git touches it
[master (root-commit) 5920c4a] first commit 日志信息
 1 file changed, 1 insertion(+)
 create mode 100644 hello.txt
 
zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git status
On branch master
nothing to commit, working tree clean
```

## 修改文件

- 在修改文件之后，需要重新添加到暂存区，并再次提交。
- 在完成提交之后，会有新的历史版本，同时指针指向该版本

```git
zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ vim hello.txt

zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   hello.txt

no changes added to commit (use "git add" and/or "git commit -a")

zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   hello.txt

no changes added to commit (use "git add" and/or "git commit -a")

zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git add hello.txt
warning: in the working copy of 'hello.txt', LF will be replaced by CRLF the next time Git touches it

zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   hello.txt


zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git commit -m "second Commit 日志信息" hello.txt
warning: in the working copy of 'hello.txt', LF will be replaced by CRLF the next time Git touches it
[master fb9be28] second Commit 日志信息
 1 file changed, 2 insertions(+)
 
zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git reflog
fb9be28 (HEAD -> master) HEAD@{0}: commit: second Commit 日志信息
5920c4a HEAD@{1}: commit (initial): first commit 日志信息
```

## 历史版本

| 命令                    | 说明            |
| :---------------------- | :-------------- |
| git reflog              | 查看版本信息     |
| git log                 | 查看版本详细信息 |
| git reset --hard 版本号 | 版本穿梭         |

### 查看版本信息

```git
git reflog

git log
```

```git
zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git reflog
fb9be28 (HEAD -> master) HEAD@{0}: commit: second Commit 日志信息
5920c4a HEAD@{1}: commit (initial): first commit 日志信息


zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git log
commit fb9be28b255e26c8d2327c06293e35ed8681f3f3 (HEAD -> master)
Author: scott <zjk1054860443@163.com>
Date:   Wed Nov 9 20:33:04 2022 +0800

    second Commit 日志信息

commit 5920c4ad653816559f14c65e11d613cb899b1010
Author: scott <zjk1054860443@163.com>
Date:   Wed Nov 9 20:28:47 2022 +0800

    first commit 日志信息
```

### 版本穿梭

```git
git reset --hard 版本号
```

- 版本穿梭之后，查看的文件就是对应版本号的文件。
- 底层：HEAD指针的移动

![](C:/NoteBook/pictures/2860321240278.png =456x)

```git
zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git reset --hard fb9be28
HEAD is now at fb9be28 second Commit 日志信息

$ git reflog
fb9be28 (HEAD -> master) HEAD@{0}: reset: moving to fb9be28
fb9be28 (HEAD -> master) HEAD@{1}: commit: second Commit 日志信息
5920c4a HEAD@{2}: commit (initial): first commit 日志信息
```

**查看指针文件**

- 打开隐藏文件的查看
- 路径G:\GitDemo\.git
  - 查看内容 ref: refs/heads/master 
- 根据查看的内容，选择路径G:\GitDemo\.git\refs\heads\master
  - 查看内容（即为记录的版本号）  5920c4ad653816559f14c65e11d613cb899b1010 

# 分支

- 同时并行推进多个分支，提高开发效率
- 各个分支在开发过程中，如果一个分支开发失败，不会对其他分支有影响。

| 命令    | 说明    |
| :-- | :-- |
|git branch 分支名     | 创建分支    |
|git branch -v     | 查看分支     |
|git checkout 分支名     | 切换分支    |
|  git merge 分支名   |把指定的分支合并到当前分支上     |

## 查看分支

```git
git branch -v
```

```git
zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git branch -v
* master 5920c4a first commit 日志信息
```

## 创建分支

```git 
git branch 分支名
```

```git
zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git branch hot-fix

zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git branch -v
  hot-fix 5920c4a first commit 日志信息
* master  5920c4a first commit 日志信息
```

## 切换分支

```git
git checkout 分支名
```

```git
zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git checkout hot-fix
Switched to branch 'hot-fix'

zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (hot-fix)
$ git branch -v
* hot-fix 5920c4a first commit 日志信息
  master  5920c4a first commit 日志信息
```

- 在分支中的修改不影响其他的分支

```git
zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (hot-fix)
$ vim hello.txt

zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (hot-fix)
$ git add hello.txt

zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (hot-fix)
$ git commit -m "日志信息：hot-fix 提交" hello.txt
[hot-fix 8f45a48] 日志信息：hot-fix 提交
 1 file changed, 2 insertions(+)
```

## 合并分支

```git
git merge 分支名
```

- 切换到（要合并到的）分支上进行合并分支。

**底层 指针**

![](C:/NoteBook/pictures/144484721236833.png =470x)

### 正常合并

```git
zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (hot-fix)
$ git checkout hot-fix
Already on 'hot-fix'

zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (hot-fix)
$ git checkout master
Switched to branch 'master'

zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git merge hot-fix
Updating 5920c4a..8f45a48
Fast-forward
 hello.txt | 2 ++
 1 file changed, 2 insertions(+)
 
zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git reflog
8f45a48 (HEAD -> master, hot-fix) HEAD@{0}: merge hot-fix: Fast-forward
5920c4a HEAD@{1}: checkout: moving from hot-fix to master
8f45a48 (HEAD -> master, hot-fix) HEAD@{2}: checkout: moving from hot-fix to hot-fix
8f45a48 (HEAD -> master, hot-fix) HEAD@{3}: commit: 日志信息：hot-fix 提交
5920c4a HEAD@{4}: checkout: moving from master to hot-fix
5920c4a HEAD@{5}: reset: moving to 5920c4a
fb9be28 HEAD@{6}: reset: moving to fb9be28
fb9be28 HEAD@{7}: commit: second Commit 日志信息
5920c4a HEAD@{8}: commit (initial): first commit 日志信息
```

### 冲突合并

- 合并分支时，两个分支在同一个文件的同一个位置有两套完全不同的修改。Git无法替我们决定使用哪个。
- 在完成修改后，此时提交到本地库时，不能带有文件名。
  - 否则：`fatal: cannot do a partial commit during a merge.`
- 只会修改合并到的分支的文件，要被合并的分支不受影响

```git
zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ vim hello.txt

zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ cat hello.txt
hello Git !
test master
hot-fix

zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git add hello.txt

zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git commit -m "日志信息：master 4" hello.txt
[master 42191c7] 日志信息：master 4
 1 file changed, 1 insertion(+), 1 deletion(-)

zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git checkout hot-fix
Switched to branch 'hot-fix'
M       hello.txt

zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (hot-fix)
$ vim hello.txt

zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (hot-fix)
$ cat hello.txt
hello Git !
test hot-fix
hot-fix

zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (hot-fix)
$ git add hello.txt

zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (hot-fix)
$ git commit -m "日志信息：hot-fix 2" hello.txt
[hot-fix 53ddaa5] 日志信息：hot-fix 2
 1 file changed, 1 insertion(+), 1 deletion(-)


# 此时在hello.txt文件中，的test位置有两个分支完全不同的修改。

# 报冲突
zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git merge hot-fix
Auto-merging hello.txt
CONFLICT (content): Merge conflict in hello.txt
Automatic merge failed; fix conflicts and then commit the result.

# 正在合并中的状态
zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master|MERGING)
$ vim hello.txt

# 查看冲突文件

hello Git !
<<<<<<< HEAD  # 表示当前分支
test master
=======
test hot-fix 
>>>>>>> hot-fix  # 表示要合并的分支
hot-fix

# 手动修改 修改该文件，选择要保留的内容

hello Git !
test hot-fix
hot-fix

# 再次添加和提交

zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master|MERGING)
$ git add hello.txt

# 此时提交到本地库时，不能带有文件名

zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master|MERGING)
$ git commit -m "日志信息 matser 5 + hot-fix"
[master fed0b15] 日志信息 matser 5 + hot-fix

# 只会修改合并到的分支的文件，要被合并的分支不受影响
```

# Git团队协作机制

# GitHub操作

**凭据管理器**

![](C:/NoteBook/pictures/555244722248071.png =499x)

**如何解决Github打不开的问题？**

- 打开C:\Windows\System32\drivers\etc\hosts
- 添加(以管理员权限打开记事本，Ctrl + O 输入文件地址进行修改)

```
# GitHub Start 

140.82.113.3       github.com
140.82.114.20      gist.github.com
199.232.69.194     github.global.ssl.fastly.net
151.101.184.133    assets-cdn.github.com
151.101.184.133    raw.githubusercontent.com
199.232.28.133     raw.githubusercontent.com 
151.101.184.133    gist.githubusercontent.com
151.101.184.133    cloud.githubusercontent.com
151.101.184.133    camo.githubusercontent.com
199.232.96.133     avatars.githubusercontent.com

# GitHub End
```

- 修改完映射文件，打开cmd输入`ipconfig/flushdns`刷新dns缓存

**翻墙不可取。。。**

**创建GitHub库**

![](C:/NoteBook/pictures/220061222232587.png =425x)

**使用Https连接**

![](C:/NoteBook/pictures/79841422250467.png =732x)

- https://github.com/creatern/Git-Demo.git

## 创建远程仓库

| 命令                              | 说明                                                 |
| :------------------------------- | :-------------------------------------------------- |
| git remote -v                    | 查看当前所有的远程地址别名                             |
| git remote add 别名 远程地址      | 起别名                                               |
| git push 别名 分支                | 推送本地分支上的内容到远程仓库                         |
| git clone 远程地址                | 将远程仓库的内容克隆到本地                             |
| git pull 远程库地址别名 远程分支名 | 将远程仓库对于分支最新内容拉下来后与当前本地分支直接合并 |

## 远程仓库操作

### 查看远程仓库别名

```git
git remote -v
```

```git
zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git remote
# 没有则都不显示
```

### 创建远程仓库别名

```git
git remote add 别名 远程地址
```

```git
zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git remote add Git-Demo https://github.com/creatern/Git-Demo.git

zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git remote -v
Git-Demo        https://github.com/creatern/Git-Demo.git (fetch)
Git-Demo        https://github.com/creatern/Git-Demo.git (push)
```

### 将本地库的推送到远程库

```git
git push 别名 分支 
```

- 没有别名也可以直接地址

```git
zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git push Git-Demo master
Enumerating objects: 13, done.
Counting objects: 100% (13/13), done.
Delta compression using up to 16 threads
Compressing objects: 100% (5/5), done.
Writing objects: 100% (13/13), 1.14 KiB | 1.14 MiB/s, done.
Total 13 (delta 1), reused 0 (delta 0), pack-reused 0
error: RPC failed; curl 56 OpenSSL SSL_read: Connection was reset, errno 10054
send-pack: unexpected disconnect while reading sideband packet
fatal: the remote end hung up unexpectedly
Everything up-to-date
```

### 拉取远程库到本地库

```git
git pull 别名 分支
```

- 自动提交到本地库
- 可能网络问题：`fatal: unable to access 'https://github.com/creatern/Git-Demo.git/': Failed to connect to github.com port 443 after 21059 ms: Timed out`

```git
zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git pull Git-Demo master
From https://github.com/creatern/Git-Demo
 * branch            master     -> FETCH_HEAD
Already up to date.
```

### 克隆远程仓库到本地

```git
git clone 远程地址
```

- 需要先到想要的文件目录打开Git
- 公共的库，不用登录也可以克隆

```git
zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git clone https://github.com/creatern/Git-Demo.git
Cloning into 'Git-Demo'...
remote: Enumerating objects: 19, done.
remote: Counting objects: 100% (19/19), done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 19 (delta 1), reused 13 (delta 1), pack-reused 0
Receiving objects: 100% (19/19), done.
Resolving deltas: 100% (1/1), done.
```

- 此时，在`G:\GitDemo\`处克隆了库`Git-Demo`

### 提交到他人的仓库

- 需要先加入他人的远程仓库的成员
- 获取邀请码，然后在自己的界面复制该邀请码即可

## 跨团队合作

**搜索他人的仓库**

- 需要是`public`的才可以搜索到
- 可以`@拥有者/仓库名`精准搜索

**fork添加到自己的仓库**

![](C:/NoteBook/pictures/61920823245573.png =640x)

**Pull requests**

- 给别人
![](C:/NoteBook/pictures/175711123226814.png =432x)
![](C:/NoteBook/pictures/395401123249254.png =438x)
![](C:/NoteBook/pictures/157941223244390.png =439x)
- 查看收到的
![](C:/NoteBook/pictures/212891323237936.png =434x)
- 合并收到的
![](C:/NoteBook/pictures/81821523221600.png =458x)
![](C:/NoteBook/pictures/342191523224104.png =449x)


## SSH免密登录

**生成.ssh文件**

```git
ssh-keygen -t rsa -C 邮箱
```

- 三次回车
- 到用户文件目录 C:\Users\zjk10

```git
zjk10@DESKTOP-HSVIOTH MINGW64 ~
$ ssh-keygen -t rsa -C zjk1054860443@163.com
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/zjk10/.ssh/id_rsa):
Created directory '/c/Users/zjk10/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/zjk10/.ssh/id_rsa
Your public key has been saved in /c/Users/zjk10/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:Jo45BrJ4Vp9MtMque+T74/vDotYPCrI8wyBMtl6VE8U zjk1054860443@163.com
The key's randomart image is:
+---[RSA 3072]----+
|      o.         |
|     . E         |
|      +          |
| o   = .         |
|= o o = S        |
|+= =.O +         |
|O =oB.*.         |
|.X ++o+.o        |
|..====+*o.       |
+----[SHA256]-----+
```

- 在对应的目录生成了.ssh文件夹
![](C:/NoteBook/pictures/576632823233051.png =445x)
   - id_rsa 私钥
   - id_rsa.pub 公钥

**获取公钥**

```git
zjk10@DESKTOP-HSVIOTH MINGW64 ~
$ cd .ssh

zjk10@DESKTOP-HSVIOTH MINGW64 ~/.ssh
$ ll
total 5
-rw-r--r-- 1 zjk10 197609 2610 Nov  9 23:27 id_rsa
-rw-r--r-- 1 zjk10 197609  575 Nov  9 23:27 id_rsa.pub

zjk10@DESKTOP-HSVIOTH MINGW64 ~/.ssh
$ cat id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDCo4Y42+qZM6Z0Y3dXnkOnWrP1zhJHfz3v6bRRkHt8o+OtLN1nKK51Lcf05E/ObMsaReKSMwwIomxHYKcpU82A8G5/TekMaYcESSrIgM2jm0k/CTAbSgMC1C0VtSC7ORWUAW6HdNcolusF7lD80Bs61P7A8gr2jYryHBbQfM2vp7n/9BsulTshxPT2azVAhHiz/iaC915eQuLEwvnQMsdGKsplECz2V1WcH8hzy8txiHQJDLYkth5wnBfzBNC8+NjDfAeIAvrk16p9s364grQtt1Hj1lCTeWuqhvQ/JnoyM5G6jQHMxzUIpUpejZF6CJdPpDAYsRm/wTLKeerBpapUh7/TS1nnxbIxiXg6b32UTzdwllggWox7Qj2HbBpdg6MMdCPUlyZWUVpgzjUHf6LSvr0g2h3wSKLWLZWMKbHih2REjKawAGV+6y5JzzfVFSYYN0PnQQwtLHbiJoygRxZ6290tRfpW1vcLvC7/wXFzSASB1WZbvRRBm4HgIAKa8Pc= zjk1054860443@163.com
```

**到GitHub中设置**

![](C:/NoteBook/pictures/71373323225936.png =642x)
![](C:/NoteBook/pictures/498163323226545.png =476x)
![](C:/NoteBook/pictures/283203423253500.png =443x)

### 以ssh协议pull

![](C:/NoteBook/pictures/121664023225011.png =823x)

- git@github.com:creatern/Git-Demo.git

```git
git pull ssh协议链接 分支
```

```git
zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git pull git@github.com:creatern/Git-Demo.git master
```


### 以ssh协议push

```git
git push ssh协议链接 分支
```

```git
zjk10@DESKTOP-HSVIOTH MINGW64 /g/GitDemo (master)
$ git push git@github.com:creatern/Git-Demo.git master
```

# IDEA 集成 Git

## 配置Git忽略文件

**创建忽略规则文件 Xxx.ignore**

- 建议git.ignore
- 这个文件的存放位置，原则上哪里都可以，为了便于让~/.gitconfig文件引用，建议放在用户目录下。
- C:\Users\zjk10\git.ignore

文件内容

```
*.class

*.log

*.ctxt

.mtj.tmp/

*.jar
*.war
*.nar
*.ear
*.zip
*.tar.gz
*.rar

hs_err_pid*

.classpath
.project
.settings
target
.idea
*.iml
```

**在.gitconfig文件中引用忽略配置文件**

- C:\Users\zjk10\.gitconfig

文件内容

```
[user]
	name = scott
	email = zjk1054860443@163.com
[core]
	excludesfile = C:/Users/zjk10/git.ignore
```

- excludesfile 即合理配置文件的物理地址

## 定位Git程序

![](C:/NoteBook/pictures/188772600221263.png =705x)
- 初始化Git 如下，然后找到项目根目录ok即可
![](C:/NoteBook/pictures/182830304239577.png =394x)
- add
![](C:/NoteBook/pictures/131080604227444.png =573x)
- commit
![](C:/NoteBook/pictures/220630904240279.png =568x)
![](C:/NoteBook/pictures/431380804247610.png =781x)

## 版本信息

### 查看版本信息

![](C:/NoteBook/pictures/334981304248072.png =846x)

### 版本穿梭

![](C:/NoteBook/pictures/98881304250468.png =850x)

## 分支

### 切换分支

1. 方式1
![](C:/NoteBook/pictures/353491504245574.png =407x)
2. 方式2
![](C:/NoteBook/pictures/316391704244391.png =300x)
![](C:/NoteBook/pictures/57181904224105.png =394x)

### 创建分支

![](C:/NoteBook/pictures/449951704237937.png =280x)
![](C:/NoteBook/pictures/164811804231071.png =388x)

### 合并分支

#### 正常合并

![](C:/NoteBook/pictures/232082104233052.png =374x)
![](C:/NoteBook/pictures/330172204225937.png =564x)

#### 冲突合并

![](C:/NoteBook/pictures/340442504226546.png =427x)
![](C:/NoteBook/pictures/405683004235714.png =522x)

## 与GitHub的连接

- GitHub插件 IDEA默认安装，如果没有则自行安装

### 口令登录 Token

**GitHub生成口令**

![](C:/NoteBook/pictures/120503904225012.png =144x)
![](C:/NoteBook/pictures/240304004221402.png =243x)
![](C:/NoteBook/pictures/581754004233492.png =414x)

- 设置口令权限

![](C:/NoteBook/pictures/463824304238531.png =554x)

**获取口令并在IDEA中设置**

![](C:/NoteBook/pictures/256234504239826.png =675x)

- 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

![](C:/NoteBook/pictures/590794504240828.png =574x)
![](C:/NoteBook/pictures/304074604241005.png =389x)

### 分享项目到GitHub

![](C:/NoteBook/pictures/429654804226557.png =404x)
![](C:/NoteBook/pictures/516425004242836.png =564x)

### push 推送本地库到远程库

![](C:/NoteBook/pictures/31080405246777.png =470x)

**自定义别名ssh连接**

- 获取ssh

![](C:/NoteBook/pictures/258310205241022.png =545x)

- git@github.com:creatern/JavaSerrior.git

![](C:/NoteBook/pictures/287145504232648.png =426x)
![](C:/NoteBook/pictures/580165504222979.png =454x)

### pull

**注意：**

- push是将本地库代码推送到远程库，如果本地库代码跟远程库代码版本不一致，push的操作是会被拒绝的。
- 也就是说，要想push成功，一定要保证本地库的版本要比远程库的版本高！
- 如果本地的代码版本己经落后，切记要先pull拉取一下远程库的代码，将本地代码更新到最新以后，然后再修改，提交，推送！

![](C:/NoteBook/pictures/596800505238145.png =255x)
![](C:/NoteBook/pictures/306160805249813.png =271x)

### clone 克隆远程库到本地

![](C:/NoteBook/pictures/395101205244921.png =464x)
![](C:/NoteBook/pictures/100281405252432.png =474x)

# Gitee 码云

- 推荐使用https远程链接

## IDEA使用Gitee

**安装Gitee插件**

![](C:/NoteBook/pictures/68383005238783.png =578x)

**设置登录**

![](C:/NoteBook/pictures/350503305225774.png =583x)

### 分享本地库到Gitee

![](C:/NoteBook/pictures/450903405236251.png =226x)

### push

![](C:/NoteBook/pictures/262383605228821.png =472x)

### pull

![](C:/NoteBook/pictures/476803905227978.png =118x)

![](C:/NoteBook/pictures/355153905249363.png =258x)

## Gitee复制Github

- Gitee新建仓库
![](C:/NoteBook/pictures/598674105246657.png =405x)
- Github获取https
![](C:/NoteBook/pictures/13694305248754.png =587x)
- Gitee
![](C:/NoteBook/pictures/366414305253772.png =392x)

### Gitee更新迁移的项目库

![](C:/NoteBook/pictures/584394405253867.png =445x)
![](C:/NoteBook/pictures/119994505241147.png =327x)

# GitLab 自建代码托管平台






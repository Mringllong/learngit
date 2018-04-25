# Git服务端 


## 服务端搭建Git很简单，有更多需求不妨试试Gogs和Gitlab

> 使用Gogs轻松搭建可能比GitLab更好用的Git服务平台 - https://wsgzao.github.io/post/gogs/

``` js
1. #安装git
2. sudo apt-get install git
3. yum install git
4. 
5. #创建一个git用户，用来运行git服务
6. sudo adduser git
7. 
8. #创建证书使用公钥免密码登录(可选)
9. ssh-keygen -t rsa
10. vi ~/.ssh/authorized_keys
11. 
12. #初始化Git仓库
13. sudo git init --bare sample.git
14. sudo chown -R git:git sample.git
15. 
16. #禁用shell登录
17. vi /etc/passwd
18. git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
19. 
20. #在客户端上克隆远程仓库
21. git clone git@server:/srv/sample.git
```

## Git客户端
> Git客户端可以按个人习惯来选择，遵守团队协作中的Git规范标准才是更重要的

* Git - https://git-scm.com/
* TortoiseGit - https://tortoisegit.org/
* SourceTree - https://www.sourcetreeapp.com/

``` js
1. #以最基本的Git命令行为例，先下载Git
2. https://git-scm.com/download/
3. 
4. #配置git提交用户名和邮箱，定义别名方便区分
5. git config --global user.name "你的姓名"
6. git config --global user.email "you@example.com"
7. 
8. #克隆仓库
9. git clone cap@172.28.70.243:/cap/cap.git
10. 
11. $ git clone cap@172.28.70.243:/cap/cap.git
12. Cloning into 'cap'...
13. warning: You appear to have cloned an empty repository.
14. Checking connectivity... done.
15. 
16. #测试推送
17. touch README
18. git add README
19. git commit -m "add readme"
20. git push origin master
21. 
22. Counting objects: 3, done.
23. Writing objects: 100% (3/3), 199 bytes | 0 bytes/s, done.
24. Total 3 (delta 0), reused 0 (delta 0)
25. To cap@172.28.70.243:/cap/cap.git
26. * [new branch]      master -> master

```

## Git常用命令
> 符号约定

* `<xxx>` 自定义内容
* `[xxx]` 可选内容
* `[<xxx>]`自定义可选内容

``` js
1. #初始设置
2. git config --global user.name "<用户名>" #设置用户名
3. git config --global user.email "<电子邮件>" #设置电子邮件
4. 
5. #本地操作
6. git add [-i] #保存更新，-i为逐个确认。
7. git status #检查更新。
8. git commit [-a] -m "<更新说明>" #提交更新，-a为包含内容修改和增删，-m为说明信息，也可以使用 -am。
9. 
10. #远端操作
11. git clone <git地址> #克隆到本地。
12. git fetch #远端抓取。
13. git merge #与本地当前分支合并。
14. git pull [<远端别名>] [<远端branch>] #抓取并合并,相当于第2、3步
15. git push [-f] [<远端别名>] [<远端branch>] #推送到远端，-f为强制覆盖
16. git remote add <别名> <git地址> #设置远端别名
17. git remote [-v] #列出远端，-v为详细信息
18. git remote show <远端别名> #查看远端信息
19. git remote rename <远端别名> <新远端别名> #重命名远端
20 .git remote rm <远端别名> #删除远端
21. git remote update [<远端别名>] #更新分支列表
22. 
23. #分支相关
24. git branch [-r] [-a] #列出分支，-r远端 ,-a全部
25. git branch <分支名> #新建分支
26. git branch -b <分支名> #新建并切换分支
27. git branch -d <分支名> #删除分支
28. git checkout <分支名> #切换到分支
29. git checkout -b <本地branch> [-t <远端别名>/<远端分支>] #-b新建本地分支并切换到分支, -t绑定远端分支
30. git merge <分支名> #合并某分支到当前分支
```
> 以下所有的命令的功能说明，都采用上述的标记的A、B、C、D的方式来阐述。

* workspace: 本地的工作目录。（记作A）
* index：缓存区域，临时保存本地改动。（记作B）
* local repository: 本地仓库，只想最后一次提交HEAD。（记作C）
* remote repository：远程仓库。（记作D）

``` js
1. #初始化
2. git init //创建
3. git clone /path/to/repository //检出
4. git config --global user.email "you@example.com" //配置email
5. git config --global user.name "Name" //配置用户名
6. 
7. #操作
8. git add <file> // 文件添加，A → B
9. git add . // 所有文件添加，A → B
10. 
11. git commit -m "代码提交信息" //文件提交，B → C
12. git commit --amend //与上次commit合并, *B → C
13. 
14. git push origin master //推送至master分支, C → D
15. git pull //更新本地仓库至最新改动， D → A
16. git fetch //抓取远程仓库更新， D → C
17. 
18. git log //查看提交记录
19. git status //查看修改状态
20. git diff//查看详细修改内容
21. git show//显示某次提交的内容
22. 
23. #撤销操作
24. git reset <file>//某个文件索引会回滚到最后一次提交， C → B
25. git reset//索引会回滚到最后一次提交， C → B
26. git reset --hard // 索引会回滚到最后一次提交， C → B → A
27. 
28. git checkout // 从index复制到workspace， B → A
29. git checkout -- files // 文件从index复制到workspace， B → A
30. git checkout HEAD -- files // 文件从local repository复制到workspace， C → A
31. 
32. #分支相关
33. git checkout -b branch_name //创建名叫“branch_name”的分支，并切换过去
34. git checkout master //切换回主分支
35. git branch -d branch_name // 删除名叫“branch_name”的分支
36. git push origin branch_name //推送分支到远端仓库
37. git merge branch_name // 合并分支branch_name到当前分支(如master)
38. git rebase //衍合，线性化的自动， D → A
39. 
40. #冲突处理
41. git diff //对比workspace与index
42. git diff HEAD //对于workspace与最后一次commit
43. git diff `<source_branch> <target_branch>` //对比差异
44. git add <filename> //修改完冲突，需要add以标记合并成功
45. 
46. #其他
47. gitk //开灯图形化git
48. git config color.ui true //彩色的 git 输出
49. git config format.pretty oneline //显示历史记录时，每个提交的信息只显示一行
50. git add -i //交互式添加文件到暂存区
```

## Git使用规范
> Git使用规范提醒

* 使用Git过程中，必须通过创建分支进行开发，坚决禁止在主干分支上直接开发。review的同事有责任检查其他同事是否遵循分支规范。
* 在Git中，默认是不会提交空目录的，如果想提交某个空目录到版本库中，需要在该目录下新建一个 .gitignore 的空白文件，就可以提交了
* 把外部文件纳入到自己的 Git 分支来的时候一定要记得是先比对，确认所有修改都是自己修改的，然后再纳入。不然，容易出现代码回溯
* 多人协作时，不要各自在自己的 Git 分支开发，然后发文件合并。正确的方法应该是开一个远程分支，然后一起在远程分支里协作。不然，容易出现代码回溯（即别人的代码被覆盖的情况）
* 每个人提交代码是一定要 git diff 看提交的东西是不是都是自己修改的。如果有不是自己修改的内容，很可能就是代码回溯
* review 代码的时候如果看到有被删除掉的代码，一定要确实是否是写代码的同事自己删除的。如果不是，很可能就是代码回溯
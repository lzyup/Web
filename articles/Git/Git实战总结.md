# Git实战总结
## 一、基本概念
- 废话不多少说，先上图
![Git流程图][1]

- **工作区**：顾名思义就是你本地工作的地方
- **暂存区**：位于文件夹`/.git/index`（我们执行 `git add`命令就是将文件提交到暂存区）
- **本地仓库**：就是我们电脑上保存版本数据的地方。位于文件夹`/.git/object`（我觉得这就是Git厉害的地方，分布式每个人都有自己的一个仓库，不用联网就可以提交）
- **远程仓库**：我们用Git多人协作，怎么保持跟别人的代码同步呢，就是靠我们自己的本地仓库和远程仓库之间进行对应的拉取(pull)和推送(push)操作。

## 二、日常我们需要进行的操作场景，十万个怎么办
1、**Q:  修改了某个文件，还没(commit)提交，想还原怎么办？**
 A：`git checkout [filename]` 

2、**Q：执行了`git commit -m 'message'`操作，想撤回怎么办？**
A：`git reset --hard [commit]`回退到你想回退的版本号 。
- `--hard`表示本地仓库，暂存区和你的工作区全部恢复。
- `--soft`表示只恢复本地仓库。
- `--mixed`表示只恢复本地仓库和暂存区，如果不写，默认是`--mixed`

3、**Q：执行了`git push`操作，想撤回怎么办？**
A：
- `git reset --hard [commit]` 将本地仓库还原
- `git push --force` 将本地仓库强制推送到远程仓库

4、**Q：已经修改了文件，但是此时要切换到其他分支，处理紧急情况，文件又不想提交怎么办？**
A：
- `git stash` 先将文件保存到暂存区然后切换到你想去的分支
- `git stash pop`当回到当前分支时候释放暂存区里面之前修改的代码

5、**Q：想删除本地分支，怎么办？**
A：`git branch -d <local-branchname>` 注意此时你不应该在你想要删除的这个分支上操作

6、**Q：想删除远程分支，怎么办?**
A：`git push orgin :<remote-branchname>`就是推送一个空的本地分支到你想删除的远程分支即删除了

7、**Q：本地分支名字取的不好，想重命名怎么办?**
A：`git branch -m <new-branch-name>`

8、**Q：想创建并切换到一个本地分支怎么办？**
A：`git checkout -b <branch-name>`

9、**Q：想创建分支并切换到一个本地分支，还要这个本地分支关联上对应的远程分支，怎么办？**
A：`git checkout -b <branch-name> orgin/<branch-name>`

10、**Q：想删除某个已经提交到远程仓库的文件怎么办？**
A：
- `git rm -f --cached [filename]` 从暂存区删除文件
- `git commit -m "message"` 提交
- `git push origin master` 推送到远程仓库
 
## 三、容易混淆的概念理解
- **` git fetch`:** 取回所有分支的更新，通过这个命令取回的代码对你本地开发的代码是没有影响的。
- **` git pull = git fetch + git fetch `** 

**以上就是我常遇到的一些场景希望能帮助到你**

##  四、附加两个最常用的命令的全称
```
$ git push <远程主机名> <本地分支名>:<远程分支名>

$ git pull <远程主机名> <远程分支名>:<本地分支名>
````


















[1]:http://www.ruanyifeng.com/blogimg/asset/2014/bg2014061202.jpg

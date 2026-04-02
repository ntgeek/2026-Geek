# git idea

> 郭瑨

## 学习背景

阶段1一直使用的GitHub&typora，对于Git这个系统十分的不熟悉。

## 学习内容的目录

- Git Bash完整提交流程。
-  Git Bash 与其他更方便、更有效率的提交方式。
- 版本回滚和冲突解决。

## 学习内容

- Git Bash完整提交流程。

  1. 在typora创建一个名为“hello.md”的文件，输入`# hello+xx`，然后保存并关闭。

  2. 打开Git Bash用cd命令切换到文件所在文件夹，使用pwd验证。

  3. git init 初始化，配置信息身份（身份&仓库）

     eg：

     ```
     git init
     git config --global user.name "xx"
     git config --global user.email "xx   @qq.com
     ```

  4. 添加文件至暂存区，然后再存入本地仓库，关联远程仓库，最后推送至远程。

     eg：

     ```
     git add hello.md
     git commit -m "第一次提交：添加hello.md"
     git remote add origin <远程仓库地址>
     git push -u origin main
     ```

- Git Bash 与其他更方便、更有效率的提交方式。

  1. Git Bash相对于麻烦，需要自己输入命令指令代码，容易因拼写，命令结构错误而报错，新手初次使用容易感到迷惑。
  2. vscode&typora更加的方便，功能较多，页面较为直观，所见即所得，更加的方便，有效率。

- 版本回滚和冲突解决。

  1. 回滚

     - 修改最近一次（git commit）

       适用于刚在本地提交完，还未推送至远程：

       eg：

       ```
       git commit--amend
       ```

     - 撤销本地提交(git  reset)

       适用于一个或者多个只在本地提交，还未推送至远程：

       1. --soft：

          撤销提交，保留至工作区，还可重新编辑：

          eg:

          ```
          git reset--soft HEAD~N
          ```

       2. --mixed:

          撤销提交，保留至工作区，还可重新编辑：

          eg:

          ```
          git reset--mixed HEAD~N
          ```

       3. --hard:

          撤销提交，彻底删除，不可重新编辑：

          eg:

          ```
          git reset--hard HEAD~N
          ```

     - 撤销远程提交(git revert)

       1. 单个：

          eg：

          ```
          git revert <commit-id>
          ```

       2. 多个：

          eg:

          ```
          git revert <old-commit-id>
          ```

     - 误删怎么处理：git reflog 

        这个命令会记录你所有的操作历史，包括那些“被删除”的提交。你可以通过它找到误删提交的哈希值，然后使用  git reset  或  git checkout  将其恢复

     - 最后记得`git push`!

  2. 冲突

     当 Git 无法自动合并两个分支对同一文件同一位置的修改时，就会发生冲突 。解决冲突的过程其实就是你手动决定最终保留哪些代码。

     1. 当你执行  git merge  或  git pull  时，如果发生冲突，Git 会提示“自动合并失败”。你可以通过  git status  命令查看所有存在冲突的文件 。

     2. 打开冲突文件，你会看到 Git 插入的特殊标记

        eg：

        ```
        <<<<<<< HEAD
          --这是你当前分支的代码
        =======                                   --这是要合并进来的分支的代码
        >>>>>>> branch-name                       --你需要手动编辑这部分内容（决定是保留你的代码、对方的代码，还是将两者结合 ）。
        ```

     3. 解决完一个文件的冲突后，使用  git add <文件名>  命令告诉 Git 这个文件的冲突已解决。当所有冲突文件都处理完毕后，执行  git commit  来完成合并 。

## 参考资料

- 小红书[xhs]( http://xhslink.com/o/3J5YBUksDCs )
- 菜鸟教程
- 豆包&千问




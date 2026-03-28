# Git学习感想

## 学习背景
阶段一直接用Github创建，故对Git不是很了解
## 学习内容的目录
-  关于Git提交练习文档（通过Git Bash上传）  
-  关于其他提交方法  
-  学习笔记  
-  版本回滚  
-  冲突解决  

## 学习内容  
#### 1. 关于Git提交练习文档（通过Git Bash上传） 
   - **准备阶段**   
     进入项目目录：cd/文件地址
     初始化：git init  
   - **创建文件**  
     生成文件：echo "# Hello,曾新颜" > 文件名  
     验证：cat 文件名  
   - **暂存提交**  
     暂存：git add 文件名  
     提交到本地仓库：git commit -m "说明" 
   - **推送至远端**  
     关联远程仓库：git remote add origin 地址  
     推送到main分支：git push origin main  
#### 2. 关于其他提交方法
   - **VSCode使用感受**：可以下载插件，功能也很多，容易上手，所见即所得，除了Markdown还能写代码，很方便。
   - **GitBash是否麻烦**：GitBash命令行需要记忆指令、手动输入路径，对新手有一定学习成本，且纯文本操作不够直观，容易因拼写错误导致失败。但它稳定性强、可自动化程度高，适合熟悉后高效批量操作。
   - **其他方法**  
    1. IDE 集成工具：VS Code、IntelliJ 等编辑器内置 Git 可视化面板，可直接点击「暂存」「提交」「推送」，无需记忆命令，所见即所得。  
    2. GUI 客户端：GitHub、 Desktop等，用图形界面展示分支、提交历史，拖拽即可完成操作，适合新手快速上手。     
    ​3. 脚本/别名：将高频操作封装为 Shell 脚本或 Git 别名，简化命令输入。
#### 3.学习笔记
1. 克隆远程仓库到本地：git clone <地址>  
2. 初始化（从0开始创建时）：git init  
3. 将文件添加到暂存区：git add <文件>  
4. 生成本地版本记录：git commit -m "说明"  
5. 绑定远程仓库（clone后自动完成）：git remote add origin <地址>  
6. 推送代码至远端：git push origin main  
7. 拉取远程最新代码：git pull origin main  
8. 查看当前仓库状态：git status  
9. 查看提交历史：git log   
#### 4.版本回滚  
   - **未推送到远端**  
     保留代码，撤销提交：git reset --soft HEAD^   
     丢弃代码，彻底撤销：git reset --hard HEAD^  
   - **已经推送**  
     git revert  
     git push origin main  
#### 5.冲突解决 
   - 1. 打开冲突文件     
   - 2. 手动编辑  
        git add 冲突文件名  
        git commit -m "fix:解决文件冲突"  
        git push origin main    

## 参考资料
1. [Google](http://www.Google.com)  
2. [Youtube](https://www.youtube.com/watch?v=mJ-qvsxPHpY)  
3. [Github](https://github.com/chuyu2025/chu-star/tree/main/Up_GitHub)
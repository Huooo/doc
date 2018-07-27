#GIT


### Git Commonds

```bash
    
    === init ==================================
    
    git init

    === config =======================

    git config --list/-l

    git config user.name "Huooo"
    git config user.email "hu71an@gmail.com"
    git config --global user.name "Huooo"
    git config --local user.name "Huooo"

    git config --global core.excludesfile ~/.gitignore // 创建全局的.gitignore文件

    === clone ===========================

    git clone https://github.com/Huooo/git-test.git
    git clone https://github.com/Huooo/git-test.git 本地文件名

    === branch ===========================

    git branch 分支名字
    
    git branch
    git branch -r
    git branch -a

    git branch -d 分支名字
    git branch -D 分支名字
    git push origin --delete 分支名字

    git branch -m 旧分支名 新分支名 // 重命名分支
    git branch -M 旧分支名 新分支名 // 重命名分支，强制

    === status ====================================

    git status
    git status -s // ???

    === add ============================

    git add 文件名1 文件名2 文件名3 ...
    git add --all/-A/.                  // 将所有工作区文件添加进暂存区
    git add --update/-u          // 将所有【更新文件】添加进暂存区
    git add --patch/-p           // 逐一查看文件修改内容然后决定是否讲该文件添加至暂存区

    git add 文件名 --edit      // 在git add的时候进入编辑环境
    git add *.html              // 适当使用通配符

    === commit =============================

    git commit -m "此次提交的相关描述"

    git commit --amend -m "追补遗忘的文件提交到最近的一次提交中"  // == git reset --soft + git commit -m 
    git commit --amend -c HEAD  // 同上

    git commit --date="定义提交时间(可过去可未来)如：2017-10-10T12:00:00" -m "此次提交的相关描述"  // 这不是有点阔怕了

    === pull ============================

    git pull 远程仓库名字 远程仓库的分支名字   // == git pull --merge === git fetch + git merge
    git pull --rebase

    === push =============================

    git push 远程仓库名字 分支名字
    git push -u 远程仓库名字 分支名字     // -u是让此次push记住远程仓库和分支名字，下次执行可直接执行git push

    === rebase =============================

    重点

    git rebase --onto // https://www.cnblogs.com/rickyk/p/3848768.html
    git rebase -i 哈希码 // 修改某次提交的内容，见Explanation

    === merge ==========================

    重点

    git merge 分支名1 分支名2 // => git checkout 分支名2 + git merge 分支名1
    git merge --no-ff 分支名1 分支名2 // 在没有冲突的情况下，创建一个冲突节点来记录合并的提交操作

    === fetch ==========================

    git fetch 
    git fetch -p

    ==== diff ==========================

    git diff
    git diff 文件名

    === blame ===========================

    git blame 文件名 // 此操作查询的是每行代码的详细日志

    === log ==============================

    git log
    git shortlog

    git log -10 // 数量

    git log 分支名字 // 分支名

    git log --after="2017-8-31"  // 日期
    git log --before="2017-8-31" 
    git log --after="2017-8-30" --before="2017-8-31"

    git log --author="Huooo" // 作者
    git log --author="Huooo\|Cici Hu"

    git log --grep="README" // 提交信息
    git log -S "Git Commands" // 提交内容

    git log master..develop // 提交范围

    git log -- README.md // 文件

    git log --merges  // 合并
    git log --no-merges // 非合并

    git log --oneline // 一行显示

    git log --decorate // 分支标签

    git log --stat // 行数差异

    git log -p // 详情差异

    git log --graph // 日志图形

    git log --pretty=format:"%cn committed %h on %cd"  // 自定义

    === show =============================

    git show 哈希码 // 显示某个节点的具体修改
    git show tag名字 // 显示tag节点的具体修改

    === reflog ===========================

    git reflog // 记录本地库的所有git操作

    === cherry-pick =====================

    git cherry-pick 哈希码 // 将特定的节点抽出放入当前分支

    === ignore ============================
    
    vim .gitignore

    === clean =============================

    只作用于【未被tracked的文件】

    git clean -n        // 显示可清除的【未被tracked的文件】
    git clean -f        // 清除所有【未被tracked的文件】
    git clean -f 文件名  // 清除指定【未被tracked的文件】
    git clean -df        // 清除所有【未被tracked的文件/文件夹】
    git clean -df 文件名  // 清除指定【未被tracked的文件/文件夹】
    git clean -xf        // 暂时没用上


    === rm ====================================
    
    git rm 文件名 
    git rm --cached 文件名 // 此操作取消对某个文件的追踪，是git add的逆向操作

    === stash ===========================

    git stash 
    git stash list
    git stash pop 名字    // git stash pop == git stash apply + git stash drop 
    git stash apply 名字    
    git stash drop 名字

    === revert ======================
    
    不懂

    git revert 哈希码 
    git revert 哈希码 --no-edit

    === mv ======================

    git mv 旧文件名 新文件名    // rename file name
    git mv 旧文件路径 新文件路径  // remove file

    === tag ====================

    git tag 
    git tag 新建tag名字
    git tag 新建tag名字 某次提交的hash号
    git tag -d tag名字

    git push --tags

    git checkout tags/tag名字

    === checkout =====================

    git checkout 分支名1 // 当前分支切换到分支名1上
    git checkout -b 新分支名 // => git checkout -b 新分支名 当前分支
    git checkout -b 新分支名 Hash值 // 从某个hash值点检出并命名为一个新的分支
    
    git checkout -- 文件名     // 将工作区的文件撤销修改
    git checkout -- "*.c"   // 撤销后缀是.c的文件   
    git checkout -- .   // 撤销工作区所有文件
    
    === reset ======================

    只作用于【被tracked的文件】

    git reset 文件名                   // 将暂存区的文件回退至工作区
    git reset --soft HEAD^            // 将最近一次提交回退至暂存区
    git reset --mixed HEAD^            // 将最近一次提交回退至工作区
    git reset --hard HEAD^            // 将最近一次提交回退上一次提交

    其实被git reset之后，远程仓库的提交依然存在，如果git pull本地依然会获取到相应提交。
    应该在git reset之后对本地进行强推git push -f操作，使得远程也回退到相应节点。但是要注意git reset不要在公共分支上使用。

    === remote =======================

    git remote
    git remote -v   // 查看远程仓库的url
    git remote add 定义远程仓库的名字 远程仓库的url

    === repack ===========================
    
    不太懂
    git repack 
    git repack -d

    === grep ===========================

    git grep 关键词
    git grep 关键词 文件名

    === bisect =======================
    
    不懂

    git bisect good 哈希码
    git bisect bad 哈希码

    === submodule ======================

    git submodule 仓库URL

    


```


### Explanation

- git status : 日常查询当前文件的状态
- git log : 日常查询历史纪录
- git reflog : 日常查询git所有操作记录
- git fetch : 日常获取远程最新状态
- git rebase -i 哈希码： 修改某次提交的内容，所做修改会修改相应的提交的哈希码，其实已经改变了提交记录，进入之后的参数解释：
```bash

    git rebase -i 哈希码

    ?? 如果利用这个撤回提交，内容被撤回到哪里？

    pick    - 表示执行此次提交；
    reword  - 表示执行此次提交，但要修改备注内容；
    edit    - 表示可以修改此次提交，比如再追加文件或修改文件；
    squash  - 表示把此次提交的内容合并到上次提交中，备注内容也合并到上次提交中；
    fixup   - 和squash类似，但会丢弃掉此次备注内容；
    exec    - 执行命令行下的命令；
    drop    - 删除此次提交。
```
- git checkout --track -b branch_1 origin/branch_1 ?????
- git push --set-upstream remote_name branch_local branch_remote ?????


### Links
- [Git Book]( https://git-scm.com/book/zh/v1/%E8%B5%B7%E6%AD%A5 )
- [Git Docs](https://git-scm.com/docs/git-rebase)
- [Zhongyi Tong](https://github.com/geeeeeeeeek)的[git-recipes](https://github.com/geeeeeeeeek/git-recipes)

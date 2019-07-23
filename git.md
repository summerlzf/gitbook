# git仓库迁移

git仓库迁移，包含所有的分支、tag、日志（提交记录），只需要执行以下几步操作即可



`git clone --mirror <旧仓库URL>`

`cd <拉下来的仓库目录>`

`git remote set-url origin <新仓库URL>`

`git push -f origin`



# git分支

#### 查看分支（本地/远程）

`git branch`

`git branch -a`

#### 本地分支切换（待切换分支本地已存在）

`git checkout <branch>`

#### 本地新建分支

`git checkout -b <branch>`

#### 从远程分支checkout到本地（对应的本地分支不存在）

`git checkout -b <branch> origin/<branch>`
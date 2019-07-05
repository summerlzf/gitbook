# git仓库迁移

git仓库迁移，包含所有的分支、tag、日志（提交记录），只需要执行以下几步操作即可



`git clone --mirror <旧仓库URL>`

`cd <拉下来的仓库目录>`

`git remote set-url origin <新仓库URL>`

`git push -f origin`


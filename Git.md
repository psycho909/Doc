# Git 使用
## .gitignore 忽略文件
> 使用.gitigonore進行設定，可選擇性不推送到遠端
```javascript
//創建.gitignore文件
//設定方式

*.a      //忽略所有 .a 結尾的文件
!lib.a    //但 lib.a 除外
/TODO     //僅僅忽略項目根目錄下的 TODO 文件，不包括 subdir/TODO
build/   //忽略 build/ 目錄下的所有文件
doc/ *.txt //會忽略 doc/notes.txt 但不包括 doc/server/arch.txt
node_modules //忽略node_modules目錄下的所有文件
```
## 創建git 用戶名 及 mail
```git
git config --global user.name "My Name"
git config --global user.email myEamil
```
## 創建一個新倉庫 git init
```git
git init
```
## 檢查狀態 git status
```git
git status
```
## 暫存 git add
```git
git add hello.text
```
> 如要提交目錄下的所有內容
```git
git add -A
```
> 可再次使用`git status` 查看
```git
$ git status
On branch master
Initial commit
Changes to be committed:
  (use "git rm --cached ..." to unstage)
  new file:   hello.txt
```
## 提交 git commit
```git
git commit -m "訊息內容"
```
> 合併上一次提交(用於反覆修改)
```git
git commit -m 'xxx'
```
> 將add和commit合為一步
```git
git commit -am 'xxx'
```
> 連接遠端倉庫 - git remote add
```git
git remote add origin https://psycho909/handlerbars-demo.git
```
> 可查詢連接遠端倉庫清單 - `git remote -v`
```
git remote -v
```
## 上傳到服務器 - git push <remote name> <branch name>
> `git push`命令會有兩個參數，遠端倉庫的名字，以及分支的名字：
```git
git push <remote name> <branch name>
git push origin master
```
## 刪除遠端Branch - git push <remote name> :<branch name>
```git
git push ES603 :black
```
## Clone 倉庫 - git clone
```git
git clone https://psycho909/handlerbars-demo.git
```
## 從服務器上拉取代碼 - git pull
```git
git pull origin master
```
# 分支
## 顯示本地分支
```git
git branch
```
> 顯示所有分支
```git
git branch -a
```
## 創建分支
```git
git branch amazing_new_feature
```
## 切換分支 - git checkout
> 單獨使用 `git branch` 可查看目前所有分支狀態:
```git
git checkout amazing_new_feature
```
## merge分支 - git merge
> 在 amazing_new_feature 分支中增加一個footer.htaml
```git
git add footer.html
git commit -m "add footer.html"
```
> 此時回到 master 分支
```git
git checkout master
```
> 會發現 新創建的 footer.html 不見了，因為`master`分支上沒有footer.html
> 把 `master` & `amazing_new_feature` 合併後 `master` 就有footer.html
```git
git merge amazing_new_feature
```
## 刪除分支
```git
git branch -d amazing_new_feature
// 删除本地分支，如果本地还有未合并的代码，则不能删除
git branch -d qixiu/feature
// 强制删除本地分支
git branch -D qixiu/feature 
```
## 文件更動
> 刪除文件實需要使用`git rm` 檔名 來通知倉庫刪除文件
```git
git rm 檔名
git commit -m "XXXX"
git push origin master
```
> 當多個文件刪除時，並且確定刪除可以使用 `git add -u`
```git
git add -u
git commit -m "XXX"
git push origin master
```
## 撤销 commit
```js
// 会将提交记录回滚，代码不回滚
git reset b14bb52

// 会将提交记录和代码全部回滚
git reset --hard b14bb52

// 将部分代码文件回滚
git checkout -- files
```
## 查看操作记录
```js
git reflog
```
## Git常用指令
```javascript
git status //常看本地碼狀態
git add files //添加代碼到緩存區
git commit -m '提交內容的備註' //提交代碼到本地倉庫
git branch //顯示本地分支
git branch branchName //創建分支
git fetch origin origin/remote_branch:your_branch //將遠程分支下載到本地，並創建分支
git branch -d branchName //刪除分支 -D強制刪除分支
git checkout -b branchName //不加-b就是普通切換分支
git fetch -p //同步遠端分支狀態
git pull -r origin branchName //fetch遠端代碼到本地，並以rebase的方式合併代碼
git push origin branchName //更新本地代碼到遠端
git push <remoteName> :branchName //刪除遠端分支

git merge <name> // 將目標分支合併到當前分支
git clean -d -f // 刪除當前目錄下沒有被track過的文件和文件夾

git log // 顯示commit的詳細日誌
git log --pretty=oneline // 只顯示commit的ID與描述
git reset --hard HEAD // 回退到最近的一個版本
git reset --hard commit_id // 根據commit_id回退到指定版本
```
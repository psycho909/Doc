#git
# Git 使用
## Merge還原
```git
<!-- ^ 倒退一步 -->
<!-- ^^ 倒退兩步 -->
<!-- ~50 倒退50步 -->
<!-- 殺掉最後一次commit -->
git reset HEAD^ --head
```
## rebase 還原
```git
git reset ORIG_HEAD --head
```
```git
git reset <hash> --head
```
## 刪除分支
```git
<!-- 隱藏branch 看不見，但還存在 -->
git branch -D <branchName>
```
## 刪除私密資料
```git
git filter-branch --tree-filter "rm -f config/<file.name>"
```
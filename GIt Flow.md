#git
# 最佳實踐 Git Flow

## **master 分支** 

也就是我們經常使用的主線分支，這個分支是最近發佈到生產環境的代碼，這個分支只能從其他分支合併，不能在這個分支直接修改。

## **develop 分支** 

這個分支是我們的主開發分支，包含所有要發佈到下一個 release 的代碼，這個分支主要是從其他分支合併代碼過來，比如 feature 分支。

## **feature 分支** 

這個分支主要是用來開發一個新的功能，一旦開發完成，我們合併回 develop 分支進入下一個 release。

## **release 分支** 

當你需要一個發佈一個新 release 的時候，我們基於 Develop 分支創建一個 release 分支，完成 release 後，我們合併到 master 和 develop 分支。

## **hotfix 分支** 

當我們在 master 發現新的 Bug 時候，我們需要創建一個 hotfix, 完成 hotfix 後，我們合併回 master 和 develop 分支，所以 hotfix 的改動會進入下一個 release。
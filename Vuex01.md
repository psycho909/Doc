# Vuex
## Actions / Mutations / State
### Actions
1. 定義整個App的所有行為，使用者在前端元件觸發的事件會`dispatch`給`Actions`，接著`Actions`會去`commit mutation`，進而讓相對應的`mutation handler`去做更改狀態的動作。
1. 在這個階段因為常要從後端讀取資料，所以也可以進行`非同步地`跟`Backend API`溝通。
### Mutations
1. 透過`commit`接收`Actions`傳遞的資料與行為，並經過計算處理過後改變State。
1. 每個`Mutation`都有一個字串型態的`事件類型(type)`和一個`回調函數(handler)`。
1. `回調函數(handler)`就是我們實際更改狀態的地方，第一個參數即帶入`State`。
1. 注意只有使用`commit mutation`才能改變在`Store`中的狀態，這個動作就像是`註冊事件`一樣，是不可以直接調用`mutation handler的`。
### State
1. 用一個物件型態記錄整個App的所有狀態。
1. 讓`Mutation`去更改狀態。
1. 雖然建議是將App的所有狀態全部放入`Store`，但是Vuex還是保有彈性，可以讓元件保有自己的局部狀態。
## Actions / Mutations差別
### Actions
1. 更改狀態:`Actions`只能透過`commit mutation`去提交事件，不能直接更改狀態。
1. 處理的事件種類:可同時處理非同步事件(call API)，這樣就可以在此階段等待API回應的時間，接收到的資料再`commit`給`Mutations`，`Mutations`收到的資料就會是即時的。
### Mutations
1. 更改狀態:`Mutations`透過`mutation handler`直接實際更改狀態。
1. 處理的事件種類:只能處理同步事件。
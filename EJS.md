# EJS

## 標籤

- `<%` 'Scriptlet' 標籤, 用於控制流，沒有輸出
- `<%=` 向模板輸出值（帶有轉義）
- `<%-` 向模板輸出沒有轉義的值
- `<%#` 註釋標籤，不執行，也沒有輸出
- `<%%` 輸出字面的 '<%'
- `%>` 普通的結束標籤
- `-%>` Trim-mode ('newline slurp') 標籤, 移除隨後的換行符

## 示範

```ejs
<% if (user) { %>
  <h2><%= user.name %></h2>
<% } %>
```

## include

包含要麼是絕對路徑，或者如果不是的話，被視為相對於調用`include`的模板的路徑（需要`filename`選項）。 例如，你在`./views/users.ejs`中包含`./views/user/show.ejs`，你應該使用`<%- include('user/show') %>`。

你可能會用到原始輸出標籤（`<%-`）避免二次轉義HTML輸出。

```ejs
<ul>
  <% users.forEach(function(user){ %>
    <%- include('user/show', {user: user}) %>
  <% }); %>
</ul>
```

包含的內容在運行時插入， 所以你可以在`include`調用中使用變量作為路徑（例如`<%- include(somePath) %>`）。在你頂級數據對象中的變量都可以用於所有的包含，而局部變量需要傳遞進來。
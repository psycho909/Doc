# Handlebars使用
## Handlebars創建模板
>0. 想使用模板
```html
<div id="header"></div>
```
>1. 創建模板
```html
<script id="header-template" type="text/x-handlebars-template">
    <div class="header-box">
        <h1>name:{{data}}</h1>
    </div>
</script>
```
>2. 定義模板
```json
var data={name:"Hello"}
```
```javascript
$("#header").html(Handlebars.compile($("#header-template").html())(data))
```
>3. 模板調用完成
```html
<div id="header">
    <div class="header-box">
        <h1>name:Hello</h1>
    </div>
</div>
```
>4. `{{@index}}`可顯示KEY值

## 遍歷數據使用
>1. each block helper
>> 可以使用`{{#programme}} {{language}} {{/programme}}`
```json
{
  programme: [
    {language: "JavaScript"},
    {language: "HTML"},
    {language: "CSS"}
  ]
}
```
```html
<div class="header-box">
    {{#programme}}
        <h1>name:{{language}}</h1>
    {{/programme}}
</div>
```
>> json數據可以使用 `{{#each this}} {{name}} {{/each}}`
```json
[
    {name:"html"},
    {name:"css"},
    {name:"handlebars"}
]
```
```html
<div class="header-box">
    {{#each this}}
        <h1>name:{{name}}</h1>
    {{/each}}
</div>
```
>2. if else block helper

>> 可以指定條件渲染DOM，如果參數返回 `false、undefind、null`，則不會渲染DOM，如果存在`{{#else}}`則執行`{{#else}}`後面的渲染
```json
{  
    info:['HTML5','CSS3',"WebGL"],
    "error":"數據取出錯誤"
}
```
```html
{{#if list}}
<ul id="list">  
    {{#each list}}
        <li>{{this}}</li>
    {{/each}}
</ul>  
{{else}}
    <p>{{error}}</p>
{{/if}}
```
>> 這裡`{{#if}}`判斷是否存在list數組，如果存在則遍歷list，如果不存在輸出錯誤信息

>3. unless block helper
>
>> 如果為`false`則渲染

>4. with block helper
```json
{
  title: "My first post!",
  author: {
    firstName: "Charles",
    lastName: "Jolley"
  }
}
```
```html
<div class="entry">
    <h1>{{title}}</h1>
    {{#with author}}
    <h2>By {{firstName}} {{lastName}}</h2>
    {{/with}}
</div>
```
>5. 其他
>> `{{../}}`可使用Path訪問
```json
{
    "person":{ "name": "Alan" },
    "company":{"name": "Rad, Inc." }
};
```
```html
{{#with person}}
    <h1>{{../company.name}}</h1>
{{/with}}
```

## 自定義helper
> Handlebars，可以從任何上下文可以訪問在一個模板，你可以使用Handlebars.registerHelper()方法來註冊一個helper。
>
> 調用方法
>
> `debug`可視為函數名`(v1,v2)`回傳入變量
```javascript
Handlebars.registerHelper('debug',function(v1,v2){
    if(v1>v2){
        return 'block';
    }else{
        return 'none';
    }
})
```
> 使用`{{#debug 3 2}}{{/debug}}` 傳入`3、2`進行比較也可以傳入`block`
```html
<h1 data-main="{{#debug 3 2}}{{/debug}}">Main</h1>
```
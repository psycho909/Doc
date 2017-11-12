# RubyOnRails
> `if current_page? '/about' `可判斷是否是`路徑`
## 第一個頁面
> `rails new <name>`產生一個專案
### 創建`controller`
> `rails generate controller <name>`<br>
> 在對應資料夾下產生`controller/name`<br>
> 開始設定第一個頁面<br>
### 前去`routes`與`controller`及`view`做關連
```ruby
# routes
get 'welcome/hello' => 'welcome#hello'

# controller
def hello
end

# view
<h1>Hello World</h1>
```
## Rails使網頁進行關聯
> `views`創建`index.html.erb`頁面
> 現在`controller`創建<br/>
```ruby
    def index
    end
```
> 進入`config/routers`
```ruby
    #get <路徑> => <controller>#mehtods
    get '/stores' => 'stores#index'
```
## 21.Rails建立資料庫
> Rails使用model與資料庫進行互動<br/>
> 進行Add,Delete,Update都是透過model進行處理
```ruby
# 注意
# model: rails g 名稱單數
# controller: rails g 名稱複數
rails generate model <name>
# 產生了數個檔案
# 其中 db/migrate & app/models/store.rb 重要
```
> `migrate`是創建了資料表<br>
> 主要用處是定義資料表的樣子
```ruby
class CreateStores < ActiveRecord::Rigration[5.1]
    def change
        create_table :stores do |t|
            # 創建要使用欄位
            t.string :name
            t.string :description
            t.string :phone
            t.string :address
            
            t.timestamps
        end
    end
end
```
```ruby
# 執行後會正式對資料表產生資料庫
rails db:migrate
# 可在 db/schema.rb 裡面看到所創建的資料表
```
### Rails資料庫 Add,Delete,Update使用
```ruby
# 進入rails 環境
rails console / rails c
# 第一種創建方式
# <databasename>.new
# 變量形式創建
store=Store.new
store.name = "便當店"
# 儲存資料庫
store.save

# 第二種創建方式 不用使用save會自動儲存
Store.create({
    name:"便當店",
    phone:"0977"
})

# 查看所有資料庫內的資料
# Store.all出來的是整群 ActiveRecord的物件，目的是讓你可以處理很多筆資料，常用來在網頁中顯示
Store.all

# 查看單筆資訊
# Store.find是幫你找出【一筆】ActiveRecord的物件，讓你能操作一筆資料，對這筆資料做修改、刪除、讀取等
# 使用變量方式查找
# <databasename>.find <id>
store=Store.find 1
# 變更 id 1 name的資料
store.name = "麵店"
# 在呼叫 save 完成更新
store.save

# 第二種更新方式 不用呼叫save
store.updata name: '牛肉麵店'

# 刪除
store.destroy
```
### 在網頁中調用資料庫
> 先在`controller`進行呼叫
```ruby
# 使用 @ 定義全局變量使用
def index
    @stores=Store.all
end
```
> 在網頁view中調用
```ruby
<% @stores.each do |store| %>
    <%= store.name %>
<% end %>
```
## Rails讓使用者在頁面中直接創建資料進入資料庫
### 創建頁面
> 創建html並在`controller`創建，`routers`建立關聯
```ruby
# controller
def new
end
# routers
get '/store/new' => 'store#new'
```
### Form表單使用
> `form`表單`action`,`method`<br>
> action 是指把資料送往的路徑<br/>
> method內有get,post<br/>
> `GET`是指當使用者跟伺服器要資料時使用<br/>
> `POST`送出資料給伺服器，在接收傳回來的資料
### 資料庫獲取
> 在`routers`中獲取
```ruby
# 在routers接收
post '/stores' => 'stores#create'
# 在controller 接資料後處理
def create
    # post會把資料儲存在params變數裡面
    # params是rails定義好的
    # params 是一個類似 hash 的物件，可以把它的操作方式想像成hash
    store=Store.new
    
    store.name=params[:name]
    store.descr=params[:descr]
    store.phone=params[:phone]
    store.address=params[:address]

    store.save
    # 重新導向回store頁面
    # stores_path = '/stores'
    redirect_to '/stores'
end
```
> 跳出`InvalidAuthenticityToken `資安疑慮錯誤
```html
<!-- 可使用此方法來阻止 -->
<input type="hidden" name="authenticity_token" value="<%= form_authenticity_token %>"></input>
```
## 24.重構表單
### `redirect_to 路徑別稱`
> 控制台輸入`rails routes`可看到rails幫我們定義的所有路徑別稱<br/>
> 也可以自定義路徑別稱`as:'name'`
```ruby
Rails.application.routes.draw do
  root 'pages#homepage'
  get '/stores' => 'stores#index'
  # create new store
  # 可自定義路徑別稱 as: 'name'
  get '/stores/new' => 'stores#new',as: 'new_store'
end
```
### 優化Form表單操作
> 使用hash方式
```html
<label>店家地址:</label>
<input type="text" name="store[address]"></input><br/>
```
> 產生
```json
"store": {
    "address": "test"
}
```
> 在controller
```ruby
def create
    # 直接把接收到的hash傳入
    store=Store.new params[:store]
    
    store.save
    
    redirect_to stores_path
end
```
> 產生資安問題錯誤`ForbiddenAttributesError`<br/>
> 因為可以透過html隨便傳輸數據，都會接收到，所以得設定哪個可以接收哪個不可以接收
```ruby
private
def stores_params
    # 只有address可以更改的
    # 如果使用在塞入其他資訊，rails則會忽略掉
    params.require(:store).permit(:address)
end
```
> 重新傳入
```ruby
def create
    # 直接把接收到的hash傳入
    store=Store.new stores_params
    
    store.save
    
    redirect_to stores_path
end
```
### 自動產生`Form`表單
> 可快速解決Form表單，並解決authenticity_token
```ruby
<%= form_for Store.new do |f| %>
    <%= f.label :name,'店家資訊',class:'label' %>
    <%= f.text_field :name %>

    <%= f.submit '送出' %>
<% end %>
```
## 25.讓資料顯示出來
> 單筆資料顯示出來如:`/stores/1`
```ruby
# 在routes 創建變量形式
# '/stores/:<varname>'
# '/stores/1'
# params ={ id:1 }
get '/stores/:id' => 'stores#show'

# controller
def show
    @store = Store.find params[:id]
end
```
### 頁面常用組件放置
> `view`在index中設置常用組件
```html
<!-- 通常放置在application -->
<!-- 檔名通常設置 _navbar -->
<!-- 並且不用設置相對路徑 -->
<!-- render 可以直接 render application資料裡面 -->
<body>
    <%= render 'navbar' %>
    <%= yield %>
    <%= render 'footer' %>
</body>
```
### `link_help`可產生 a link連結
```javascript
<%= link_to '<showname>',<path>,class:'nav-item' %>

// 可產生包裹a link
<%= link_to '<showname>',<path>,class:'nav-item' %>
    somthing
<% end %>
```
> `link_to`,`form_to`,`img_tag`都是rails的helper
```ruby
# 可傳入routes :id變量
<%= link_to store_path(store) %>
```
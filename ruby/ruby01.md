# Ruby01
## 基本輸入輸出方法
```ruby
# 顯示somthing不換行
print 'somthing'
# 顯示somthing並換行
puts 'somthing'
# gets可取得使用者的輸入，gets會 以字串傳回使用者的輸，並轉換成換行字元
number=gets
# 字串物件的chomp方法
number=gets.chomp
# 變量定義方式
number=12
# 轉string
number=12.to_s
# 轉integer
number=12.to_i
```
## `if`&&`while`
```ruby
# 隨機產生0~9
guess=gets.chomp.to_i
number=Random.rand(9)
while true
    if guess == number
        puts '正確'
        break
    elsif guess > number
        puts '太大'
    elsif guess < number
        puts '太小'
    end
end
```
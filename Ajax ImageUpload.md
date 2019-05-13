# Ajax 上傳圖片給後端

## step1 前端傳送:

>   `enctype="multipart/form-data"`必須在`form`標籤添加

```html
<form enctype="multipart/form-data">
    <input type="text" name="image_text">
    <input type="file" id="pic" name="pic" ref="pic">
</form>
```

>   `FormData` 可以用來收集表單資訊，如果有個 `form` 代表著 `<form>` 標籤的 DOM，可以直接作為 `FormData` 建構之用：

1.  用一些鍵值對來模擬一系列表單控件：即把form中所有表單元素的name與value組裝成一個queryString。
2.  異步上傳二進制文件。

```js
let formData = new FormData(form);
```

```js
let pic=document.querySelector('#pic');
let imgFile=pic.files[0];

// notes:使用new FormData()
let formData=new FormData();
formData.append('pic',imgFile)
```

```js
// axios
axios({
    method:"post",
    url:api,
    data:formData
})
.then((res)=>{
// 處理成功或失敗後的訊息
})
```

```js
// jquery.ajax
$.ajax({
    url:api,
    data:formData,
    method:"POST",
    contentType:false, // jQuery不要去設定Content-Type請求頭
    processData:false // jQuery不要去處理發送的資料
})
```



## step2 後端接收:

```php
$conn = mysqli_connect("localhost", "root", "", "demo");

if (isset($_POST['upload'])) {
    $files=$_FILES['pic'];
    $image_text = mysqli_real_escape_string($conn, $_POST['image_text']);
    // $baseUrl = "images/".basename($files['name']);

    $tmp=explode('.',$image);
    $filessub=end($tmp);

    $name=$image_text.".".$filessub;

    $sql = "INSERT INTO images (image, image_text) VALUES ('$name', '$image_text')";
    mysqli_query($conn, $sql);
    
    if (move_uploaded_file($files['tmp_name'], "images/".$name)) {
        $msg = "Image uploaded successfully";
    }else{
        $msg = "Failed to upload image";
    }
}
```


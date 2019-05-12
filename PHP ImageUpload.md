# Image Upload

## step1

> Note:form表單有無設定`enctype="multipart/form-data"`

```html
<form  method="POST"  action="imageUpload.php" enctype="multipart/form-data">
    <input type="hidden" name="size" value="1000000">
    <div>
        <input type="file" name="image">
    </div>
    <div>
        <textarea 
         id="text" 
         cols="40" 
         rows="4" 
         name="image_text" 
         placeholder="What's In Your Mind?"></textarea>
    </div>
    <div>
        <button type="submit" name="upload">Upload</button>
    </div>
</form>
```

## step2

1. `$_FILES['image']['name']`獲取檔案名稱
2. `mysqli_real_escape_string()`轉役字符串特殊字符
3. `$_FILES['image']['tmp_name']`存儲在服務器的文件的臨時副本的名稱
4. `move_uploaded_file($_FILES['image']['tmp_name'],路徑+檔名)`將上傳的文件移動到新位置，返若成功，則返回` true`，否則返回 `false`

```php
$conn = mysqli_connect("localhost", "root", "", "demo");

$msg = "";
if (isset($_POST['upload'])) {
    $image = $_FILES['image']['name'];
    $image_text = mysqli_real_escape_string($conn, $_POST['image_text']);
    $baseUrl = "images/".basename($image);

    $tmp=explode('.',$image);
    $filessub=end($tmp);

    $name=$image_text.".".$filessub;

    $sql = "INSERT INTO images (image, image_text) VALUES ('$name', '$image_text')";
    mysqli_query($conn, $sql);
    
    if (move_uploaded_file($_FILES['image']['tmp_name'], "images/".$name)) {
        $msg = "Image uploaded successfully";
    }else{
        $msg = "Failed to upload image";
    }
}
$result = mysqli_query($conn, "SELECT * FROM images");
```


# PHP

## String

### substr()

```php
$str="HEELOWORLD";
substr($str,1,4); // ELLO
```

### str_replace()

```php
$str="HEELOWORLD";
str_replace("H","Z",$str); // ZEELOWORLD
```

### strlen()

```php
$str="HEELOWORLD";
echo strlen($str); // 10
```

### str_pad(string,length,pad_string,pad_type)

*pad_type*

*   STR_PAD_BOTH - 填充字符串的兩側。如果不是偶數，則右側獲得額外的填充
*   STR_PAD_LEFT - 填充字符串的左側。
*   STR_PAD_RIGHT - 填充字符串的右側。默認。

```php
$str="HEELOWORLD";
echo str_pad($str,30,".",STR_PAD_LEFT); // ....................HEELOWORLD
```



## Array
```php
//Old array
$fruits=array(
    'apple'=>'red',
    'lemon'=>'green'
);
echo $fruits['apple'];//red

$weathers=array('sunny','cloudy','raining');
echo $weathers[0];//sunny
```
```php
$fruits=['apple'=>'red','lemon'=>'greeb'];
echo $fruits['apple'];//red

$weathers=['sunny','raining'];
echo $weathers[0];//sunny
```
### looping through array
```php
$fruits=['apple'=>'red','lemon'=>'greeb'];

echo '<ul>';
foreach($fruits as $key=>$value){
    echo "<li>$key , $value</li>";
}
echo '</ul>';
```
### modify array
```php
$fruits=['apple'=>'red','lemon'=>'greeb'];

$fruits['apple']='grren';
echo $fruits['apple'];//greeb
```
```php
//delete
unset($fruits['lemon']);
var_dump($fruits);//['apple'=>'red']

//insert
$fruits=[];
$fruits['apple']='red';
echo $fruits['apple'];//red
//insert
$fruits=[];
$fruits[]='apple';
echo $fruits[0];//apple
```
### array function()
```php
$fruits=['apple','lemon','pineapple'];
//類似JS array.join(', ');
$menu=implode(', ',$fruits);
echo 'We have '.$menu;
// We have apple, lemon,pineapple;

//類似 JS string.split(',');
$transport='car,bus,MRT,airplane';
$transport=explode(',',$transport);
var_dump($transport);
//['car','bus','MRT','airplane']
```
### sort / rsort
```php
//sort 排序 值
$fruits=['pineapple','lemon','apple'];
$fruitss=[
    'apple'=>'red',
    'lemon'=>'greeb',
    'pineapple'=>'yellow'
];
sort($fruits);
sort($fruitss);
var_dump($fruits);
//['apple','lemon','pineapple'];
var_dump($fruits);
//['greeb','red','yellow'];
```
### asort / arsort
```php
//asort 除了sort之外 還保留索引

$fruitss=[
    'apple'=>'red',
    'lemon'=>'greeb',
    'pineapple'=>'yellow'
];

var_dump($fruits);
//['lemon'=>'greeb','apple'=>'red','pineapple'=>'yellow']
```
### ksort / krsort
```php
//ksort 排序索引

$fruitss=[
    'pineapple'=>'yellow',
    'apple'=>'red',
    'lemon'=>'greeb',
];

var_dump($fruits);
//['apple'=>'red','lemon'=>'greeb','pineapple'=>'yellow']
```
### usort 可排序object字串

```php
$openlist=Array();
$openlist[0]['title']="6/10 中獎名單";
$openlist[0]['name']="6/10 獎項: Panasonic 日製手持無線吸塵器";
$openlist[0]['datetime']="2019-06-10 17:00:00";

$openlist[1]['title']="6/19 中獎名單";
$openlist[1]['name']="6/19 獎項: Panasonic 日製手持無線吸塵器";
$openlist[1]['datetime']="2019-06-19 17:00:00";

// 小到大
function sortDate($a,$b){
    return $a['datetime'] > $b['datetime']?1:-1;
}
usort($openlist1,'sortDate');
```

### array_filter

```php
function findunNum($array){
    if($array['num'] == 0){
        return true;
    }else{
        return false;
    }
}

$openlist1=array_filter($openlist,findunNum);
```



## Object

### Object
```php
// 定義架構
class Computer{
    public $cpu;
    public $ram;
    public $hd;
    public $power;

    public function trunOn(){
        $this->power=true;
    }

    public function trunOff(){
        $this->power=false;
    }
}
// 建立Object
$mac=new Computer;
```
```php
class Doggy{
    public $color;

    public function bark(){
        echo 'WoW!';
    }
    
}
$charlie=new Doggy;
$charlie->color='yellow';
$charlie->bark();
```
### constructor 預先處理事情
```php
// 定義架構
class Computer{
    public $cpu;
    public $ram;
    public $hd;
    public $power;

    //如果要預先處理事情 就得先定義__constructor
    public function __construct($cpu,$ram,$hd){
        $this->cpu=$cpu;
        $this->ram=$ram;
        $this->hd=$hd;

        $this->power=false;
    }

    public function trunOn(){
        $this->power=true;
    }

    public function trunOff(){
        $this->power=false;
    }
}
// 建立Object
$mac=new Computer;
```
### extending an object 繼承
```php
// 繼承 Computer
class ATM extends Computer{
    public $monitor;
    public $keyboard;
}
$bankATM=new ATM('1.2Ghz','2GB','2TB');
```
### example

```php
class Database{
    function __construct(){
        $this->conn=mysqli_connect(DB_HOST,DB_USER,DB_PASS);
        $this->set_db_encode();
    }

    function set_db_encode(){
        return mysqli_query($this->conn,"SET NAMES UTF8");
    }
}
```

```php
class DB extends Databaase{
    function __construct(){
        parent::__construct();
    }
    function query($sql){
        $result=mysqli_query($this->conn,$sql);
        return $result;
    }
}
```



### property &  method visibility

```php
// 定義架構
class Computer{
    private $branding;//不能接去存取，只能在同個class內使用
    protected $power;//不能接去存取，只能在同個class內使用
    public $cpu;
    public $ram;
    public $hd;

    //如果要預先處理事情 就得先定義__constructor
    public function __construct($cpu,$ram,$hd){
        $this->cpu=$cpu;
        $this->ram=$ram;
        $this->hd=$hd;

        $this->power=false;
    }

    public function trunOn(){
        $this->power=true;
    }

    public function trunOff(){
        $this->power=false;
    }
}
$mac=new Computer('1.2Ghz','2GB','2TB');
echo $mac->cpu;
echo $mac->ram;
echo $mac->hd;
```
```php
class Computer{
    //設定 private 是讓外界不要去接觸到,是透過function去處理
    private $branding;

    public function setBranding($branding){
        $this->branding=$branding.' @@!';
    }
    public function getBranding(){
        return $this->branding;
    }
}
$mac=new Computer;
$mac->setBranding('Apple');
echo $mac->getBranding();// Apple @@!
```
### private && protected
```php
class Computer{
    // private 在 extends 只會留給之前的class
    private $branding;
    // protected 在 extends 會繼承下去
    protected $power;

    public function __construct(){
        $this->trunOff();
    }

    public function trunOn(){
        $this->power=true;
    }
    public function trunOff(){
        $this->power=false;
    }
}
class ATM extends Computer{
    public $monitor;
    public $keyboard;

    public function isPowerOn(){
        return $this->power;
    }
}

$bankATM=new ATM;
$bankATM->trunOn();
echo $bankATM->isPowerOn();
```
```php
class Computer{
    private $branding;

    public function setBranding($branding){
        $this->branding=$branding.' @@!';
    }
    public function getBranding(){
        return $this->branding;
    }
}
class ATM extends Computer{
    public $monitor;
    public $keyboard;

    public function getBrandingName(){
        return $this->branding;
    }
}
$bankATM=new ATM;
$bankATM->setBranding('IBM');
echo $bankATM->getBranding();
echo $bankATM->getBrandingName();//Notice: Undefined property
```
## namespace 防止命名相同
```php
//防止命名相同
namespace Taker;
class Say{
    public function hi(){
        echo 'Hello,my friend!';
    }
}

namespace Taker\Classroom;
class Say{
    public function welcome(){
        echo 'welcome too my classroom';
    }
}

include 'Say.php';
include 'Classroom.php';

\Taker\Say::hi();
\Taker\Classroom\Say::welcome();
```
### namespace + use
```php
//防止命名相同
namespace Taker;
class Say{
    public function hi(){
        echo 'Hello,my friend!';
    }
}

namespace Taker\Classroom;
class Say{
    public function welcome(){
        echo 'welcome too my classroom';
    }
}

include 'Say.php';
include 'Classroom.php';

use \Taker\Say;

//防止function 撞名
use \Taker\Classroom\Say as Classroom;

Say::hi();
Classroom::welcome();
```
## PHP validaing
```php
if(filter_var($_POST['email'],FILTER_VALIDATE_EMAIL)){
    echo 'email ok!';
}else{
    echo 'email errror!!';
}
```
### server variables (back-end)
```php
echo $_SERVER['QUERY_STRING']; // section=1%course=20
echo $_SERVER['PATH_INFO']; // classroom
echo $_SERVER['SERVER_NAME']; // www.google.com
echo $_SERVER['DOCUMENT_ROOT']; // /user/local/htodc
echo $_SERVER['REMOTE_ADDR']; // 175.38.221.3
echo $_SERVER['HTTP_REFERER']; // https://www.google.com
echo $_SERVER['HTTP_USER_AGENT']; // Mozilla/5.0 (windows NT 5.1)......
```
## PDO (PHP Data Object) 與伺服器聯繫
```php
// connect to database program
$user='taker';
$pass="password";
$pdo=new PDO('mysql:host=localhost;dbname=myblog',$user,$pass);

// insert data

$affectedRows=$pdo->exec('
    INSERT INTO posts (author_id,category_id,title,content,post_time)
    VALUES (1,10,"10 things you should know","...","2017-10-20 17:00:00")
');

// select data

$query=$pdo->query('SELECT author_id,title FROM posts');
while($row=$query->fetch()){
    echo $row['title'].'<br>';
}
```
### 安全問題解決
```php
$user='taker';
$pass="password";
$pdo=new PDO('mysql:host=localhost;dbname=myblog',$user,$pass);

// :author_id => 預留一個值
$sql='SELECT * FROM posts WHERE author_id=:author_id';
$statement=$pdo->prepare($sql);
// 把:author_id預留的值，正式放進去
$statement->bindValue(':author_id',$_POST['author_id']);
$statement->execute();

// PDO::FETCH_ASSOC -- 關聯數組形式
while(($result=$statement->fetch(PDO::FETCH_ASSOC)) !== false){
    // htmlentites() 會把裡面的奇怪字元轉為html代碼
    echo htmlentities($result['title']).'<br>';
}
// ========================
$results=$statment->fetchAll(PDO::FETCH_ASSOC);
foreach($results as $key => $result){
    echo $result['title'];
}
```
### fetch
* fetch => 一次抓一筆資料
* fetchAll => 一次全部把資料抓出來
* PDO::FETCH_ASSOC => 抓到的資料要用array參數去呈現
## cookies & sessions 記住使用者
### cookie
```php
// cookie 一次只能存儲單一資料，並有存儲數量限制
// cookie 存儲在電腦端上，有安全性上問題
setcookie('userid','jack',time()+60*60);
echo $_COOKIE['userid'];
```
### session
```php
//自設定 session 使用時間
ini_set('session.gc_maxlifetime',3600);
// session 可以存儲多筆資輛
// session 資訊放在server
session_start();
// isset() 判斷有無值
if(!isset($_SESSION['user_id'])){
    $_SESSION['user_id']='Jack';
}
echo 'hello, '.$_SESSION['user_id'];
```
## login 實作步驟
* user 透過登入頁面填寫 username/password
    * server 收到 username/password 之後做驗證
    * 驗證成功，把username存到session裡面(登入成功)
* user要求看VIP的葉面
    * server可以透過session，取得相對應的資料給user看
* user要把電腦還朋友了，按下登出
    * 從session清除username資訊(登出)
### header already sent問題出現
> `session`&`cookie`都要放在頁面最上方，如果沒放在最上方會出現header already sent問題出現

> 
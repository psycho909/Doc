# PHP
## array
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
    public function __constructor($cpu,$ram,$hd){
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
    public function __constructor($cpu,$ram,$hd){
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
    // private 在 extends 會繼承下去
    protected $power;

    public function __constructor(){
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
# Hello World

1. 安裝JAVA

   1. https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

2. 安裝FlutterSDK

   1. https://flutter.io/docs/development/tools/sdk/archive#windows

3. 安裝Android Studio

   1. 打開Android Stuido 軟件，然後找到Plugin的配置，搜索Flutter插件
   2. https://developer.android.com/

4. 環境變數設定`D:\Flutter\flutter\bin`

5. 安裝Android證書

   `flutter doctor --android-licenses`

6. Android studio新建Flutter項目

   1. `Flutter Application`
   2. 安裝AVD虛擬機
   3. 現在需要一個虛擬機來運行我們的程序，可以點擊Android Studio中的上方菜單`tool` -`AVD Manager`選項。
   4. `Create Virtual Device.....`
   5. `Nexus 5x`

7. 讓Flutter跑起來

   1. 虛擬機運行以後，可以點擊`debug`按鈕

8. VSCode安裝Flutter插件

   1. VSCode右下角 `Flutter`按鈕可以快速啟動虛擬機
   2. `flutter create [name]`創建一個專案
   3. flutter 跟目錄下`flutter upgrade`可升級flutter版本

9. 在VSCode啟動虛擬機並輸入`flutter run`

10. 創建APK`flutter build apk`

    1. `<app dir>/build/app/outputs/apk/app-release.apk`

11. 執行後可再輸入:

    - r 鍵：點擊後熱加載，也就算是重新加載吧。
    - p 鍵：顯示網格，這個可以很好的掌握佈局情況，工作中很有用。
    - o 鍵：切換android和ios的預覽模式。
    - q 鍵：退出調試預覽模式。

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget{
  @override

  Widget build(BuildContext context){
    return MaterialApp(
      title:"Welcome to Flutter Title",
      home:Scaffold(
        appBar:AppBar(
          title:Text("Welcome to Flutter Bar")
        ),
        body: Center(
          child:Text("Hello World Context")
        ),
      ),
    );
  }
}
```

## StatefulWidget和StatelessWidget

- StatefulWidget ： 具有可變狀態的窗口部件，也就是你在使用應用的時候就可以隨時變化，比如我們常見的進度條，隨著進度不斷變化。
- StatelessWidget：不可變狀態窗口部件，也就是你在使用時不可以改變，比如固定的文字（寫上後就在那裡了，死也不會變了）。

# Text Widget

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget{
  @override

  Widget build(BuildContext context){
    return MaterialApp(
      title:"Welcome to Flutter Title",
      home:Scaffold(
        body: Center(
          child:Text(
            "Hello World Context,It good it good,React,Hello World Context,It good it good,React",
            // 多行文字對其
            textAlign: TextAlign.start,
            // 文字顯示幾行
            maxLines: 1,
            // 文字溢出顯示 ...
            overflow: TextOverflow.ellipsis,
            // 文字樣式
            style:TextStyle(
              fontSize: 25.0,
              color:Color.fromARGB(255, 255, 125, 125),
              // 下滑線
              decoration: TextDecoration.underline,
              decorationStyle: TextDecorationStyle.solid,
            ),
          ),
        ),
      ),
    );
  }
}
```

# Container

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget{
  @override

  Widget build(BuildContext context){
    return MaterialApp(
      title:"Welcome to Flutter Title",
      home:Scaffold(
        body: Center(
          child:Container(
            child:new Image.network(
              'https://fm.cnbc.com/applications/cnbc.com/resources/img/editorial/2018/12/21/105642793-1545408067309gettyimages-1074484904.1910x1000.jpeg',
              color:Colors.greenAccent,
              colorBlendMode: BlendMode.lighten,
              repeat: ImageRepeat.repeat,
            ),
            width: 300.0,
            height:200.0,
            color:Colors.redAccent
          )
        ),
      ),
    );
  }
}
```

# Image

```dart
import 'package:flutter/material.dart';
void main () => runApp(MyApp());
class MyApp extends StatelessWidget{
  @override
  Widget build(BuildContext context ){
      return MaterialApp(
        title:'Text widget',
        home:Scaffold(
          body:Center(
          child:Container(
            child:new Image.network(
              'http://jspang.com/static/myimg/blogtouxiang.jpg',
              scale:1.0,
            ),
            width:300.0,
            height:200.0,
            color: Colors.lightBlue,
          ),
          ),
        ),
      );
  }
}
```

## fit屬性的設置

1. **BoxFit.fill**:全圖顯示，圖片會被拉伸，並充滿父容器。
2. **BoxFit.contain**:全图显示，显示原比例，可能会有空隙。
3. **BoxFit.cover**：显示可能拉伸，可能裁切，充满（图片要充满整个容器，还不变形）。

## 圖片的混合模式

1. color：是要混合的顏色，如果你只設置color是沒有意義的。
2. colorBlendMode:是混合模式，相當於我們如何混合。

```dart
child:new Image.network(
  'http://jspang.com/static/myimg/blogtouxiang.jpg',
    color: Colors.greenAccent,
    colorBlendMode: BlendMode.darken,
),
```

## repeat圖片重複

- ImageRepeat.repeat : 橫向和縱向都進行重複，直到鋪滿整個畫布。
- ImageRepeat.repeatX: 橫向重複，縱向不重複。
- ImageRepeat.repeatY：縱向重複，橫向不重複。

```dart
child:new Image.network(
  'http://jspang.com/static/myimg/blogtouxiang.jpg',
   repeat: ImageRepeat.repeat,
),
```



# ListView

```dart
import "package:flutter/material.dart";

void main()=> runApp(MyApp());

class MyApp extends StatelessWidget{
  @override

  Widget build(BuildContext context){
    return MaterialApp(
      title:"DEMO",
      home:Scaffold(
        appBar: new AppBar(title:new Text("ListView Widget")),
        body:Center(
          child: Container(
            height:200.0,
            child:MyList(),
          ),
        )
      ),
    );
  }
}

class MyList extends StatelessWidget{
  @override

  Widget build(BuildContext context){
    return ListView(
      scrollDirection: Axis.horizontal,
      children: <Widget>[
        new Container(
          width:180.0,
          color:Colors.greenAccent,
        ),
        new Container(
          width:180.0,
          color:Colors.blueAccent,
        ),
        new Container(
          width:180.0,
          color:Colors.yellowAccent,
        ),
        new Container(
          width:180.0,
          color:Colors.deepOrange,
        ),
      ],
    );
  }
}
```

## new ListView.builder()動態列表

```dart
import "package:flutter/material.dart";

void main()=> runApp(MyApp(
  // 從MyApp傳遞進去
  // new List<String> 數組<String類型>
  items:List<String>.generate(1000, (i)=>"Item $i")
));

class MyApp extends StatelessWidget{
  // 接收參數
  final List<String> items;
  // 構造函數
  // 必須有個 Key值
  // @required 必需的要傳的參數
  // :super(key:key) 調用父類
  MyApp({Key key,@required this.items}):super(key:key);

  @override
  Widget build(BuildContext context){
    return MaterialApp(
      title:"DEMO",
      home:Scaffold(
        appBar: new AppBar(title:new Text("ListView Widget")),
        body:Center(
          // new ListView.builder() 生成動態列表
          child:ListView.builder(
            // itemCount:生成長度
            itemCount: items.length,
            // itemBuilder:(context,index){}
            itemBuilder: (context,index){
              return new ListTile(
                title:new Text('${items[index]}')
              );
            },
          ),
        )
      ),
    );
  }
}

```

## ListTile

```dart
ListTile(
	title:Text("文字")
)
```



# GridView

```dart
import 'package:flutter/material.dart';
void main () => runApp(MyApp());
class MyApp extends StatelessWidget{
  @override
  Widget build(BuildContext context ){
      return MaterialApp(
        title:'ListView widget',
        home:Scaffold(
          body:GridView.count(
            padding:const EdgeInsets.all(20.0),
            crossAxisSpacing: 10.0,
            crossAxisCount: 3,
            children: <Widget>[
              const Text('I am Jspang'),
              const Text('I love Web'),
              const Text('jspang.com'),
              const Text('我喜欢玩游戏'),
              const Text('我喜欢看书'),
              const Text('我喜欢吃火锅')
            ],
          )
        ),
      );
  }
}
```

```js
import "package:flutter/material.dart";

void main() => runApp(MyApp());

class MyApp extends StatelessWidget{
  @override

  Widget build(BuildContext context){
    return MaterialApp(
      title:"Grid Demo",
      home:Scaffold(
        appBar: new AppBar(title:new Text("Grid Demo")),
        body:GridView(
          gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
            crossAxisCount: 3,
            mainAxisSpacing: 2.0,
            crossAxisSpacing: 2.0,
            childAspectRatio: 0.7
          ),
          children: <Widget>[
            new Image.network("https://p2.bahamut.com.tw/S/2KU/71/2057c1e690f9df20a52c8d45c113w5z5.PNG",fit:BoxFit.cover),
            new Image.network("https://p2.bahamut.com.tw/S/2KU/38/2cbaca77f19f5b6374af187dd413woi5.JPG",fit:BoxFit.cover),
            new Image.network("https://p2.bahamut.com.tw/S/2KU/32/4a40fa6cc0d05f41ce8c00f83a13woc5.JPG",fit:BoxFit.cover),
            new Image.network("https://p2.bahamut.com.tw/S/2KU/22/5ba3b8f6e1a2f1f599dd87602613w7e5.JPG",fit:BoxFit.cover),
            new Image.network("https://p2.bahamut.com.tw/S/2KU/95/1a3a54bf1a568d646bcff0088d13wnb5.JPG",fit:BoxFit.cover),
            new Image.network("https://p2.bahamut.com.tw/S/2KU/86/30334c32da894503fa2932ac9e13wn25.PNG",fit:BoxFit.cover),
          ],
        )
      )
    );
  }
}
```

# RowWidget布局

```dart
body:new Row(
    children: <Widget>[
        new RaisedButton(
            onPressed: (){},
            color:Colors.greenAccent,
            child: new Text("green"),
        ),
        Expanded(
            child:new RaisedButton(
                onPressed: (){},
                color:Colors.redAccent,
                child:new Text("red")
            )
        ),
        Expanded(
            child:new RaisedButton(
                onPressed: (){},
                color:Colors.blueAccent,
                child:new Text("blue")
            )
        )
    ],
)
```

# Column布局

```dart
body:Center(
    child: new Column(
        crossAxisAlignment: CrossAxisAlignment.center,
        mainAxisAlignment: MainAxisAlignment.center,
        children: <Widget>[
            Text("I am Chen"),
            Expanded(
                child: Text("I am Website is Chen"),
            ),
            Text("I am Psychosocial909@gmail.com"),
        ],
    ),
)
```

# StackWidget布局

```dart
    var stack=new Stack(
      // 對其屬性，只適用於兩個物品重疊使用
      // FractionalOffset(x,y) 0~1
      alignment: const FractionalOffset(0.5, 0.8),
      children: <Widget>[
        // 相當於圓形頭像
        new CircleAvatar(
          backgroundImage: new NetworkImage("https://p2.bahamut.com.tw/S/2KU/00/e8dab39f68f89b1dabd471dc1c13wyk5.JPG"),
          radius: 100.0,
        ),
        new Container(
          decoration: new BoxDecoration(
            color:Colors.lightBlue
          ),
          padding: const EdgeInsets.all(5.0),
          child: Text("董卓",style: TextStyle(fontSize: 24.0),),
        )
      ],
    );
    return MaterialApp(
      title:"Hello World",
      home:Scaffold(
        appBar: new AppBar(
          title:new Text("垂直方向布局")
        ),
        body:Center(
          child: stack,
        )
      )
    );
```

## 對其方式2

```dart
var stack=new Stack(
    // 對其屬性，只適用於兩個物品重疊使用
    // FractionalOffset(x,y) 0~1
    alignment: const FractionalOffset(0.5, 0.8),
    children: <Widget>[
        // 相當於圓形頭像
        new CircleAvatar(
            backgroundImage: new NetworkImage("https://p2.bahamut.com.tw/S/2KU/00/e8dab39f68f89b1dabd471dc1c13wyk5.JPG"),
            radius: 100.0,
        ),
        new Positioned(
            top:10.0,
            left:10.0,
            child: Text("董卓"),
        ),
        new Positioned(
            bottom:10.0,
            right:10.0,
            child: Text("董軍"),
        )
    ],
);
```

# CardWidget 卡片布局

```dart
import "package:flutter/material.dart";

void main() => runApp(MyApp());

class MyApp extends StatelessWidget{
  @override

  Widget build(BuildContext context){
    var card=new Card(
      child: Column(
        children: <Widget>[
          ListTile(
            title:Text(
              "《靈魂之聲 Online》台港澳代理權確定 預計近期上線、展開一場諸神與巨人的戰爭！",
              style: TextStyle(
                fontWeight:FontWeight.w800 
              ),
            ),
            subtitle: Text("紅心辣椒今（11）日宣布，取得由韓國研發商 Blue Potion Games 開發的北歐奇幻線上遊戲《靈魂之聲 Online》台港澳代理權，預計近期上市。"),
            leading: new Icon(Icons.account_box,color:Colors.lightBlue),
          ),
          // 分隔線
          new Divider(),
          ListTile(
            title:Text(
              "《靈魂之聲 Online》台港澳代理權確定 預計近期上線、展開一場諸神與巨人的戰爭！",
              style: TextStyle(
                fontWeight:FontWeight.w800 
              ),
            ),
            subtitle: Text("紅心辣椒今（11）日宣布，取得由韓國研發商 Blue Potion Games 開發的北歐奇幻線上遊戲《靈魂之聲 Online》台港澳代理權，預計近期上市。"),
            leading: new Icon(Icons.account_box,color:Colors.lightBlue),
          ),
          new Divider(),
          ListTile(
            title:Text(
              "《靈魂之聲 Online》台港澳代理權確定 預計近期上線、展開一場諸神與巨人的戰爭！",
              style: TextStyle(
                fontWeight:FontWeight.w800 
              ),
            ),
            subtitle: Text("紅心辣椒今（11）日宣布，取得由韓國研發商 Blue Potion Games 開發的北歐奇幻線上遊戲《靈魂之聲 Online》台港澳代理權，預計近期上市。"),
            leading: new Icon(Icons.account_box,color:Colors.lightBlue),
          ),
          new Divider(),
        ],
      ),
    );
    return MaterialApp(
      title:"Hello World",
      home:Scaffold(
        appBar: new AppBar(
          title:new Text("垂直方向布局")
        ),
        body:Center(
          child: card,
        )
      )
    );
  }
}
```

# 導航Page

```dart
import "package:flutter/material.dart";

void main(){
  runApp(MaterialApp(
    title:"導航",
    home:new FirstScreen()
  ));
}

class FirstScreen extends StatelessWidget{
  @override

  Widget build(BuildContext context) {
    // TODO: implement build
    return Scaffold(
      appBar: new AppBar(
        title:Text('導航頁面')
      ),
      body:Center(
        child:RaisedButton(
            child: Text("Check Item info"),
            onPressed: (){
              Navigator.push(context,MaterialPageRoute(
                  builder: (context)=>new SecondScreen()
              ));
            },
        )
      )
    );
  }
}

class SecondScreen extends StatelessWidget{
  @override

  Widget build(BuildContext context) {
    // TODO: implement build
    return Scaffold(
      appBar: AppBar(
        title:Text("Itme info page")
      ),
      body: Center(
        child: RaisedButton(
            child: Text("Back"),
            onPressed: (){
              Navigator.pop(context);
            }
        ),
      ),
    );
  }
}
```

# 導航參數傳遞和接收

```dart
import "package:flutter/material.dart";

class Products{
  final String title; // 商品名稱
  final String description; // 商品描述
  Products(this.title,this.description);
}

void main(){
  runApp(MaterialApp(
    title:"Nav data",
    home:ProductList(
      products:List.generate(20, (i)=>Products('商品名稱 $i',"商品描述 $i"))
    )
  ));
}

class ProductList extends StatelessWidget {
  // 參數接收
  // 定義List變量<Product 類>
  final List<Products> products;
  ProductList({Key key,@required this.products}):super(key:key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title:Text("Item List")
      ),
      body:ListView.builder(
        itemCount: products.length,
        itemBuilder: (context,index){
          return ListTile(
            title:Text("${products[index].title} @ ${products[index].description}"),
            onTap:(){
              Navigator.push(context,
              MaterialPageRoute(
                builder: (context)=>ProductDetail(products:products[index])
              ));
            }
          );
        },
      )
    );
  }
}

class ProductDetail extends StatelessWidget {
  final Products products;
  ProductDetail({Key key,@required this.products}):super(key:key);

  @override

  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title:Text("${products.title}")
      ),
      body:Center(
        child: Text("${products.description}"),
      )
    );
  }
}
```

頁面跳轉並返回數據

```dart
import "package:flutter/material.dart";

void main(){
  runApp(MaterialApp(
    title:"Page 跳轉返回數據",
    home:FirstPage()
  ));
}

class FirstPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title:Text("找~~~")
      ),
      body:Center(
        child: RouteButton(),
      )
    );
  }
}

class RouteButton extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return RaisedButton(
      child: Text("去找~"),
      onPressed: (){_navigateToXiaoJieJie(context);},
    );
  }

  _navigateToXiaoJieJie(BuildContext context) async{
    final result=await Navigator.push(
      context,
      MaterialPageRoute(builder: (context)=>XiaoJieJie()));

      Scaffold.of(context).showSnackBar(SnackBar(content: Text("$result")));
  }
}

class XiaoJieJie extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title:Text("Find~~")
      ),
      body:Center(
        child: Column(
          children: <Widget>[
            RaisedButton(
              child: Text("第一個JieJie"),
              onPressed: (){
                Navigator.pop(context,"第一個JieJie 15616");
              },
            ),
            RaisedButton(
              child: Text("第二個JieJie"),
              onPressed: (){
                Navigator.pop(context,"第二個JieJie 3213123");
              },
            )
          ],
        ),
      )
    );
  }
}
```

# 底部導航Bar

```dart
import 'package:flutter/material.dart';
import 'pages/home_screen.dart';
import 'pages/pages_screen.dart';
import 'pages/email_screen.dart';
import 'pages/airplay_screen.dart';


class BottomNavgationWidget extends StatefulWidget {
  _BottomNavgationWidgetState createState() => _BottomNavgationWidgetState();
}

class _BottomNavgationWidgetState extends State<BottomNavgationWidget> {
  final _BottomNavgationColor=Colors.blue;
  int _currentIndex=0;
  // List 類似JS的Array
  List<Widget> list=List();
	
  // 初始化 重寫
  @override
  void initState(){
    list
      ..add(HomeScreen())
      ..add(EmailScreen())
      ..add(PagesScreen())
      ..add(AirplayScreen());
    super.initState();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body:list[_currentIndex],
      /*
      bottomNavigationBar:BottomNavigationBar(
      	// 數組
        items:[
        	BottomNavigationBarItem(
        		icon:Icon(
        			Icons.home,
        			color:Colors.blue
        		),
        		title:Text(
        			"Home",
        			style:TextStyle(color:Colors.blue)
        		)
        	)
        ],
      	// 哪個被點擊 就顯示哪個
      	currentIndex:_currentIndex,
      	// 響應事件
        onTap:(int index){
        	setState(() {
                _currentIndex=index;
            });
        }
      )
      */
      bottomNavigationBar: BottomNavigationBar(
        items:[
          BottomNavigationBarItem(
            icon: Icon(
              Icons.home,
              color:_BottomNavgationColor
            ),
            title:Text(
              "Home",
              style:TextStyle(color:_BottomNavgationColor)
            )
          ),
          BottomNavigationBarItem(
            icon: Icon(
              Icons.email,
              color:_BottomNavgationColor
            ),
            title:Text(
              "Email",
              style:TextStyle(color:_BottomNavgationColor)
            )
          ),
          BottomNavigationBarItem(
            icon: Icon(
              Icons.pages,
              color:_BottomNavgationColor
            ),
            title:Text(
              "Pages",
              style:TextStyle(color:_BottomNavgationColor)
            )
          ),
          BottomNavigationBarItem(
            icon: Icon(
              Icons.airplay,
              color:_BottomNavgationColor
            ),
            title:Text(
              "Airplay",
              style:TextStyle(color:_BottomNavgationColor)
            )
          )
        ],
		currentIndex: _currentIndex,
        onTap:(int index){
          setState(() {
            _currentIndex=index;
          });
        }
      ),
    );
  }
}

```

# 懸浮按鈕`floatingActionButton`

```dart
// each_view.dart

import "package:flutter/material.dart";

class EachView extends StatefulWidget {
  String _title;
  EachView(this._title);
  @override
  _EachViewState createState() => _EachViewState();
}

class _EachViewState extends State<EachView> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title:Text(widget._title)),
      body:Center(
        child: Text(widget._title),
      )
    );
  }
}
```



```dart
import "package:flutter/material.dart";
import "each_view.dart";

class BottomAppBarDemo extends StatefulWidget {
  final Widget child;

  BottomAppBarDemo({Key key, this.child}) : super(key: key);

  _BottomAppBarDemoState createState() => _BottomAppBarDemoState();
}

class _BottomAppBarDemoState extends State<BottomAppBarDemo> {
  List<Widget> _eachView;
  int _index=0;
  // 初始化
  @override
  void initState() {
    _eachView=List();
    _eachView
      ..add(EachView("Home"))
      ..add(EachView("JSPang"));

    super.initState();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body:_eachView[_index],
      // 可交互浮動按鈕
      floatingActionButton: FloatingActionButton(
        onPressed: (){
          // 點按鈕 生成一個路由
          Navigator.push(context,
            MaterialPageRoute(
              builder: (BuildContext context){
                return EachView("New Page");
              }
            )
          );
        },
        tooltip: "JSPang",
        child:Icon(
          Icons.add,
          color:Colors.white
        )
      ),
      // 與底部工具欄 融合
      floatingActionButtonLocation: FloatingActionButtonLocation.centerDocked,
      // 底部工具欄
      bottomNavigationBar: BottomAppBar(
        color:Colors.lightBlue,
        // 底部导航栏打一个圆形的洞
        shape:CircularNotchedRectangle(),
        child:Row(
          //mainAxisSize，可压缩或伸长行内控件的间距
          mainAxisSize: MainAxisSize.max,
          //对齐方式
          mainAxisAlignment: MainAxisAlignment.spaceAround,
          children: <Widget>[
            IconButton(
              icon:Icon(
                  Icons.home,
                  color:Colors.greenAccent,
              ),
              onPressed: (){
                setState(() {
                  _index=0;
                });
              },
            ),
            IconButton(
              icon:Icon(
                  Icons.airline_seat_flat,
                  color:Colors.greenAccent,
              ),
              onPressed: (){
                setState(() {
                  _index=1;
                });
              },
            )
          ],
        )
      ),
    );
  }
}
```

# 路由換頁動畫

```dart
import "package:flutter/material.dart";

class CustomRoute extends PageRouteBuilder{
  final Widget widget;

  CustomRoute(this.widget)
    // 調用父類方法
    :super(
      // 設置過渡時間
      transitionDuration:Duration(seconds: 1),
      // 重寫頁面構造氣
      // 接收3個參數
      pageBuilder:(
        // 1.上下文
        BuildContext context,
        Animation<double> animation1,
        Animation<double> animation2,
      ){
        return widget;
      },
      // 過去效果 接收4個參數
      transitionsBuilder:(
        BuildContext context,
        Animation<double> animation1,
        Animation<double> animation2,
        Widget child
      ){
        // 漸隱見線的路由動畫效果
        // return FadeTransition(
        //   opacity:Tween(begin:0.0,end:1.0)
        //   .animate(CurvedAnimation(
        //     parent:animation1,
        //     curve: Curves.fastLinearToSlowEaseIn
        //   )),
        //   child: child,
        // );

        // 縮放的動畫效果
        // return ScaleTransition(
        //   scale:Tween(begin:0.0,end:1.0).animate(CurvedAnimation(
        //     parent:animation1,
        //     curve: Curves.fastLinearToSlowEaseIn
        //   )),
        //   child: child,
        // );

        // 旋轉+縮放動畫效果
        // return RotationTransition(
        //   turns: Tween(begin: 0.0,end: 1.0)
        //   .animate(CurvedAnimation(
        //     parent: animation1,
        //     curve: Curves.fastLinearToSlowEaseIn
        //   )),
        //   child:ScaleTransition(
        //     scale:Tween(begin:0.0,end:1.0).animate(CurvedAnimation(
        //       parent: animation1,
        //       curve: Curves.fastLinearToSlowEaseIn
        //     )),
        //     child:child
        //   )
        // );

        // 左右滑動動畫
        return SlideTransition(
          position: Tween<Offset>(
            begin: Offset(-1.0, 0.0),
            end:Offset(0.0, 0.0)
          ).animate(CurvedAnimation(
            parent: animation1,
            curve: Curves.fastLinearToSlowEaseIn
          )),
          child:child
        );
      }
    );
}
```

```dart
import "package:flutter/material.dart";
import "custom_router.dart";

class FirstPage extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.blue,
      appBar: AppBar(
        title:Text("FirstPage",style: TextStyle(fontSize: 36.0)),
        elevation: 4.0
      ),
      body:Center(
        child: MaterialButton(
          child:Icon(
            Icons.navigate_next,
            color:Colors.white,
            size:64.0
          ),
          onPressed: (){
            Navigator.of(context).push(
              CustomRoute(SecondPage())
            );
          },
        ),
      )
    );
  }
}

class SecondPage extends StatelessWidget{
  @override

  Widget build(BuildContext context){
    return Scaffold(
      backgroundColor: Colors.pinkAccent,
      appBar: AppBar(
        title:Text("SecondPage",style: TextStyle(fontSize: 36.0)),
        elevation: 4.0,
        backgroundColor: Colors.pinkAccent,
        leading: Container()
      ),
      body:Center(
        child: MaterialButton(
          child:Icon(
            Icons.navigate_before,
            color:Colors.white,
            size:64.0
          ),
          onPressed: (){
            Navigator.of(context).pop();
          },
        ),
      )
      
    );
  }
}
```

# 毛玻璃效果

```dart
import "package:flutter/material.dart";
// 圖片過濾器
import 'dart:ui';

class FrostedGlassDemo extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      // 層疊效果
      body:Stack(
        children: <Widget>[
          // 約束盒子組件,添加額外的約束條件
          ConstrainedBox(
            constraints: const BoxConstraints.expand(),
            child: Image.network('https://p2.bahamut.com.tw/M/2KU/81/80d8f81da0010dd06468a3355f144cp5.JPG'),
          ),
          Center(
            // 可裁切的矩形
            child:ClipRect(
              // 背景過濾器
              child: BackdropFilter(
                filter: ImageFilter.blur(sigmaX: 5.0,sigmaY: 5.0),
                child: Opacity(
                  opacity: 0.5,
                  child: Container(
                    width: 500.0,
                    height:700.0,
                    // 盒子修飾器
                    decoration: BoxDecoration(color:Colors.grey.shade200),
                    child: Center(
                      child: Text(
                        "JSPang",
                        style:Theme.of(context).textTheme.display3  
                      ),
                    ),
                  ),
                ),
              ),
            )
          ),
        ],
      )
    );
  }
}  
```


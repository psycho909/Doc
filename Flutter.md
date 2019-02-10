# Hello World

1. 安裝JAVA

2. 安裝FlutterSDK

3. 安裝Android Studio

   1. 打開Android Stuido 軟件，然後找到Plugin的配置，搜索Flutter插件

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

## 動態列表

```dart
import "package:flutter/material.dart";

void main()=> runApp(MyApp(
  items:List<String>.generate(1000, (i)=>"Item $i")
));

class MyApp extends StatelessWidget{
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
          child:new ListView.builder(
            itemCount: items.length,
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
import "package:flutter/material.dart";

void main() => runApp(MyApp());

class MyApp extends StatelessWidget{
  @override

  Widget build(BuildContext context){
    return MaterialApp(
      title:"Hello World",
      home:Scaffold(
        appBar: new AppBar(
          title:new Text("水平方向布局")
        ),
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
      )
    );
  }
}
```


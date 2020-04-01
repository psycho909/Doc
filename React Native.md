## navigation

## 安裝功能

### 先安裝第三方

```js
npm install @react-navigation/native
```

### 如果使用expo，還需要再安裝

```js
expo install react-native-gesture-handler react-native-reanimated react-native-screens react-native-safe-area-context @react-native-community/masked-view
```

### 使用Tab navigation換頁

```js
npm install @react-navigation/bottom-tabs
```

### 使用nav bar換頁，推疊效果

```js
npm install @react-navigation/stack
```

### 使用icon

```js
npm install --save react-native-vector-icons
```


## 使用bottom-tabs - tab功能

https://reactnavigation.org/docs/tab-based-navigation

### App.js

```js
import React from 'react'
import {StyleSheet, Text, View,Image} from 'react-native'
import {NavigationContainer} from '@react-navigation/native'
import {createBottomTabNavigator} from '@react-navigation/bottom-tabs'
import { Ionicons } from '@expo/vector-icons';
import HomeScreen from './src/sreens/HomeScreen'
import ProfileScreen from './src/sreens/ProfileScreen'
```
```react
const Tab = createBottomTabNavigator();
<NavigationContainer>
<Tab.Navigator
	initialRouteName="Home"
    screenOptions={({route})=>({
        tabBarIcon:({color,focused}=>{
            let iconName;
            if(route.name == "Home"){
             	iconName=focused?"ios-trophy":"ios-information-circle-outline";
                return <Image 
                        source={{uri:'http://pic101.huitu.com/res/20171031/109077_20171031101332140018_1.jpg'}}
                        style={{width:30,height:30}} />
             }else if(route.name == "Settings"){
				iconName=focused?"ios-options":"ios-list"
             }
             return <Ionicons name={iconName} size={25} color={color} />
        })
    })}
    tabBarOptions={{
        activeTintColor: 'tomato',
        inactiveTintColor: 'gray',
    }}
>
    <Tab.Screen name="Home" component={HomeScreen} />
    <Tab.Screen name="Settings" component={ProfileScreen} />
</Tab.Navigator>
</NavigationContainer>
```

### Tab頁面

```react
import React from 'react'
import {StyleSheet, Text, View} from 'react-native'

export default function ProfileScreen() {
    return (
        <View style={styles.container}>
            <Text style={styles.textStyle}>
                ProfileScreen app!
            </Text>
        </View>
    )
}

const styles = StyleSheet.create({
    container: {
        flex: 1,
        backgroundColor: '#fff',
        alignItems: 'center',
        justifyContent: 'center'
    },
    textStyle: {
        color: 'red'
    }
})

```



## 使用stack - nav bar功能

https://reactnavigation.org/docs/stack-navigator/

### App.js

```js
import React from 'react'
import {StyleSheet, Text, View,Image} from 'react-native'
import {NavigationContainer} from '@react-navigation/native'
import { createStackNavigator } from '@react-navigation/stack';
import HomeScreen from './src/sreens/HomeScreen'
import HomeDetailScreen from './src/sreens/HomeDetailScreen'

```
```react
const Stack = createStackNavigator();

<NavigationContainer>
	<Stack.Navigator
        initialRouteName="Home"
        screenOptions={{
          headerStyle:{backgroundColor:"tomato"},
          headerBackTitle:"返回",
          headerTintColor:"#fff"
    	}}
    >
    	<Stack.Screen name="Home" component={HomeScreen} />
    	<Stack.Screen name="HomeDetailScreen" component={HomeDetailScreen} options=		{{title:"My Detail"}} />
	</Stack.Navigator>
</NavigationContainer>
```

### Tab頁面

```react
import React from 'react'
import {StyleSheet, Text, View,Button} from 'react-native'

export default function HomeScreen(props) {
    return (
        <View style={styles.container}>
            <Text style={styles.textStyle}>HomeScreen app!</Text>
            <Button
            title="go to next page"
            // push(name) 跳頁
            // pop() 返回
            onPress={()=>props.navigation.push("HomeDetailScreen")}
            />
        </View>
    )
}

const styles = StyleSheet.create({
    container: {
        flex: 1,
        backgroundColor: '#fff',
        alignItems: 'center',
        justifyContent: 'center'
    },
    textStyle: {
        color: 'red'
    }
})
```

## 同時有 tab & nav bar

createBottomTabNavigator

Home ----------------------------  Profile

createStackNavigator

HomeMain ---------------------- Detail

createStackNavigator

ProfileMain ---------------------- Detail

```react
import React from 'react'
import {StyleSheet, Text, View, Image} from 'react-native'
import {NavigationContainer} from '@react-navigation/native'
import {createBottomTabNavigator} from '@react-navigation/bottom-tabs'
import {createStackNavigator} from '@react-navigation/stack'
import {Ionicons} from '@expo/vector-icons'
import HomeScreen from './src/sreens/HomeScreen'
import ProfileScreen from './src/sreens/ProfileScreen'
import HomeDetailScreen from './src/sreens/HomeDetailScreen'
import ProfileDetaiScreen from './src/sreens/ProfileDetaiScreen'
```

```react
const Tab = createBottomTabNavigator()
const Stack = createStackNavigator()
```

### 創建Home的Stack & Profile的Stack

```react
function MyHomeStack() {
    return (
        <Stack.Navigator
            initialRouteName="Home"
            screenOptions={{
                headerStyle: {backgroundColor: 'tomato'},
                headerBackTitle: '返回',
                headerTintColor: '#fff'
            }}
        >
            <Stack.Screen
                name="Home"
                component={HomeScreen}
                options={{title: 'HOME'}}
            />
            <Stack.Screen
                name="HomeDetailScreen"
                component={HomeDetailScreen}
                options={{title: 'My Detail'}}
            />
        </Stack.Navigator>
    )
}

function MyProfileStack() {
    return (
        <Stack.Navigator
            initialRouteName="ProfileScreen"
            screenOptions={{
                headerStyle: {backgroundColor: 'tomato'},
                headerBackTitle: '返回2',
                headerTintColor: '#fff'
            }}
        >
            <Stack.Screen
                name="Profile"
                component={ProfileScreen}
                options={{title: 'ProfileScreen'}}
            />
            <Stack.Screen
                name="ProfileDetaiScreen"
                component={ProfileDetaiScreen}
                options={{title: 'My Detail2'}}
            />
        </Stack.Navigator>
    )
}
```

```react
<NavigationContainer>
	<Tab.Navigator
		initialRouteName="Home"
		screenOptions={({route}) => ({
                    tabBarIcon: ({color, focused}) => {
                        let iconName
                        if (route.name == 'Home') {
                            // iconName=focused?"ios-trophy":"ios-information-circle-outline";
                            return (
                                <Image
                                    source={{
                                        uri:
                                            'http://pic101.huitu.com/res/20171031/109077_20171031101332140018_1.jpg'
                                    }}
                                    style={{width: 30, height: 30}}
                                />
                            )
                        } else if (route.name == 'Settings') {
                            iconName = focused ? 'ios-options' : 'ios-list'
                        }
                        return (
                            <Ionicons name={iconName} size={25} color={color} />
                        )
                    }
                })}
                tabBarOptions={{
                    activeTintColor: 'tomato',
                    inactiveTintColor: 'gray'
                }}
            >
                <Tab.Screen name="Home" component={MyHomeStack} />
                <Tab.Screen name="Settings" component={MyProfileStack} />
	</Tab.Navigator>
</NavigationContainer>
```

### 不同頁面間的傳值

#### 主頁傳到內頁

HomeScreen -> HomeDetailScreen

HomeScreen.js

```react
<View style={styles.container}>
    <Text style={styles.textStyle}>HomeScreen app!</Text>
    <Button
        title="go to next page"
        onPress={()=>props.navigation.push("HomeDetailScreen",{name:"avon"})}
        />
</View>
```

HomeDetailScreen.js

```react
<View style={styles.container}>
    <Text style={styles.textStyle}>HomeDetailScreen app!</Text>
    <Button
        title="go back"
        onPress={()=>props.navigation.pop()}
        />
    <Text>{props.route.params.name || "default name"}</Text>
</View>
```

#### 內頁傳到主頁

HomeScreen.js

```react
const [food,setFood]=useState("candy")
const changeFood=(foodGet)=>{
    setFood(foodGet)
}

<View style={styles.container}>
    <Text style={styles.textStyle}>HomeScreen app!</Text>
    <Text>{food}</Text>
    <Button
        title="go to next page"
        onPress={()=>props.navigation.push("HomeDetailScreen",{name:"avon",functionA:(arg)=>changeFood(arg)})}
        />
</View>
```

HomeDetailScreen.js

```react
<View style={styles.container}>
    <Text style={styles.textStyle}>HomeDetailScreen app!</Text>
    <Button
        title="go back"
        onPress={()=>props.navigation.pop()}
        />
    <Text>{props.route.params.name || "default name"}</Text>
    <Button
    title="change food"
    onPress={()=>props.route.params.functionA("Noodle")}
    />
</View>
```

## FlatList列表

http://reactnative.dev/docs/flatlist

```react
const DATA = [
  {
    id: 'bd7acbea-c1b1-46c2-aed5-3ad53abb28ba',
    title: 'First Item',
  },
  {
    id: '3ac68afc-c605-48d3-a4f8-fbd91aa97f63',
    title: 'Second Item',
  },
  {
    id: '58694a0f-3da1-471f-bd96-145571e29d72',
    title: 'Third Item',
  },
];

function Item({ title }) {
  return (
    <View style={styles.item}>
      <Text style={styles.title}>{title}</Text>
    </View>
  );
}

<FlatList 
data={DATA} 
renderItem={({item})=><Item title={item.title} />} 
keyExtractor={item=>item.id}
/>
```

* data:列表的數據來源
* renderItem:從數據源中逐個去解這個data數據，然後再返回一個設定好格式的組件來做渲染
* keyExtractor:用來給每一個item，生成一個不重複的key或者id

### 範例使用

```react
import React,{useState,useEffect} from 'react'
import {StyleSheet, Text, View,FlatList,TouchableOpacity,Image} from 'react-native'
```

```json
var MOCKED_DATA=[
    {
        id:"1",
        note:"恭喜你!達成環島100次",
        date:"2020/01/28 14:00"
    },{
        id:"2",
        note:"你的會員身分認證",
        date:"2020/01/29 14:00"
    },{
        id:"3",
        note:"健身時間",
        date:"2020/01/30 14:00"
    }
]
```

```react
export default function HomeScreen(props) {
    const [dataSource,setDataSource]=useState([])

    useEffect(()=>{
        var book=MOCKED_DATA
        setDataSource(book)
    })
    
    const showNoticeDetail = (cases)=>{
        props.navigation.push("HomeDetailScreen",{passProps:cases})
    }
    const renderBook=(cases)=>{
        return (
            <TouchableOpacity onPress={()=>showNoticeDetail(cases)}>
                <View>
                    <View style={styles.MainView}>
                        {/* <Image/> */}
                        <View style={{flex:1}}>
                            <Text ellipsizeMode="tail" numberOfLines={3} style={{color:"black",fontSize:15,marginTop:8}}>
                                {cases.note}
                            </Text>
                            <Text ellipsizeMode="tail" numberOfLines={3} style={{fontSize:13,marginTop:8,marginBottom:8}}>
                                {cases.date}
                            </Text>
                        </View>
                        <Image source={require("../../assets/img/arrow.png")} style={styles.image} />
                    </View>
                    <View style={styles.seperator} />
                </View>
            </TouchableOpacity>
        )
    }

    return (
        <View>
            <FlatList
            data={dataSource}
            renderItem={({item})=>renderBook(item)}
            keyExtractor={item=>item.id}
            style={{backgroundColor:"#fff"}}
            />
        </View>
    )
}
```

## AsyncStorage

https://reactnative.dev/docs/asyncstorage

https://docs.expo.io/versions/latest/react-native/asyncstorage/

類似瀏覽器的Cookie，唯一官方支援的儲存工具

> 存入 => AsyncStorage.setItem(key,value)

> 讀取 => AsyncStorage.getItem(key)

這是存儲存手機的local storage，可以在沒有網路的時候使用

### AsyncStorage結合async await

簡單的、異步的、持久化的Key-Value存儲系統。

處理異步(非同步)的救星:

async(非同步)await(等待)，等待這段函示完成後，在往下繼續執行



Await如果有錯誤發生，只會默默地拋出錯誤...

後面的程式碼就無法執行



需要Error handling錯誤處理(捕獲錯誤)

錯誤的接收執行動作->catch()

在src創建helpers資料夾在創建StorageHelper.js

```react
import { AsyncStorage } from 'react-native';

export const getMySetting = (key) =>  AsyncStorage.getItem(key);
export const setMySetting=(key,value)=>AsyncStorage.setItem(key,value)
```

使用

```react
import React,{useState,useEffect} from 'react'
import {StyleSheet, Text, View,Button,TextInput} from 'react-native'
import * as StorageHelper from '../Helps/StorageHelper'

export default function ProfileScreen(props) {
    const [name,setName]=useState("")

    useEffect(()=>{
        loadStorage()
        console.log("useEffect")
    },[])

    const loadStorage=async ()=>{
        let nameGet=await StorageHelper.getMySetting('name')
        if(nameGet){
            setName(nameGet)
        }
    }
    const changeName =async()=>{
        try{
            await StorageHelper.setMySetting("name",name)
        }catch(error){
            console.error(error)
        }
    }
    return (
        <View style={styles.container}>
            <Text style={styles.textStyle}>ProfileScreen app!</Text>
            <TextInput
            maxLength={8}
            style={{height:50,width:300,borderWidth:5,borderColor:"darkgray",fontSize:28,textAlign:"center",color:"#fff",backgroundColor:"gray"}}
            onChangeText={(text)=>setName(text)}
            value={name}
            />
            <Text>Hello {name} !</Text>
            <Button
            title="設定名字"
            onPress={()=>changeName()}
            />
        </View>
    )
}
```



## react-redux

```js
npm i redux redux-react-hook
```

在src創建redux資料夾在創建store.js、action.js、reducer.js

### 首先會在action.js

定義action名稱&動作(action creator)

```react
export const CHANGE_NAME="CHANGE_NAME";

export function changeName(newName){
    return {
        type:CHANGE_NAME,
        payload:{
            newName:newName
        }
    }
}
```

### reducer.js

監聽action，並依照不同動作更新狀態

```react
import {CHANGE_NAME} from './action'

const initialState={
    newName:"Avon"
}

export default function reducer(state=initialState,action){
    switch(action.type){
        case CHANGE_NAME:
            return {
                ...state,
                newName:action.payload.newName
            }
        default:
            return state
    }
}
```

### store.js

引入reducer到store,並讓之後app進入點可以引入這個store connects the reducer to the store

```js
import reducer from './reducer'
import {createStore} from 'redux'

export default function configureStore(){
    let store=createStore(
        reducer
    )

    return store;
}
```

### App.js

import這兩行，引入store和Provider組件

```react
import {StoreContext} from 'redux-react-hook';
import configureStore from './src/redux'
```

```react
const store=configureStore()

export default MyApp(){
    return (
        // 利用Provider引入store(Provider把store當成參數往下傳)
        <StoreContext.Provider value={store}>
            <App />
        </StoreContext.Provider>
    )
}
```

### 如何使用

新增兩行引入redux-react-hook和動作

```react
import {useMappedState,useDispatch} from 'redux-react-hook';
import {changeName} from '../redux/action'
```
讀取:

把全域的state`state.newName`轉變成props，讓這頁ProfileScreen用`myNewName`來讀取

觸發動作:

利用dispatch來呼叫執行動作，就會觸發reducer來返回新的state，store即會監聽到state的變化，合併回props

```react
const myNewName=useMappedState(state=>state.newName)
const dispatch=useDispatch()
```

```react
// 方法一
<Button
title="redux 設定名子"
onPress={()=>dispatch({type:"CHANGE_NAME",payload:{newName:name}}))}
/>
// 方法二
<Button
title="redux 設定名子"
onPress={()=>dispatch(changeName(name))}
/>
```

## Storage和redux有什麼不同

redux只是廣播全域變數，並不會儲存。

storage主要在APP restart後的data資料是否還存在

redux是state的狀態管理

兩者同時使用需要使用redux-persist


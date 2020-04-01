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
## Vue API管理

### 創建server.js

```js
import axios from 'axios'

const apiClient=axios.create({
    baseURL:'https://api/',
    withCredentials:false,
    headers:{
        Accept:"application/json",
        "Content-Type":"application/json"
    }
})

export const apiGet=data=>apiClient.get()
```

### 使用

```js
import {apiGet} from '@/server.js'

apiGet().then(res=>{
    // do somthing
})

```

## 攔截器使用

> 在初始化時，攔截器會馬上執行，可先在調用API之前使用攔截器，預先處理API的錯誤問題

```js
import axios from 'axios'
import { CURRENT_WEATHER } from '@/constants';

const apiClient=axios.create({
    baseURL:CURRENT_WEATHER,
    withCredentials:false,
    headers:{
        Accept:"application/json",
        "Content-Type":"application/json"
    }
})

apiClient.interceptors.request.use(
    request=>{
        console.log(request)
        return request
    }
)

apiClient.interceptors.response.use(
    response=>{
        console.log(response)
        return response
    },
    error=>{
        if(error.response){
            console.log(error.response)
        }
        return Promise.reject(error.response)
    }
)

export const getWeather=data=>apiClient.get()
```


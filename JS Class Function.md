#js
# First-Class Functions in JavaScript

```js
const sayHello=()=>{
    return "Hello";
}

const sayHelloToPerson=(greeter,person)=>{
	return greeter()+" "+person;
}

console.log(sayHelloToPerson(sayHello,"John"))
```

```js
const greeterMaker=greeting=>{
    return person=>{
        return greeting+" "+person
    }
}
const sayHelloToPerson=greeterMaker("Hello");
console.log(sayHelloToPerson('John'))
```

### example 1 Object Validation

```js
const usernameLognEnough=obj=>{
    return obj.username.length >= 5;
}
const passwordsMatch=obj=>{
    return obj.password === obj.confirmPassword
}
const objectIsVaild=(obj,...funcs)=>{
    for(let i=0;i<funcs.length;i++){
        if(funcs[i](obj) === false){
            return false;
        }
    }
    return true;
}

const obj1 = {
  username: 'abc123',
  password: 'foobar',
  confirmPassword: 'foobar',
};

const obj1Valid = objectIsVaild(obj1,usernameLognEnough,passwordsMatch);
console.log(obj1Valid) // true
```

### example 2 API Key Closure

```js
const apiContent=apiKey=>{
	const getData=route=>{
		return axios.get(`${route}?key=${apiKey}`)
	}
    const postData=(route,params)=>{
        return axios.post(route,{
            body:JSON.stringify(params),
            headers:{
                Authorization: `Bearer ${apiKey}`,
            }
        })
    }
    return {getData,postData}
}

const api = apiConnect('my-secret-key');

// No need to include the apiKey anymore
api.getData('http://www.example.com/get-endpoint');
api.postData('http://www.example.com/post-endpoint', { name: 'Joe' });
```

```js
var getPerson=name=>{
    const sayHello=say=>{
        return `${say} ${name}`;
    }
    const sayFood=food=>{
        return `${name} eat ${food}`; 
    }
    return {sayHello,sayFood}
}

var p=getPerson("John")
p.sayFood("rise")
```


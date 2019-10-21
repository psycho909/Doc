## example1

### old

```js
const getFirstUser = () => {
  const url = 'https://jsonplaceholder.typicode.com/users/1';
  fetch(url)
    .then(response => response.json())
    .then(json => console.log(json));
}
```

### now

```js
const getSecondUser = async () => {
  const url = 'https://jsonplaceholder.typicode.com/users/2';
  const response = await fetch(url);
  console.log(await response.json());
}
```

## example2

### old

```js
const handleSubmit = (event) => {
  event.preventDefault();
  UserAdapter.checkUser(user)
    .then(validUser => setCurrentUser(validUser));
};
```

### now

```js
const handleSubmit = async (event) => {
  event.preventDefault();
  const validUser = await UserAdapter.checkUser(user);
  setCurrentUser(validUser);    
};
```

## example3

### old

```js
getThirdUser().then(console.log);
```

### now

```js
const contrivedExample = async () => {
  await getThirdUser().then(console.log);
  // more function stuff
}
```
## example4

### old

```js
const getFirstUser = () => {
  const url = 'https://jsonplaceholder.typicode.com/users/1';
  fetch(url)
    .then(response => response.json())
    .then(json => console.log(json))
    .catch(err => console.log(err))
}
```

### now

```js
const getFourthUser = async () => {
  try {
    const url = 'https://jsonplaceholder.typicode.com/users/4';
    const response = await fetch(url);
    return response.json();
  catch (e) {
    // handle error
  }
}
```
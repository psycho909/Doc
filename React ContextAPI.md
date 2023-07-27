#react #js 
## 架構
1. `App` 主體
2. `Header`
3. `ProductList`
4. `Product`

## 創建共用`Context`

```react
// ./OrderContext

import {createContext} from 'react'

export const {Provider,Consumer}=createContext({
    orders:[],
    addOrder:()=>{}
});

```

## App

```react
import React, { Component} from 'react';
import Header from './Header'
import ProductList from './ProductList'
import {Provider} from './OrderContext'

class App extends Component {
  state={
    orders:[]
  }
  addOrder=order=>{
    this.setState({
      orders:[...this.state.orders,order]
    })
  }
  render() {
    const {orders}=this.state
    const contextValue={
      orders,
      addOrder:this.addOrder
    }
    return (
      <div className="App">
        <Provider value={contextValue}>
          <Header />
          <ProductList />
        </Provider>
      </div>
    );
  }
}

export default App;

```

## Header

```react
import React from 'react'
import {Consumer} from './OrderContext'

const Header=()=>{
    return (
        <div>
            <Consumer>
                { value=>(
                    `購物車(${value.orders.length})`
                ) }
            </Consumer>
        </div>
    )
}
export default Header

```

## ProductList

```react
import React from 'react'
import Product from './Product';

const products=[
    {id:1,name:"牛肉鍋"},
    {id:2,name:"豬肉鍋"},
    {id:3,name:"羊肉鍋"},
    {id:4,name:"雞肉鍋"},
    {id:5,name:"好肉鍋"}
]

const ProductList=()=>{
    return (
        <ul>
            {
                products.map(product=>{
                    return <Product {...product} key={product.id} />
                })
            }
        </ul>
    )
}

export default ProductList
```

## Product

```react
import React from 'react'
import {Consumer} from './OrderContext'

const Product=({id,name})=>{
    return (
        <li>
            <label>{name}</label>
            <Consumer>
            {
                value=>(
                    <button onClick={()=>value.addOrder(id)}>+</button>
                )
            }
            </Consumer>
        </li>
    )
}

export default Product

```


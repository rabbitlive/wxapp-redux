<div align="center">
	<img title="wechat" src="https://cdn.worldvectorlogo.com/logos/wechat.svg" width="100" hspace="20" />
	<img title="redux" src="https://cdn.worldvectorlogo.com/logos/redux.svg" width="100" hspace="20" />
	<h1>wxapp-redux</h1>
</div>



<h2 align="center">Installation</h2>

wxapp-redux 依赖于 **[Redux](https://github.com/reactjs/redux)**.

```sh
yarn add wxapp-redux
````


然后, 复制 `dist/wxapp-redux.js` 或 `dist/wxapp-redux.min.js` 到微信小程序项目中，比如`/lib`。当然，不要忘记导入`redux.js`。



<h2 align="center">Usage</h2>

#### Provider(store: Store)([appOptions: Object])

首先，需要注入`store`到全局的`App`，可以使用`Provider`：


```js
Provider(store)({
  foo: 42		
})
```


这里会将`store`绑定到`appOptions.store`属性上，这就要确保`appOptions.store`没有被占用。如果需要换其他key，需要传入`appOptions.storeKey`：


```js
Provider(store)({
  storeKey: 'foo'
})
```

Provider将多余的属性传递给App，并在其中已经内置了App。如果不想内置App和Page，可以将 `__WXAPPREDUXEXPORT__` 设置为 `true`：

```js
const options = Provider(store)({
  foo: 42,
  __WXAPPREDUXEXPORT__: true
})

App(options)
```


#### connect([mapStateToProps], [mapDispatchToProps], [mergeProps], [options])([pageOptions: Object = {}], [style: Object = undefined])

在`Page`中，使用`connect`可以将**redux store**绑定到view上：

```js
function mapStateToProps() {
  //...
}

function mapDispatchToProps() {
  //...
}

connect(mapStateToProps, mapDispatchToProps)()
```

`connect` 将 state 绑定到 `pageOption.data` 上，这样在视图中就可以使用 `{{foo}}` 来绑定属性。dispatch 会被绑定到 `pageOption` 上，可以通过视图的 bindTap 等事件触发 `dispatch(action)` 来改变 store。

`pageOptions` 的其他属性会原样传递给 `Page(pageOptions)`

同样，connect 也内置了 `Page`。

如果希望使用 [CSS Modules](https://github.com/css-modules/css-modules)，可以将 `style` 传递给 connect。这样 `style` 会被绑定到 `pageOption.data.style`。

下面是一个计数器 connect 的简单使用：

```js
/**
 * reducers/counter.js
 */
import { ActionType } from '../actions/counter'

export const initState = {
  value: 5,
  max: 10,
  min: 0
}

export default function(state = initState, action) {
  const { type, payload } = action

  switch(type) {
  case ActionType.COUNTER_INC:
    return Object.assign({}, state, {
      value: Math.min(state.max, state.value + 1)
    })

  case ActionType.COUNTER_DEC:
    return Object.assign({}, state, {
      value: Math.max(state.min, state.value - 1)
    })

  default: return state
  }
}



/**
 * actions/counter.js
 */
export const ActionType = {
  COUNTER_INC: 'COUNTER_INC',
  COUNTER_DEC: 'COUNTER_DEC'
}

export function counterInc() {
  return {
    type: ActionType.COUNTER_INC
  }
}

export function counterDec() {
  return {
    type: ActionType.COUNTER_DEC
  }
}


/**
 * pages/index/index.js
 */
import { compose } from 'redux'
import { connect } from 'wxapp-redux'
import * as action from '../../actions/counter'
import style from '../../styles/counter.css'

function mapStateToProps(state) {
  return {
    counter: state.counter.value
  }
}

function mapDispatchToProps(dispatch) {
  return {
    inc: compose(dispatch, action.counterInc),
    dec: compose(dispatch, action.counterDec)
  }
}


connect(mapStateToProps, mapDispatchToProps)({}, style)
```

```css
/**
 * styles/counter.css
 */
.btnHover {
  background: blue;
}
```

```html
<!-- pages/index/index.html -->
<view>
  <button size="mini" bindtap="dec" hover-class="{{style.btnHover}}">-</button>
  <view>{{counter}}</view>
  <button size="mini" bindtap="inc">+</button>
</view>
```


### TODO BuildTools

### TODO DevTools



<h2 align="center">LICENSE</h2>

> MIT License (MIT)

> Copyright (c) Rabbit

> Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

> The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

> THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

## 高阶组件

高阶组件就是一个函数，传给它一个组件，它返回一个新的组件。

```
const NewComponent = higherOrderComponent(OldComponent)
```

高阶组件是一个函数（而不是组件），它接受一个组件作为参数，返回一个新的组件。

使用形式：

```
import React, {Component} from 'react';

export default (WrappedComponent) => {
  class NewComponent extends Component {
    render() {
      return (
        <WrappedComponent></WrappedComponent>
      )
    }
  }
  return NewComponent
}
```

使用方式详解：

```
const higherComponent = (WrappedComponent, name) => {
  /**
   * 目的是为了实现组件的复用
   * 在目标组件的上层封装一层，做一些操作，把操作后的值传递给目标组件
   * 最后返回这个新组件
   */
  class NewComponent extends Component{

    UNSAFE_componentWillMount() {
      // 假设现在的复用代码就是从缓存中取特定的值
      let username = localStorage.getItem(name)
      if (!username) {
        username = '七月有风'
      }
      this.setState({
        data: username
      })
    }

    render() {
      return (
        <WrappedComponent data={this.state.data} />
      )
    }
  }
  return NewComponent
}

// 定义组件
class User extends Component{
  render() {
    return (
      <div>{this.props.data}</div>
    )
  }
}

// 包装，返回新的组件
const UserComponent = higherComponent(User, 'name')
```

这样就实现了代码的复用，如果另外一个组件在加载时也要有相同的操作，包装一些即可，不用重复写代码。


## context
通过 context，我们可以在父组件中定义一个状态，然后所有子组件都能访问。
React.js 的 context 就是这么一个东西，某个组件只要往自己的 context 里面放了某些状态，这个组件之下的所有子组件都直接访问这个状态而不需要通过中间组件的传递。一个组件的 context 只有它的子组件能够访问，它的父组件是不能访问到的，你可以理解每个组件的 context 就是瀑布的源头，只能往下流不能往上飞。

使用方法

```
// 父组件在 context 中定义状态
class App extends Component {
  /**
   * 如果不给组件加静态属性 childContextTypes,会有一个警告
   * Waring: App.getChildContext(): childContextTypes must be defined in order to use getChildContext()
   */
  static childContextTypes = {
    // 类型检查
    themeColor: .PropTypes.string
  }
  state = {
    color: 'green'
  }

  getChildContext() {
    return {themeColor: this.state.color}
  }
}

// 子组件中使用状态
class User extends Component {
  // 同样需要静态检查
  static contextTypes = {
    themeColor: PropTypes.string
  }

  render() {
    return (
      <div>{this.context.themeColor}</div>
    )
  }
}
```

一个组件可以通过 getChildContext 方法返回一个对象，这个对象就是子树的 context，提供 context 的组件必须提供 childContextTypes 作为 context 的声明和验证。
如果一个组件设置了 context，那么它的子组件都可以直接访问到里面的内容，它就像这个组件为根的子树的全局变量。任意深度的子组件都可以通过 contextTypes 来声明你想要的 context 里面的哪些状态，然后可以通过 this.context 访问到那些状态。
context 打破了组件和组件之间通过 props 传递数据的规范，极大地增强了组件之间的耦合性。而且，就如全局变量一样，context 里面的数据能被随意接触就能被随意修改，每个组件都能够改 context 里面的内容会导致程序的运行不可预料。


## redux 的学习

redux 是一个综合的状态管理容器，不是只有 react 才能使用，jQuery 也能使用。
主要解决的问题是多个组件之间的状态共享，归根到底就是定义一个全局对象，然后各个组件都能访问。但是这仅仅是第一层，当前仅仅做到这些是不够的，也是不合适的。因为我们要尽量避免全局变量。
这样做的后果是，每个组件都能更改全局的状态，会造成羡慕的难以维护。所以我们要制定一套规则，一套访问全局变量和更改变量的规则，这样做的话，我们就知道在什么地方做了何种操作。
组件无法直接更改状态管理器中的状态，需要通过派发一个调度 dispatch,通知状态管理中心修改特定的数据。
然后状态变化后还需要通知使用了的组件，进行页面的数据更新（所谓的发布－订阅模式）

实现方式：

```
// 定义 state
const appState = {
  title: {
    text: '七月有风',
    color: 'red'
  },
  content: {
    text: '八月有雨，九月有你',
    color: 'green'
  }
}

// 通过提交一个 action,包含操作的类型、携带的参数
function dispatch(action) {
  switch (action.type) {
    case 'UPDATE_TITLE_COLOR':
      appState.title.color = action.color
      break;
    case 'UPDATE_TITLE_TEXT':
      appState.title.text = action.text;
      break;
    default:
      break;
  }
}
```

这样做的好处是有一个统一的更改状态的方法，提高代码的可维护性。

实现更加友好的方式

```
/**
 * 统一的 store　生成方法
 * 传入 state 和　action
 * 返回　getState 和　dispatch 和　subscribe(实现发布订阅模式)
 */
const createStore = (state, stateChange) => {
  const listeners = []
  const getState = (state) => state
  const subscribe = (listener) => listeners.push(listener)
  const dispatch = (action) => {
    stateChange(state, action)
    listeners.forEach((item) => item())  
  }
  return {
    getState,
    subscribe,
    dispatch,
  }
}

// 创建原始数据
const initState = {
  username: '',
  token: '',
}

// 定义 action
const stateChange = (state, action) => {
  switch(action.type) {
    case 'UPDATE_USERNAME':
    state.username = action.username
    break;
    case 'UPDATE_TOKEN':
    state.token = action.token
    break;
  }
}

// 创建一个 store
const store = createStore(initState, stateChange)
// 订阅
store.subscribe(/* 值更改之后需要执行的操作 */)
// 通过调度更改值
store.dispatch()
```

这个版本的实现还是有问题的，现在的逻辑默认我们只要提交了 dispatch　页面就会重新渲染，不管数值有没有发生改变。
理想状态是，数据没有发生改变时，不同重新渲染页面(componentShouldUpdate)


扩展运算符的妙用：



## reducer　减速器

合并 appState 和 stateChange 如果什么都不传，就返回原始数据

改造 stateChange 函数

完整 redux 实现事例

```
// 创建 store
const createStore = (reducer) => {
  let state = null
  const listeners = []
  const subscribe = (listener) => listeners.push(listener)
  // 获取 state
  const getState = () => state
  const dispatch = (action) => {
    // 更新 state
    state = reducer(state, action)
    // 通知组件更新数据
    listeners.forEach((item) => item())
  }
  dispatch({})
  return {
    getState,
    subscribe,
    dispatch,
  }
}
// 更改数据 reducer
const reducer = (state, action)　=> {
  if (!state) {
    return {
      title: {
        text: '每天学习一点点',
        color: '#ddffee'
      },
      content: {
        text: '争取下一份工资月薪11k',
        color: 'green'
      }
    }
  }

  switch(action.type) {
    case 'UPDATE_TITLE_TEXT':
      return {
        ...state,
        title: {
          ...state.title,
          text: action.text
        }
      }
    case 'UPDATE_CONTENT_TEXT':
      return {
        ...state,
        content: {
          ...state.content,
          text: action.text
        }
      }
    default:
      console.log('default')
      return state
  }
}

class App extends Component {
  render() {
    return (
      <div className="App">
        <div className="title"></div>
        <div className="content"></div>
      </div>
    );
  }

  componentDidMount() {
    const store = createStore(reducer)
    let oldState = store.getState()
    this.renderApp(store.getState())

    // 订阅
    store.subscribe(() => {
      let newState = store.getState()
      this.renderApp(newState, oldState)
    })
    setTimeout(() => {
      store.dispatch({type: 'UPDATE_TITLE_TEXT', text: '能能能'})
      console.log(store.getState())
    }, 2000);
  }

  renderApp(newState, oldState={}) {
    if (newState === oldState) return
    this.renderTitle(newState.title, oldState.title)
    this.renderContent(newState.content, oldState.content)
  }

  renderTitle(newTitle, oldTitle) {
    if (newTitle === oldTitle) return
    const titleDom = document.getElementsByClassName('title')[0]
    titleDom.innerHTML = newTitle.text
    titleDom.style.color = newTitle.color
  }

  renderContent(newContent, oldContent) {
    if(newContent === oldContent) return
    const contentDom = document.getElementsByClassName('content')[0]
    contentDom.innerHTML = newContent.text
    contentDom.style.color = newContent.color
  }
}
```

## 纯函数（Pure Function）

一个函数的返回结果只依赖于它的参数，并且在执行过程里面没有副作用，我们就把这个函数叫做纯函数。

代码演示：

```
// 函数的返回结果只依赖于它的参数
let a = 0
const add = (b) => a + b
add(2)  // 2
let a = 1
add(2)  // 3,传入相同的参数，返回值不同，不是纯函数
```

```
// 函数执行过程没有副作用,即对其他变量没有影响，特别是对实参
const add = (obj, b) => {
  obj.x = 2
  return obj.x + b
}
let o = {x:1}
add(o, 2)  // 4
o  // {x:2} => 更改了实参，原始值是通过复制，引用值是直接引用地址
```

对象与对象作比较，比较的是引用地址

```
const oldState = {a: 1, b: 2}
const newState = oldState  // 让两个变量指向相同的引用地址
newState.a = 3
newState === oldState  // true
```

## 一个 yarn start 启动报错

Error: ENOSPC: System limit for number of file watchers reached, watch '/home/cat/Desktop/redux_study/public'

权限不够，无法实现对项目各个文件的监听
解决方法：sudo yarn start

dispatch -- 调度

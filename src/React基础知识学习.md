# create-react-app 新版默认使用 function 定义组件

现在

```
import React from 'react'

function App() {
  return (
    <div className="App">Hello React</div>
  )
}
```

之前的

```
import React, {Component} from 'react'

class App extends Component {
  constructor(props) {
    super(props)
  }

  render() {
    return (
      <div className="App">Hello React</div>
    )
  }
}
```

像这种在 .js　文件中书写 HTML 的语法称为 JSX

JSX 的由来

每个 DOM 元素的结构都可以用 JavaScript 的对象来表示。
React.js 就把 JavaScript 的语法扩展了一下，让 JavaScript 语言能够支持这种直接在 JavaScript 代码里面编写类似 HTML 标签结构的语法(如果每一个 DOM 的书写都用JavaScript这种描述写法，会非常的不方便，React 对它进行了精简，让我们可以向书写原生 HTML 的形式去书写 JavaScript　对象)；
最终的编译还是会把类似 HTML 的 JSX 结构转换成 JavaScript 的对象结构。

React.craeteElement()

```
<div class="App">Hello React</div>

{
  tag: 'div',
  attrs: {className: 'App'},
  children: ['Hello React']
}

```

## 总结
* JSX 是 JavaScript 语言的一种语法扩展，长得像 HTML，但并不是 HTML。
* React.js 可以用 JSX 来描述你的组件长什么样的。
* JSX 在编译的时候会变成相应的 JavaScript 对象描述。
* react-dom 负责把这个用来描述 UI 信息的 JavaScript 对象变成 DOM 元素，并且渲染到页面上


## 组件的思想
React.js 中一切皆组件，用 React.js 写的其实就是 React.js 组件。我们在编写 React.js 组件的时候，一般都需要继承 React.js 的 Component（还有别的编写组件的方式我们后续会提到）。一个组件类必须要实现一个 render 方法，这个 render 方法必须要返回一个 JSX 元素。

书写 JSX 的注意事项

1、render() 函数不能返回多个 JSX 元素，如果要返回多个 要在最外层嵌套一个 JSX 元素

```
function App() {
  return (
    <div className="Main">
      <header></header>
      <Main></Main>
    </div>
  )
}
```
2、class 要用 className 代替，因为 class 是 JavaScript　中的关键词
3、<label for="xx"> 中的 for 也不能使用(同上)，要使用 htmlfor
4、插值表达式，由一堆中括号组成 {} 里面可以放置任何 JavaScript 代码，比如表达式、变量、函数执行,最终渲染在页面中的是这些代码的返回结果
5、JSX 元素变量，JSX 元素其实可以像 JavaScript 对象那样自由地赋值给变量，或者作为函数参数传递、或者作为函数的返回值。
6、自定义的组件都必须要用大写字母开头，普通的 HTML 标签都用小写字母开头。


## 事件监听

直接在 JSX 元素上通过 onClick onChange 的形式，React 已经为我们封装好了兼容性，我们只需指定事件类型和事件处理函数即可。

*这些 on* 的事件监听只能用在普通的 HTML 的标签上，而不能用在组件标签上*

关于事件中的 this 指向

```
class Title extends Component {
  constructor(props) {
    super(props)
    this.state = {
      title: 'Hello React'
    }
  }

  render() {
    return (
      <div onClick={this.handleClick} className="Title">{this.state.title}</div>
    )
  }

  handleClick() {
    // 这里的 this 指向 undefined,而我们期望它指向当前的组件
    console.log(this)
  }
}
```

这是因为 React.js 调用你所传给它的方法的时候，并不是通过对象方法的方式调用（this.handleClickOnTitle），而是直接通过函数调用 （handleClickOnTitle），所以事件监听函数内并不能通过 this 获取到实例。

在这里我们期望指向当前的组件，有两种解决方法
* bind(this) 硬绑定，指向当前组件(在constructor 中绑定，在 JSX 元素上绑定)
* 使用箭头函数，箭头函数中的 this 会默认绑定到当前组件上，无法改变

*this绑定的规则是什么？*
简而言之，this 指向调用者

全局中的 this 指向 window,严格模式下指向 undefined
一个对象调用一个方法，那么这个方法中的 this 指向该对象
箭头函数中的 this 无法改变，指向所在环境的上层作用域
可以通过 call apply bind 改变 this 指向，其中 bind() 返回的是绑定过 this 的新方法，call apply 返回方法的执行

应用场景：

```
setInterval(function() {
  // 这里的 this 不是我们期望的，期望取得是其所在的上层作用域，可直接通过箭头函数
}, 1000)
```


## 组件中的 state 和 setState

一个组件的显示形态是可以由它数据状态和配置参数决定的。一个组件可以拥有自己的状态,来控制页面的展示。

state
相当于一个容器，里面存储了当前组件的一些数据和状态
使用的时候直接 this.state.xxx 即可

setState
既然有一个存储数据和状态的容器，那么就有更改数据和状态的方法。
你可能会想直接使用 this.state.xxx = xxx 的语法更改 state 中的值(vue 中使用 this.data.xxx = xxx),其实不然如果你这么做 React 会给你一个警告

```
Do not mutate state directly. Use setState()
```

setState 的参数可以是对象，也可以是函数

对象

```
this.setState({
  title: 'React'
})
```

函数

```
this.setState((prevState) => {
  // prevState 表示当前的 state
  // 返回一个对象
  prevState.title = 'React'
})
```
setState 方法由父类 Component 所提供。当我们调用这个函数的时候，React.js 会更新组件的状态 state ，并且重新调用 render 方法，然后再把 render 方法所渲染的最新的内容显示到页面上。

setState 合并

为了性能的考虑，React 会帮助我们合并多个 setState，要不然会触发多次 render 函数的渲染


## 配置组件的 props
为了做到组件的可复用性，需要传入一些配置项，通过 props 接收

使用 class 定义的组件可以直接通过 this.props 接收父组件的传值

使用 function 定义的组件还不知道的怎么使用　Hook?

使用默认配置 defaultProps

```
class Header extends Component {
  static defaultProps = {
    content: 'no content'
  }
}
```

注：不能直接修改 props 的值，因为 React 中的传值是像水流一样由上往下的，上一层可以修改值，而通过 props 接收相关属性的组件不能直接修改该值，
因为上一层组件中的这个值，可能同时传递给多个组件，如果直接修改了，会导致其他组价显示异常，所以控制权还是交给持有该值的组件。

## state vs props

state 的主要作用是用于组件保存、控制、修改自己的可变状态。state 在组件内部初始化，可以被组件自身修改，而外部不能访问也不能修改。你可以认为 state 是一个局部的、只能被组件自身控制的数据源。state 中状态可以通过 this.setState 方法进行更新，setState 会导致组件的重新渲染。

props 的主要作用是让使用该组件的父组件可以传入参数来配置该组件。它是外部传进来的配置参数，组件内部无法控制也无法修改。除非外部组件主动传入新的 props，否则组件的 props 永远保持不变。

一句话总结：state 是让组件控制自己的状态，props 是让外部对组件自己进行配置。

有状态组件 vs 无状态组件

```
// 有状态组件可以使用 state 作为自身的状态管理
class Header extends Comonent {

}

// 无需继承自 Component,一个函数就是一个无状态组件
const Main = (props) => {
  return (
    <div>{props.xxx}</div>
  )
}
```

无状态组件中如何使用 defaultProps?

## 渲染列表数据

在原生 JavaScript 或者 jQuery 中渲染一个列表中的数据，往往要用到字符串拼接，在 React 中最后只需要传入相关数据，借助数组的 map 方法即可

```
class List extends Component {
  constructor() {
    super()
    this.state = {
      list: ['R', 'E', 'A', 'C', 'T']
    }
  }
  render() {
    return (
      <div className="Wrapper">{this.state.list}</div>
    )
  }
}
```

如果我们往插值表达式中{} 中放入一个数组，React 会帮助我们把数组的每一项渲染到页面中
但是需要注意的是，如果数组中的子项不是原始值，则会报错，而且为了灵活性，我们需要把类表中的每一项渲染到指定的 DOM 节点上

所以我们考虑把数组里面的数据先做一层处理

```

  constructor() {
    super()
    this.state = {
      list: [{
        id: 1,
        text: 'react'
      },{
        id: 2,
        text: 'redux'
      }, {
        id: 3,
        text: 'react-native'
      }, {
        id: 4,
        text: 'ant-design'
      }]
    }
  }
  render() {
    const {list} = this.state
    const renderList = []
    for (let item of list) {
      renderList.push(
        <div>
          <div>{item.id}</div>
          <div>{item.text}</div>
        </div>
      )
    }
    return (
      <div className="Wrapper">{renderList}</div>
    )
  }
}

```

实际上根据前面的学习，我们了解到只要往里面放一个数组，每一项就会被渲染出来，包括 JSX 语句
后面我们可以通过数组的循环手动把原始数据处理成 JSX 语法，归根到底还是在插值表达式中{}放入了处理过后的数组
这里多了一层嵌套，实际上我们直接可以使用 Array.map 返回一个新数组的特性，直接在{}中操作即可

```
render() {
  return (
    {/* js的语句执行必须包含在{}里面，JSX 语法中必须要这样才能注释 */}
    {
      this.state.list.map((skill) => {
        return (
          <div>
            <div>{skill.id}</div>
            <div>{skill.text}</div>
          </div>
        )
      })
    }
  )
}
```

这样写了代码之后，React 会发出警告

```
Warning: Each child in a list should have a unique "key" prop.
```
这是提示我们在列表循环的时候，需要给每一项指定一个特殊 key,作为它的唯一标识
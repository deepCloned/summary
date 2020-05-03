## defaultProps 使用方法

都是借助 static 把设置挂到组件上

```
class MyComponent extends Components {
  // 第一种写法
  static defaultProps = {

  }
}

// 第二种写法
MyComponent.defaultProps = {

}
```

向数组的第一位追加一项 - push  pop
向数组最后以为追加 - unshift  shift

## 挂载阶段的组件生命周期

```
ReactDOM.render(
  <App />,
  document.getElementById('root')
)
```

会被编译成

```
ReactDOM.render(
  React.createElement(App, null),
  document.getElementById('root')
)
```

React.js 将组件渲染，并且构造 DOM 元素然后塞入页面的过程称为组件的挂载。

React 组件挂载的生命周期过程

constructor
componentWillMount: Rename componentWillMount to UNSAFE_componentWillMount to suppress this warning in non-strict mode.
waring: Using UNSAFE_componentWillMount in strict mode is not recommended and may indicate bugs in your code. (不建议使用了)
render
componentDidMount

包含父子组件时的执行顺序

父 render
子 render
子 didMount
父 didMount

React render函数为什么执行两次？

React 组件删除的生命周期

componentWillUnmount

生命周期函数应用：
我们一般会把组件的 state 的初始化工作放在 constructor 里面去做；在 componentWillMount 进行组件的启动工作，例如 Ajax 数据拉取、定时器的启动；组件从页面上销毁的时候，有时候需要一些数据的清理，例如定时器的清理，就会放在 componentWillUnmount 里面去做。

说一下本节没有提到的 componentDidMount 。一般来说，有些组件的启动工作是依赖 DOM 的，例如动画的启动，而 componentWillMount 的时候组件还没挂载完成，所以没法进行这些启动工作，这时候就可以把这些操作放在 componentDidMount 当中。componentDidMount 的具体使用我们会在接下来的章节当中结合 DOM 来讲。

更新阶段的组件生命周期
由 setState 方法触发，setState 导致 React.js 重新渲染组件并且把组件的变化应用到 DOM 元素上的过程，这是一个组件的变化过程。

shouldComponentUpdate(nextProps, nextState) {
  // 返回 true or false 返回 false 表示不渲染组件
  /**
    * 同级的组件如果触发了父组件的 setState 方法，会执行父组件的 render 函数
    * 然后会进一步触发当前组件的 render 函数
    * 可是当前组件明明什么都没做，数据也没更新，没必要重新执行一次 render 函数
    * 这里可以拿当前 state 和　接下来的 nextState 作对比，如果没有改变，返回 false 即可
    * 所以说该组件能在一定程度上提升性能
    */
  const state = this.state
  if (nextState === state) {
    return false
  }
  return true
}

componentWillReceiveProps(nextProps) {

}

componentWillUpdate() {}

componentDidUpdate() {}


## ref
React 不建议我们操作 DOM，但是也提供了相应的接口供我们使用
ref,直接绑定在 DOM 节点上

```
<div ref="demo"></div>

// 使用
this.refs.demo  // 需要注意的是等到 DOM 挂载完成才能使用 componentDidMount
```

## dangerouslySetHTML 和 style 属性
出于安全考虑的原因（XSS 攻击），在 React.js 当中所有的表达式插入的内容都会被自动转义，就相当于 jQuery 里面的 text(…) 函数一样，任何的 HTML 格式都会被转义掉

如果不启用这个模式，会把我们输入的标签直接转成普通字符串

如何书写行间样式 style

```
<div style={{background: '#f8f8f8'}}>
```


## PropTypes 类型检测

子组件接收父组件传值如果不做类型判断，会导致一些难以捉摸的错误（TypeScript）

```
import PropTypes from 'prop-types'

class xxx extends Component{
  static propTypes = {
    xxx: Protypes.array.isRequired
  }
}
```

PropTypes.xxx 指定接收值的类型
isRequired  表示必传，必传的情况下就不需要设置 defaultProps 了
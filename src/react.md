# React

## JSX => JavaScript

JSX

```
  <div class="react">
    <span>React<span>
    <span>React Dom</span>
  </div>
```

转化为 JavaScript

```
React.createElement('div', {
  class: 'react',
}, 
  React.createElement('span', null, 'React'),
  React.createElement('span', null, 'React Dom'),
)
```

React.createElement(name, properties, childred)
第一个参数 Dom 节点名，原生 dom 元素即为字符串，如果是通过 function 定义的组件（约定为大写，用于与原生节点区分），即为组件名
第二个参数 Dom 节点上的属性　没有就为 null,有就是一个对象，键值对
第三个参数子节点的相关信息，同上,多个子节点接在后面
子节点后面再接子节点...


定义组件

```
function Button() {
  reutrn <button></button>
};
```

```
React.createElement(Button, null);
```
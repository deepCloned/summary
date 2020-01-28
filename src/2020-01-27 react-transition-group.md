## React 动画的使用

### 使用 css3 动画
* transition
* animation

### react-transition-group
>按需引入 CSSTransition TransitionGroup，原理就是在想要有动画效果的 DOM 元素外面套一层组件

/* CSSTransition 用法 */

```
<CSSTransition
  in={ state }
  timeout={ 500 }
  classNames="fade"
  unmountOnExit
  appear={ true }
>
</CSSTransition>
```
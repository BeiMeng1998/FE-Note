# ReactElement.js

```
export function createElement(type, config, children) {
  // 处理参数

  return ReactElement(
    type,
    key,
    ref,
    self,
    source,
    ReactCurrentOwner.current,
    props,
  );
}

const ReactElement = function(type, key, ref, self, source, owner, props) {
  const element = {
    // This tag allows us to uniquely identify this as a React Element
    $$typeof: REACT_ELEMENT_TYPE,

    // Built-in properties that belong on the element
    type: type,
    key: key,
    ref: ref,
    props: props,

    // Record the component responsible for creating this element.
    _owner: owner,
  };

  return element
}
```

ReactElement 通过 createElement API 创建

需要传入三个参数(type, config, children)：
type：指代这个 ReactElement 的类型
    1.字符串如'div', 'p'代表原生DOM，称为 HostComponent
    2.Class类型是我们继承自 Component 或者 PureComponent 的组件，称为 ClassComponent
    3.方法类型就是 functional Component
    4.原生提供的 Fragment、AsyncMode 等是 Symbol，会被特殊处理
    5.TODO: 是否有其他的

从 config 对象中保存着 attribute 键值对，从 config 对象中筛选出 props 的内容以及特殊的 key, ref 这样的属性

从源码可以看出虽然创建的时候都是通过 config 传入的，但是 key 和 ref 不会跟其他 config 中的变量一起被处理，而是单独作为变量出现在 ReactElement 上。

children 中放标签内容

在最后创建 ReactElement 我们看到了这么一个变量 $$typeof ，在这里可以看出来他是一个常量：REACT_ELEMENT_TYPE，但有一个特例： ReactDOM.createPortal 的时候是 REACT_PORTAL_TYPE，不过他不是通过 createElement 创建的，所以他应该也不属于ReactElement

ReactElement 是一个JS对象，是一个用来承载信息的容器，它会告诉后续操作这个节点的以下信息
    1.type 类型，如何创建DOM节点
    2.key 和 ref 这些特殊信息
    3.props 新的属性内容
    4.$$typeof 用于确定是否属于 ReactElement

# ReactBaseClasses.js

## Component

```
function Component(props, context, updater) {
  this.props = props;
  this.context = context;
  // If a component has string refs, we will assign a different object later.
  this.refs = emptyObject;
  // We initialize the default updater but the real one gets injected by the
  // renderer.
  this.updater = updater || ReactNoopUpdateQueue;
}

Component.prototype.isReactComponent = {};

Component.prototype.setState = function(partialState, callback) {
  invariant(
    typeof partialState === 'object' ||
      typeof partialState === 'function' ||
      partialState == null,
    'setState(...): takes an object of state variables to update or a ' +
      'function which returns an object of state variables.',
  );
  this.updater.enqueueSetState(this, partialState, callback, 'setState');
};
```
只是一般的组合继承，3个参数（props, context, updater）
而setState()方法也只是调用了updater.enqueueSetState()

## PureComponent

`pureComponentPrototype.isPureReactComponent = true;`

# ReactCreateRef.js

```
export function createRef(): RefObject {
  const refObject = {
    current: null,
  };
  if (__DEV__) {
    Object.seal(refObject);
  }
  return refObject;
}
```

# ReactForwardRef.js

```
export function forwardRef<Props, ElementType: React$ElementType>(
  render: (props: Props, ref: React$Ref<ElementType>) => React$Node,
) {
  const elementType = {
    $$typeof: REACT_FORWARD_REF_TYPE,
    render,
  };
  return elementType;
}
```

# ReactContext.js

用于祖先组件的通信
祖先`<Provider value={this.state.xxx}></Provider>`
后代`<Consumer>{value => {xxx}}</Consumer>`

# ConcurrentMode

ConcurrentMode = REACT_CONCURRENT_MODE_TYPE(就是一个Symbol)
可以控制组件更新的优先级
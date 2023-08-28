# React

### 所有Api
| 名称   | 描述 |  建议或更多 |
| :----- | :--: | -------: |
| **[cloneElement](#cloneElement)** |  覆盖组件props  | 接收props的function替代 |
| **[Component](#Component)** |  `class YourComponent extends Component {}`  | 建议使用函数组件替代 |
| **[createElement](#createElement)** | 创建一个React节点  |直接创建标签组件 |
| **[createFactory](#createFactory)** | 创建一个可以创建指定类型React元素的工厂函数  |直接使用JSX创建标签组件 |
| **[createRef](#createRef)** | class组件中创建ref对象  | 在函数组件中使用`useRef` |
| **[PureComponent](#PureComponent)** | 根据props和state的变化判断更新的类组件, [浅层比较]  | 使用函数组件实现 |
||||
| **[createContext](#createContext)** | 创建一个context对象 | 在函数组件中搭配`useContext`使用, 一旦context数据改变，所有的子组件均重新渲染  |
| **[forwardRef](#forwardRef)** | 将组件的ref暴露给父节点 | 适用于函数组件,可直接暴露组件，也可搭配`useImperativeHandle`使用 |
| **[lazy](#lazy)** | 延迟加载组件，传入load函数，返回一个Promise, 并被缓存 | 首次渲染提升性能， 可搭配`<Suspense>`使用按需加载组件  |
| **[memo](#memo)** | 提升React组件渲染性能，外部变化：判断仅限父组件传入的props的变化更新组件[context变化时仍然渲染], 内部：正常组件渲染机制  | 用于函数组件、 forwardRef包裹的组件 |
| **[startTransition](#startTransition)** | 不阻塞ui的情况下更新state[官方描述] | 接收一个同步的函数[callSetState]，无返回值，  callSetState立即执行，将state更新标记为transition; 优先级：低于组件的输入更新。**不能用于控制文本输入** |
||||
| **[Fragment](#Fragment)** | React组件Api，唯一支持属性值 `key`  | <Fragment></Fragment> 或者 <></>, 不增加节点的情况下包裹多个组件|
| **[Profiler](#Profiler)** | 属性：`id`<string>,`onRender`<Function>:当包裹的组件更新时被调用  | **仅用于开发环境中测试组件的渲染性能**|
| **[StrictMode](#StrictMode)** | 严格模式组件  | 即时警告组件错误 |
| **[Suspense](#Suspense)** | 组件的替代方案, 属性：fallback={<Loading />}  | 组件挂起[首次渲染之前除外]，未渲染完成前的替代例如：loading。。。, 写在fallback中 |
||||
| **[createRoot](#createRoot)** | <a id="createRootMenu"></a>在浏览器中创建渲染组件的根节点  | `createRoot(Node, options)` 入参：Node:dom 元素， options?：对节点的配置 |
| **[hydrateRoot](#hydrateRoot)** | <a id="hydrateRootMenu"></a>**18.x** 可以在服务端生成的节点中渲染React组件  | `hydrateRoot(domNode, reactNode, options)` 入参：Node:dom 元素，reactNode: 服务端生成的节点 ,options?：对节点的配置 |
||||
| **[renderToNodeStream](https://react.docschina.org/reference/react-dom/server/renderToNodeStream)** | **18.x弃用** 推荐使用`renderToPipeableStream`  | 为 Node.js 只读流 渲染 React 树  |
| **[renderToPipeableStream](https://react.docschina.org/reference/react-dom/server/renderToPipeableStream)** | **18.x** 将React组件树渲染为Node.js流  | 专供Node.js使用  |
| **[renderToReadableStream](https://react.docschina.org/reference/react-dom/server/renderToReadableStream)** | 将 React 树渲染后发送至 Web 可读流  | web中使用 |
| **[renderToStaticMarkup](https://react.docschina.org/reference/react-dom/server/renderToStaticMarkup)** | 将非交互的 React 组件树渲染成 HTML 字符串  | 适用于静态页面，静态内容 |
| **[renderToStaticNodeStream](https://react.docschina.org/reference/react-dom/server/renderToStaticNodeStream)** | 为 Node.js 只读流将 React 树渲染为静态 HTML | 适于做静态页面生成器，渲染静态内容 |
| **[renderToString](https://react.docschina.org/reference/react-dom/server/renderToString)** | 将 React 树渲染为一个 HTML 字符串 | **不支持流式传输或等待数据** ，需用替代方案|
||||
| **[createPortal](#createPortal)** | **18.x** 将 JSX 作为 children 渲染至 DOM 的不同节点，只是改变了组件所处的位置, context、事件冒泡等仍有效  |参数：`children:JSX`, domNode:要渲染的document中的节点 |
| **[flushSync](#flushSync)** | 强制React同步更新，可确保DOM立即更新 **会降低应用性能，请谨慎使用， 一般不建议使用**| 入参：`callback:Function`, 会刷新所有等待的更新， 以及callback内部更新有关的所有更新 |
| **[findDOMNode](#findDOMNode)** | **18.x** 已弃用， 有替代方案`ref`  |参数：`componentInstance:classComponent`, 查找类组件DOM实例在浏览器中对应的节点 |
| **[hydrate](#hydrate)** | **18.x** 已弃用， 有替代方案[`hydrateRoot`](#hydrateRootMenu)  |[参数：](#hydrateRootMenu) |
| **[render](#render)** | **18.x** 已弃用， 有替代方案[`createRoot`](#createRootMenu)  |参数: `reactNode`, `domNode`, `callback?`:组件插入domNode后调用 |
| **[unmountComponentAtNode](#unmountComponentAtNode)** | **18.x** 已弃用， 有替代方案`root.unmont`  |参数: `domNode`, 从DOM中移除已挂载的组件 |
||||


### 需注意的Api以及替代方案

<a id="cloneElement"></a>
1. cloneElement

```javascript
  // 原用法
  React.cloneElement(<YourComponent/>, newProps)
  // 替代法
  const RenderComponent=(props)=>{return <YourComponent {...props}/>}
  const getCloneElement = ({newProps, RenderComponent})=>{
    ...
    return (<>
      {RenderComponent(newProps)}
    </>)
  }
```
<a id="Component"></a>
2. Component

<a id="createElement"></a>
3. createElement

```javascript

  // 原用法
  React.createElement('div', divProps, ...divChildrens)
  // 替代法
  const getNewDiv=(divProps, divChildrens)=>{
    ...
    return (<div {...divProps}>
      {divChildrens.map(child=>(child))}
    </div>)
  }
```
<a id="createFactory"></a>
4. createFactory

```javascript

  // 原用法
  const createButton = React.createFactory('button')
  const btn = createButton({
    onClick, ...
  }, 'btn-name')
  // 替代法
  const btn=(props, btnName)=>{
    ...
    return (<Button {...props}>
      {btnName}
    </Button>)
  }
```
<a id="createRef"></a>
5. createRef

<a id="PureComponent"></a>
6. PureComponent

```javascript

  // 原用法
  class YourComponent extends PureComponent {
    ...
    render(){
      const {className} = this.props //当props有变化时才会更新
      return <div className={className}>
        {...}
      </div>
    }
  }
  // 替代法
  const YourComponent =(props)=>{
    const {className} = props
    ...
    return (<div className={className}>
        {...}
      </div>)
  }
```

### 其他Api

<a id="createContext"></a>
7. createContext

```javascript
  // 示例
  //index.js
  import React, {createContext, useContext, useState} from 'react'
  export const yourContext = createContext({color:'black', count:-1})
  const yourComponent = ()=>{
    const context = useContext(yourContext)
    const [yourDefaultValues, yourDefaultValuesSet] = useState(context)
    const onClick=()=>{
      const values = {...yourDefaultValues}
      values.count += 1
      yourDefaultValuesSet(values)
    }
    ...
    return (
      <context.Provider value={yourDefaultValues}>
        <div><YourForm /></div>
        <Button onClick={onClick}>{`myColor:${yourDefaultValues.color}`}</Button>
      </context.Provider>
    )
  }

  //Form yourForm.js
  import {yourContext} from './index.js'
  const YourForm=()=>{
    const datas = useContext(yourContext)
    return (
      <Form>
        <Input value={datas.color}/>
        <Input type="number" value={datas.count}/>
      </Form>
    )
  }

```

<a id="forwardRef"></a>
8. forwardRef

```javascript
  // 示例
  
  // index.js
  ...
  import React, {useRef} from 'react'
  import ListGroup from './listGroup.js'
  const yourComponent=()=>{
    const listGroupRef = useRef()
    const getListGroupRef=()=>{
      //直接暴露List组件
      console.log(listGroupRef?.current) // 输出 formRef对象的值

      // 搭配useImperativeHandle使用
      console.log(listGroupRef?.current.formRef) // 输出 formRef对象的值
    }
    ...
    return (
      <div>
        <ListGroup ref={listGroupRef}/>
      </div>
    )
  }

  //listGroup.js
  ...
  import React, {forwardRef, useImperativeHandle} from 'react'
  let ListGroup;
  // 直接暴露组件
  ListGroup = (props, ref)=>{
    // props为组件的props, ref为父节点中暴露ListGroup的ref
    return (
      <>
        <Form ref={ref}>...</Form>
      </>
    )
  }
  // 搭配 useImperativeHandle 使用
  ListGroup = (props, ref)=>{
    // props为组件的props, ref为父节点中暴露ListGroup的ref

    const formRef = useRef() //Form的ref
    const [ln, lnSet] = useState(0) //list长度

    useImperativeHandle(ref, ()=>{
      return {ln, formRef}
    }, [])

    return (
      <>
        <Form ref={formRef}>... </From>
      </>
    )
  }
  export default forwardRef(ListGroup)

```

<a id="lazy"></a>
9. lazy

```javascript
  //直接使用
  const loadComponent = React.lazy(()=>import('./index.js'))

  // 搭配 <Suspense>使用，按需加载组件，并缓存
  import React, {lazy, useState, Suspense} from 'react'
  const loadComponent = lazy(delayComponent(()=>import('./index.js'), 200))
  const YourComponent = ()=>{
    const [isShow, isShowSet] = useState(false)
    return (
      <div>
        <Button onClick={()=>{isShowSet(!isShow)}}>triggle loadComponent</Button>
        <Suspense fallback={<>...loading</>}>
          isShow && <loadComponent />
        </Suspense>
      </div>
    )
  }
  // 延迟加载时间
  const delayComponent = (fn, time)=>{
    return new Promise(resolve=>{
      setTimeout(()=>{
        fn(),
        resolve(true)
      }, time)
    })
  }
```

<a id="memo"></a>
10. memo

```javascript
  const YourComponent = (props)=>{
    ...
    return (
      <div>
        {props.age}
        ...
      </div>
    )
  }
  // 记忆化组件
  const Memoried = mome(YourComponent)

  // 使用
  function Home(){
    const [count, countSet] = useState(0)
    const [age, ageSet] = useState(20)

    return (
      <div>
        <div className="show-count">{count}</div>
        <Button onClick={()=>{
          // button click 修改count , div.show-count,重新渲染，但 <Memoried />不重新渲染
          countSet((c)=>c+1)
        }}>compute</Button>
        ...
        {/* 当父组件传入的age 变化时才重新渲染 */}
        <Memoried age={age}/>
      </div>
    )
  }

```

<a id="startTransition"></a>
11. startTransition

```javascript

  const YourComponent=()=>{
    const [type, typeSet] = useState('small')
    const onChangeType=(typeIn)=>{
        startTransition(()=>{
          typeSet(typeIn)
        })
    }
    ...
    return (
      <>
        ...
        <div>{type}</div>
        <Button onClick={()=>{ onChangeType('large')}}>change-type</Button>
      </>
    )
  }
  // 示例场景
 /* [官方] 通过 transition，你的 UI 在重新渲染过程中保持响应。例如，如果用户单击一个选项卡后又改变主意并单击另一个选项卡，则可以在第一次重新渲染完成之前执行此操作而无需等待。 */
```


<a id="Fragment"></a>
12. Fragment

```javascript

  const YourComponent = (props)=>{
    ...

    return (
      <Fragment key={pkey}>
        <List />
        <div>...</div>
        ...
        <>
          <div>...</div>
          <Checks />
        </>
      </Fragment>
    )
  }
```

<a id="Profiler"></a>
13. Profiler

```javascript
  // 示例
  <Profiler id="YourComponent" onRender={onRender}>
    <YourComponent />
  </Profiler>

  const onRender=(id, phase, actualDuration, baseDuration, startTime, commitTime)=>{
    // id: string, 组件树的id
    // phase: mount|update|nested-update, 组件渲染的激活来源，mount:首次挂载激活，update|nested-update: props|state|hook激活的更新
    // actualDuration： 本次更新中渲染 <Profiler> 组件树的毫秒数
    // baseDuration： 估算在没有任何优化的情况下重新渲染整棵 <Profiler> 子树所需的毫秒数
    // startTime： 当 React 开始渲染此次更新时的时间戳。
    // endTime： 当 React 提交此次更新时的时间戳。此值在提交的所有 profiler 中共享，如果需要，可以对它们进行分组。
  }
```

<a id="StrictMode"></a>
14. StrictMode

```javascript

  <StrictMode>
    <YourComponent />
  </StrictMode>

```

<a id="Suspense"></a>
15. Suspense

```javascript

  <Suspense fallback={<Loading />}>
    <YourComponent />
  </Suspense>

  // loading
  const Loading = ()=>{
    return (
      <>...loading</>
    )
  }
```

### React-dom 客户端Api
<a id="createRoot"></a>
16. createRoot

```javascript
  import {createRoot} from 'react-dom/client'

  const options={
    onRecoverableError: ()=>{}, //react从异常恢复后调用
    identifierPrefix: string, // 结合 useId 生成的id string prefix 避免节点之间的冲突
  }
  const El = document.querySelect('#root')
  const root = createRoot(El, options)
  root.render(<App />)

```
<a id="hydrateRoot"></a>
17. hydrateRoot

```javascript
  import {hydrateRoot} from 'react-dom/client'
  
  const options={
    onRecoverableError: ()=>{}, //react从异常恢复后调用
    identifierPrefix: string, // 结合 useId 生成的id string prefix 避免节点之间的冲突
  }
  const El = document.querySelect('#root')
  const root = hydrateRoot(El, reactNode, options)
  root.render(<App />)

  // 客户端和服务端不同的内容
  // index.html

  <div id="root"><div>is server</div></div>
  //index.js
  import {hydrateRoot} from 'react-dom/client'
  import YourComponent from './yourComponent.js'

  hydrateRoot(document.getElementById('root'), <YourComponent />)


  //yourComponent.js
  import React, {useState, useEffect} from 'react'
  
  export default const YourComponent = ()=>{
    const [isServer, isServerSet] = useState(true)
    useEffect(()=>{
      isServerSet(false)
    }, [])
    return (
      <div>{isServer?'is server':'is client'}</div>
    )
  }

  // 可以将整个dom文档使用 `hydrateRoot` 处理, 包括<html>

    const YourComponent = ()=> {
      return (
        <html>
          <head>
            <meta charSet="utf-8" />
            <meta name="viewport" content="width=device-width, initial-scale=1" />
            <link rel="stylesheet" href="/styles.css"></link>
            <title>My app</title>
          </head>
          <body>
            <Router />
          </body>
        </html>
      );
    }
    //将全局 document 传给hydrateRoot
     hydrateRoot(document, <YourComponent />)

    {/* 
      log hydrateRoot 
     */}

    ![hydrateRoot返回](./log-hydrateRoot.png)
    ![hydrateRoot返回-1](./log-hydrateRoot-1.png)

```

### React-dom 服务端Api

18. renderToNodeStream
[点此查看](https://react.docschina.org/reference/react-dom/server/renderToNodeStream)
19. renderToPipeableStream
[点此查看](https://react.docschina.org/reference/react-dom/server/renderToPipeableStream)
20. renderToReadableStream
[点此查看](https://react.docschina.org/reference/react-dom/server/renderToReadableStream)
21. renderToStaticMarkup
[点此查看](https://react.docschina.org/reference/react-dom/server/renderToStaticMarkup)
22. renderToStaticNodeStream
[点此查看](https://react.docschina.org/reference/react-dom/server/renderToStaticNodeStream)
23. renderToString
[点此查看](https://react.docschina.org/reference/react-dom/server/renderToString)


### React-dom Api

<a id="createPortal"></a>
24. createPortal 

```javascript

  // 示例
  import {createPortal} from 'react-dom'

  const YourComponent = ()=>{
    ...
    return (

      <div>
        <span>something...</span>
        createPortal(<YourModal />, document.body)  
      </div>
    )
  }
  // YourModal 渲染在document.body 中， 但仍是 YourComponent的子组件
```

<a id="flushSync"></a>
25. flushSync

```javascript

  //示例
  ...
  const update = flushSync(()=>{
    listSet(newList)
    ...
  })

```

<a id="findDOMNode"></a>
26. findDOMNode

```javascript

  //示例
  import {findDOMNode} from 'react-dom'
  ...
  const domNode = findDOMNode(YourClassComponentInstance)

  //
  import React, {Component} from 'react'

  class YourClassComponent extends Component {
    componentDidMount(){
      const input = findDOMNode(this)
      console.log('YourClassComponent', input)
      input.focus()
    }
    render(){
      return <input placeholder="请输入..."/>
    }
  }
  // 替代方案 ref 读取组件
  // class 组件
  import React, {Component, createRef} from 'react'

  class YourClassComponent extends Component {
    constructor(props){
      super(props)
      this.inputRef = createRef(null)
    }
    componentDidMount(){
      console.log('YourClassComponent', this.inputRef)
      this.inputRef.current.focus()
    }
    render(){
      return <input ref={this.inputRef} placeholder="请输入..."/>
    }
  }
  // 函数组件
  import React, {useRef, useEffect} from 'react'
  const YourFunCComponent = ()=>{
    let inputRef = useRef(null)
    useEffect(()=>{
      console.log('YourFunCComponent', inputRef)
      inputRef.current.focus()
    }, [])
    return (
      <input ref={inputRef} placeholder="请输入..."/>
    )
  }
```

<a id="hydrate"></a>
27. hydrate

[查看替代方案](#hydrateRoot)

<a id="render"></a>
28. render

```javascript

  //示例
  import {render} from 'react-dom'

  const res = render(<YourComponent />, document.getElementById('root'))
  // 如果 YourComponent = classComponent ,res = YourComponentInstance
  // 其他则 res = null
```

<a id="unmountComponentAtNode"></a>
29. unmountComponentAtNode

```javascript

  //示例
  import {unmountComponentAtNode} from 'react-dom'

  const res = unmountComponentAtNode(<YourComponent />)
  // 成功 res = true, 失败 res = false
```


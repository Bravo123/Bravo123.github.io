---
layout:     post
title:      "Redux-Sage "
subtitle:   "工作中遇到的技术点总结"
date:       2019-11-12 18:11:00
author:     "Thq"
header-img: "img/home-bg-geek.jpg"
catalog: true
tags:
    - Redux-Sage
---

> 文章主要记录Redux-Sage相关概念内容和使用，方便日后自己复习浏览

在使用`Ant design pro`，`dva`的时候发现`Redux-Sage`使用了`*`和`yield`，深入了解记录一下。

要知道`*`和`yield`首先要先了解下`ES6`中的`Generator`函数

> Generator
 
`Generator`是`ES6`是一种异步编程解决方案。

较我们平常写的函数不同的点有`*`、`yield`、`.next()`。

`*` 写在function与函数名之间，用来表示这是个`Generator`函数。

`yield`后面紧跟一个表达式，它为`Generator`函数提供了一种状态，也是一个暂停标志。`yield`本身不会有返回值，总是`undefined`。

`.next()`他会从暂停处开始执行，直到遇到下一个状态（`return`)语句也是一种状态）。因为`yield`表达式总返回`undefined`，所以`.next()`带的参数就是上一次yeild的返回值。

我们常见的 `Generator`形式如下
```
function* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
  return 'ending';
}

var hw = helloWorldGenerator();
hw.next()
// value 表示当前yield表达式的值，done表示遍历还未结束。
// { value: 'hello', done: false }

hw.next()
// { value: 'world', done: false }

hw.next()
// { value: 'ending', done: true }

hw.next()
// { value: undefined, done: true }
```
`Generator`函数可以看成一个状态机上面的例子表明该函数有三个状态`hello`、`world`和`return`语句。该函数返回一个遍历器对象（指向内部状态的指针对象），通过`.next()`方法可以依次遍历每一个状态，每次调用`.next()`就使得指针移向下一个状态。

`.next()`带参数的例子，传入的参数就是上一次yield的返回值
```
function* f() {
  for (let i = 0; true; i++) {
    let result = yield i;
    if (result) i = -2;
  }
}

let g = f();
g.next(); // { value: 0, done: false }
g.next(); // { value: 1, done: false }
g.next(true); // { value: -1, done: false }
```
```
function* foo(x) {
  var y = 2 * (yield (x + 1));
  var z = yield (y / 3);
  return (x + y + z);
}

var a = foo(5);
a.next() // Object{value:6, done:false}
a.next() // Object{value:NaN, done:false}
a.next() // Object{value:NaN, done:true}

var b = foo(5);
b.next() // { value:6, done:false }
b.next(12) // { value:8, done:false }
b.next(13) // { value:42, done:true }
```
上面两个例子都搞懂的话，`Generator`函数就基本上有了个大致的了解了。

> 用`for...of`遍历`Generator`

`Generator`因为本身返回了一个遍历器，一个`Iterator`对象，所以可以用`for...of`来进行遍历

```
function* foo() {
  yield 1;
  yield 2;
  yield 3;
  yield 4;
  yield 5;
  return 6;
}

for (let v of foo()) {
  console.log(v);
}
// 1 2 3 4 5
```
此时会自动去执行`.next()`方法，且当返回值的`done: true`的时候终止循环。

### `Redux-Sage`

`Generator`了解了之后就开始进入本文的主题了————`Redux-Sage`

有几个名词需要先了解一下
+ `Effect`

一个`effect`就是一个`Plain Object JavaScript`对象，包含一些`sage middleware`执行的指令

+ `Task`

`task`可以看作是一个后台运行的进程，可以同时运行多个`task`，通过`fork`函数创建`task`
```
function* saga() {
  ...
  const task = yield fork(otherSaga, ...args)
  ...
}
```

+ 阻塞调用/非阻塞调用

阻塞调用就是在`yield Effect`之后等待其执行结果返回

非阻塞调用则会`yield Effect`之后立刻恢复执行

```
function* saga() {
  yield take(ACTION)              // 阻塞: 将等待 action
  yield call(ApiFn, ...args)      // 阻塞: 将等待 ApiFn (如果 ApiFn 返回一个 Promise 的话)
  yield call(otherSaga, ...args)  // 阻塞: 将等待 otherSaga 结束

  yield put(...)                   // 阻塞: 将同步发起 action (使用 Promise.then)

  const task = yield fork(otherSaga, ...args)  // 非阻塞: 将不会等待 otherSaga
  yield cancel(task)                           // 非阻塞: 将立即恢复执行
  // or
  yield join(task)                             // 阻塞: 将等待 task 结束
}
```

+ `Watcher/Worker`
  + `Watcher`：监听发起的`action`  并且每次收到`action`的时候`fork`一个`worker`
  + `Worker`：处理action并结束它

```
function* watcher() {
  while(true) {
    const action = yield take(ACTION)
    yield fork(worker, action.payload)
  }
}

function* worker(payload) {
  // ... do some stuff
}
```

到目前为止还是不太懂，接着往下看吧

### 开始使用

因为`dva`封装过了，所以并不能直接在代码中看到关于`React-Sage`的影子。

`Redux-Sage`顾名思义是需要有`Redux`的前提，为了能使用`Sage`需要以下两步。

+ 创建一个`Sage middleware`和要运行的`Sage`
+ 将`Sage middleware`连接到`Redux store`

在`main.js`中修改

```
import { createStore, applyMiddleware } from 'redux'
import createSagaMiddleware from 'redux-saga'

//...
import { helloSaga } from './sagas'

const store = createStore(
  reducer,
  applyMiddleware(createSagaMiddleware(helloSaga))
)
```

通过`createSageMiddleware`工厂函数创建`Sage middleware`，然后在通过`applyMiddleware`把`middleware`连接到`store`上

### 异步调用

先写一个UI组件

```
const Counter = ({ value, onIncrement, onDecrement, onIncrementAsync }) =>
  <div>
    <button onClick={onIncrementAsync}>
      Increment after 1 second
    </button>
    {' '}
    <button onClick={onIncrement}>
      Increment
    </button>
    {' '}
    <button onClick={onDecrement}>
      Decrement
    </button>
    <hr />
    <div>
      Clicked: {value} times
    </div>
  </div>
```

把组件和`Store action`连接起来

```
// main.js
function render() {
  ReactDOM.render(
    <Counter
      value={store.getState()}
      onIncrement={() => action('INCREMENT')}
      onDecrement={() => action('DECREMENT')}
      onIncrementAsync={() => action('INCREMENT_ASYNC')} />,
    document.getElementById('root')
  )
}
```

`INCREMENT_ASYNC`  `action`执行一个异步调用

```
// sages.js
import { delay } from 'redux-saga'
import { put, takeEvery } from 'redux-saga/effects'
// ...
// Our worker Saga: 将执行异步的 increment 任务
export function* incrementAsync() {
  yield delay(1000)
  yield put({ type: 'INCREMENT' })
}
// Our watcher Saga: 在每个 INCREMENT_ASYNC action spawn 一个新的 incrementAsync 任务
export function* watchIncrementAsync() {
  yield takeEvery('INCREMENT_ASYNC', incrementAsync)
}
```

引入了一个工具函数`delay`，返回一个延时1秒再`resolve`的`Promise`对象。

`Sage`会把`yield`后面的对象放到`Redux-Sage middleware`上，这一类对象都是都是一类指令且都可以被`middleware`解释执行。当`middleware`接收到一个`yield`的`Pormise`后会暂停`Sage`，直到`Promise`完成。`yield put`则告诉`middleware`去发起一个`INCREMENT`的`action`。`put`就是一个`Effect`例子，当`middleware`拿到一个`yield Effect`的时候会暂停`Sage`，直到`Effect`执行完成。不需要开发者去通过`.next()`去执行。特别注意的是`middleware`会检查每个被`yield`的`Effect`类型，如果是`put`，`middleware`则会`dispatch`一个`action`给`Store`，如果是`call`，则会调用给定的函数。

所以上面的`incrementAsync`方法就是，通过`delay(1000)`延迟了1秒然后`dispath`了一个`INCREMNET`的`action`。

另一个`Sage` `watchIncrementAsync`用了`takeEvery`辅助函数，用来监听所有`INCREMENT_ASYNC` `action`，并且在`action`被匹配的时候执行`incrementAsync`任务。

现在有两个`Sage` `helloSage` 和 `watchIncrementAsync`，现在需要同时启动它们，则需要另一个`Sage` `rootSage`。

```
export default function* rootSage() {
  yield all([
    helloSage(),
    watchIncrementAsync()
  ])
}
```

```
// main.js
import rootSaga from './sagas'

const sagaMiddleware = createSagaMiddleware()
const store = createStore(
  reducer,
  applyMiddleware(sagaMiddleware)
)
sagaMiddleware.run(rootSaga)
```

以上就是`Redux-Sage`一个初级的使用方法。

todo list

  + `Sage`辅助函数
  + `声明式`Effects`
  + `dispatch action`
  + 错误处理 
## ReduxStoreProvider
这个库是为了简化 redux 创建 Action 和 Reducer 的过程，使创建过程语义化，提升应用自明性方便后期维护。


## 安装
```shell
npm install redux-store-provider --save
```

## 手册
```javascript
import ReduxStoreProvider from 'redux-store-provider'
```

### 成员

* [ReduxStoreProvider](#type)
    * `static` [type(id)](#static-typeid--string) ⇒ `string`
    * `static` [config(options)](#static-configoptions--this) ⇒ `this`
    * `static` [.getInitialState()](#reduxstoreprovidergetinitialstate--object) ⇒ `Object`
    * `static` [.getInitialStateList()](#reduxstoreprovidergetinitialstatelist--object) ⇒ `Object`
    * [new ReduxStoreProvider()](#new-reduxstoreprovider)
    * [.addHandler(name, handler)](#reduxstoreprovideraddhandlername-handler--this) ⇒ `this`
    * [.setInitialState(value)](#reduxstoreprovidersetinitialstatevalue--this) ⇒ `this`
    * [.getInitialState()](#reduxstoreprovidergetinitialstate--object-1) ⇒ `Object`
    * [.getReducer()](#reduxstoreprovidergetreducer--function) ⇒ `function`
    * [.createAction([actions])](#reduxstoreprovidercreateactionactions--object) ⇒ `Object`
    * [.createActionList([actions])](#reduxstoreprovidercreateactionlistactions--object) ⇒ `Object`



### `static` type(id) ⇒ `string`
生成一个在 Action 中使用的 `type` 字符串

| Param | Type | Description |
| --- | --- | --- |
| id | `string` | id value |


### `static` config(options) ⇒ `this`
全局配置，设置后将会覆盖默认设置并且在全局生效

| Param | Type | Description |
| --- | --- | --- |
| [options] | `Object` | 数据 |
| options.initialState | `Object` | 单一型配置 |
| options.initialStateList | `Object` | 列表型配置 |

```javascript
// 单一型 Store 配置
ReduxStoreProvider.config({
  initialState: {
    value: {}
  }
});

// 列表型 Store 配置
ReduxStoreProvider.config({
  initialState: {
    list: [],
    total: 0,
    value: {}
  }
});
```


### ReduxStoreProvider.getInitialState() ⇒ `Object`
获取单一型全局默认 Store 值


### ReduxStoreProvider.getInitialStateList() ⇒ `Object`
获取列表型全局默认 Store 值


### new ReduxStoreProvider()
初始化一个 Store 值

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| [options] | `Object` | `{}` | 数据 |
| options.key | `string` |  | 初始化 Store 的 KEY，必须具有单一性 |
| options.type | `string` |  | `single` 单一型，`list` 列表型 |
| options.initialState | `Object` |  | 初始化 Store，将会覆盖全局默认设置 |

```javascript
const UserStore = new ReduxStoreProvider({ key: 'POSTS' });
```


### ReduxStoreProvider.addHandler(name, handler) ⇒ `this`
扩展一个方法，可以在 Action 中触发

| Param | Type | Description |
| --- | --- | --- |
| name | `string` | 扩展方法名 |
| handler | `function` | 扩展方法 |


### ReduxStoreProvider.setInitialState(value) ⇒ `this`
覆盖默认 Store，仅在当前 Store 生效

| Param | Type | Description |
| --- | --- | --- |
| value | `Object` | 数据 |


### ReduxStoreProvider.getInitialState() ⇒ `Object`
获取当前 Store 的值
```javascript
UserStore.getInitialState();
// 返回值
// {
//   name: 'Sanonz',
//   email: 'sanonz@126.com'
//   ...
// }
```


### ReduxStoreProvider.getReducer() ⇒ `function`
获取当前 Store 的 Reducer，包括默认和自定义



### ReduxStoreProvider.createAction([actions]) ⇒ `Object`
创建单一型 Action，在需要更新 Store 的地方调用进行更新操作


| Param | Type | Default | Description |
| --- | --- | --- | --- |
| [actions] | `Object` | `{}` | 自定义 Action 对象 |

#### 默认的 Action 列表
* `action.set(path, value)`
* `action.merge(value)`

自定义 Action
```javascript
// 首先定义 Reducer 执行设置用户名的逻辑
UserStore.addHandler('set_name', (state, action) => {
  const newState = {...state};
  // 设置用户名
  newState.name = action.value;

  return newState;
});
// 然后定义触发 Reducer 逻辑的 Action
const userAction = UserStore.createAction({
  setName: function (value) {
    return {
      value,
      type: UserStore.type('set_name'),
    };
  },
});
// 进行设置操作
store.dispatch(userAction.setName('Sanonz'));
```


### ReduxStoreProvider.createActionList([actions]) ⇒ `Object`
创建列表型 Action，在需要更新 Store 的地方调用进行更新操作

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| [actions] | `Object` | `{}` | 自定义 Action 对象 |

#### 默认的 Action 列表
* `action.fill(value, total)`
* `action.shift()`
* `action.unshift(value)`
* `action.pop()`
* `action.push(value)`
* `action.insert(index, value)`
* `action.replace(index, value)`
* `action.remove(index)`
* 以及 [.createAction([actions])](#ReduxStoreProvider.createAction)  的所有 Action 列表


## 例子
### 创建 `users.json` 和 `posts.json` 存放模拟请求数据
`data/users.json`
```json
[
  {
    "name": "Sanonz",
    "email": "sanonz@126.com"
  }, {
    "name": "Toni Schneider",
    "email": "t@toni.org"
  }, {
    "name": "Beau Lebens",
    "email": "beau@dentedreality.com.au"
  }
]
```

`data/posts.json`
```json
[
  {
    "id": 1,
    "type": "primary",
    "text": "This is a primary alert—check it out!",
    "isLike": false
  },
  {
    "id": 2,
    "type": "secondary",
    "text": "This is a secondary alert—check it out!",
    "isLike": true
  },
  {
    "id": 3,
    "type": "success",
    "text": "This is a success alert—check it out!",
    "isLike": false
  },
  {
    "id": 4,
    "type": "danger",
    "text": "This is a danger alert—check it out!",
    "isLike": false
  },
  {
    "id": 5,
    "type": "warning",
    "text": "This is a warning alert—check it out!",
    "isLike": false
  },
  {
    "id": 6,
    "type": "info",
    "text": "This is a info alert—check it out!",
    "isLike": false
  }
]
```

### 创建 `users.js` 和 `posts.js` Reducer

`reducers/user.js`
```javascript
import ReduxStoreProvider from 'redux-store-provider';

import users from './data/users.json';


export default new ReduxStoreProvider({ key: 'USER' })
  .setInitialState({value: users[0]});
```

`reducers/posts.js`
```javascript
import ReduxStoreProvider from 'redux-store-provider';


export new ReduxStoreProvider({ key: 'POST', type: 'list' })
  // 扩展更新 Store 方法，LIKE 对应 Action 中的 type 字段
  .addHandler('LIKE', (state, action) => {
    let newState = state;
    let index = state.list.findIndex(row => row === action.value);
    if (!!~index) {
      newState = _.cloneDeep(state);
      const row = newState.list[index];
      row.isLike = !row.isLike;
    }

    return newState;
  });
```

### 创建 `users.js` 和 `posts.js` Action

`actions/user.js`
```javascript
import UserStore from '../reducers/user';


export default UserStore.createAction();
```

`actions/posts.js`
```javascript
import PostsStore from '../reducers/posts';


export default PostsStore.createActionList({
  like: function (value) {
    return {
      value,
      type: PostsStore.type('LIKE'),
      // 触发 LIKE 事件，type 和 ReduxStoreProvider:addHandler(key) 的 key 对应
    };
  },
});
```

### 创建 UI 组件

`components/Header.jsx`
```javascript
import React from 'react';
import md5 from 'blueimp-md5';
import { connect } from 'react-redux';


@connect(state => ({userStore: state.userStore}))
export default class Header extends React.Component {

  render() {
    const { userStore } = this.props;

    return (
      <nav className="navbar navbar-dark bg-dark">
        <div className="container">
          <a className="navbar-brand" href="#">
            ReduxStoreProvider
          </a>
          <span className="navbar-text">
            <img
              className="d-inline-block align-middle"
              src={`https://www.gravatar.com/avatar/${md5(userStore.value.email)}?s=40`}
              width="20"
              height="20"
            />
            &ensp;
            <span className="d-inline-block align-middle">{userStore.value.name}</span>
          </span>
        </div>
      </nav>
    );
  }

};
```

`components/Posts.jsx`
```javascript
import React from 'react';
import { connect } from 'react-redux';

import postsAction from './actions/posts.js';


connect(state => ({userStore: state.userStore, postsStore: state.postsStore}))
export default class Posts extends React.Component {

  componentDidMount() {
    const { dispatch } = this.props;

    // 模拟HTTP请求数据
    fetch('./data/post.json')
      .then(response => response.json())
      .then(rows => {
        // 请求的数据填充的 PostsStore 中
        dispatch(postsAction.fill(rows);
      }));
  }

  onLike(e, row) {
    const { dispatch } = this.props;
    dispatch(postAction.like(row));
  }

  onRemove(e, index) {
    const { dispatch } = this.props;
    dispatch(postAction.remove(index));
  }

  render() {
    const { postsStore } = this.props;

    return (
      <div>
        {postsStore.list.map((row, index) =>
          <div
            key={row.id}
            className={`alert alert-${row.type}`}
            role="alert"
          >
            {row.text} &ensp;
            <button
              type="button"
              className={`btn btn-sm btn-${row.isLike ? 'success' : 'light'}`}
              onClick={e => this.onLike(e, row)}
            >
              Like
            </button>
            <button
              type="button"
              className="close"
              aria-label="Close"
              onClick={e => this.onRemove(e, index)}
            >
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
        )}
      </div>
    );
  }

};
```

`App.jsx`
```javascript
import React from 'react';
import { connect } from 'react-redux';

import Header from './components/Header';
import Posts from './components/Posts';
import userAction from './actions/user.js';
import postsAction from './actions/posts.js';
import users from './data/users.json';


@connect(state => ({userStore: state.userStore, postsStore: state.postsStore}))
export default class App extends React.Component {

  onRandomUser() {
    const { dispatch, userStore } = this.props;
    const data = users.filter(user => user.email !== userStore.value.email);
    dispatch(userAction.merge(_.sample(data)));
  }

  onShufflePosts() {
    const { dispatch, postsStore } = this.props;
    dispatch(postsAction.fill(_.shuffle(postsStore.list)));
  }

  onAddPost() {
    const { dispatch, postsStore } = this.props;
    dispatch(postAction.push(_.sample(postsStore.list)));
    // dispatch(postAction.unshift(_.sample(postsStore.list)));
  }

  render() {
    return (
      <div className="app">
        <Header />
        <div className="container">
          <div className="content">
            <div className="row">
              <div className="col-sm-4">
                <div className="list-group">
                  <button
                    type="button"
                    className="list-group-item list-group-item-action"
                    onClick={e => this.onAddPost(e)}
                  >
                    Add Post
                  </button>
                  <button
                    type="button"
                    className="list-group-item list-group-item-action"
                    onClick={e => this.onShufflePosts(e)}
                  >
                    Shuffle Posts
                  </button>
                  <button
                    type="button"
                    className="list-group-item list-group-item-action"
                    onClick={e => this.onRandomUser(e)}
                  >
                    Random User
                  </button>
                </div>
              </div>
              <div className="col-sm-8">
                <Posts />
              </div>
            </div>
          </div>
        </div>
      </div>
    );
  }

}
```

### 创建 APP

`setup.js`
```javascript
import App from './App';

import UserStore from '../reducers/user';
import PostsStore from '../reducers/posts';


const store = Redux.createStore(
  Redux.combineReducers({
    userStore: UserStore.getReducer(),
    postsStore: PostsStore.getReducer(),
  })
);

ReactDOM.render(
  <ReactRedux.Provider store={store}>
    <App />
  </ReactRedux.Provider>,
  document.getElementById('root')
);
```

```javascript

```

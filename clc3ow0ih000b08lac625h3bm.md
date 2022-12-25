# Zustand , The state management tech

As you know there are many global state management solutions to React out there, like [Context](https://reactjs.org/docs/context.html), [Redux](https://redux.js.org/), [Mobx](https://mobx.js.org/), [Recoil](https://recoiljs.org/), etc. All of them have their advantages and disadvantages. I used Redux for several years and using Context too, but there is a state manager that if you check it out, you choose it over other ones! It’s [Zustand](http://github.com/react-spring/zustand). If you likes hooks you will like Zustand too.

![](https://miro.medium.com/max/875/1*fKV3_Y4usDYZKPsNp1yCvA.png align="left")

Zustand is really simple but powerful state manager for react. For Zustand, just create a store, and then bring the hook into your component, and use it.

You’d create a custom hook and the states shared unlike built-in react hooks. There is no Context Provider wrappers in app and it’s really tiny (near 600 Bytes minified and gzipped!).

## **Installation**

```javascript
yarn add zustand
// or
npm install zustand
```

As you can see in the following code we just create a global store and then create two components to use the store.

```javascript
import create from 'zustand'const useStore = create(set => ({
  counter: 0,
  increase: () => set(state => ({ counter: state.counter + 1 })),}))const Counter = () => {
  const counter = useStore(state => state.counter)return <h1>{counter}</h1>
}const IncreaseButton = () => {
  const increase = useStore(state => state.increase)return <button onClick={increase}>+</button>
}
```

## **Async actions**

If you need to update global state asynchronously in Redux you needs to use middlewares like [Redux Thunk](https://github.com/reduxjs/redux-thunk) or [Redux Saga](https://redux-saga.js.org/) but Zustand doesn’t care if your actions are async or not.

```javascript
const useStore = create(set => ({
  users: {},
  fetch: async () => {
    const response = await fetch('/users')
    set({ users: await response.json() })
  }
}))
```

## **Using Redux DevTools**

If you like [Redux DevTools](https://github.com/reduxjs/redux-devtools) and missed on other state management solutions, you should know that Zustand supports Redux DevTools. You should know that it does not Redux based.

## **Using Zustand without React**

It can truly be used vanilla without react! You should just import create from ‘zustand/vanilla’ instead of ‘zustand’.

```javascript
import create from 'zustand/vanilla'const { getState, setState, subscribe, destroy } = create(() => ({ ... }))
```

## **Using it outside of components**

If you need your global state values in some modules that are not component you have access to it by several utility functions. You can get state by getState(), set new value with setState() and subscribe to state changes with subscribe().

```javascript
const useStore = create(() => ({ isAwsome: true }))const state = useStore.getState()
consol.log(state.isAwsome)useStore.setState({ isAwsome: false })
```
Notes by building a simple counter app 

Store : One central object holding all your app's state
Slice : A chunk of state + the reducers (functions) that update it, and auto generated actions
Dispatch : How components tell the store "something happened", triggering a reducer to update state

Data flow is one directional : Component dispatches an action -> reducer computes new state -> store updates -> component re-renders with new state.
No manual state mutation, no prop drilling.

Create Slice : 
CounterSlice.ts
```ts
import {createSlice} from '@reduxjs/toolkit'

interface CounterState {
    value : number;
}

const initialState : CounterState = {
    value: 0
}

const counterSlice = createSlice({
    name : 'counter',
    initialState,
    reducers: {
        increment: (state) => {
            state.value += 1;
        },
        decrement : (state) => {
            state.value -= 1;
        }
    }
});

export const {increment, decrement} = counterSlice.actions;
export default counterSlice.reducer;
```

createSlice() - Auto generates action creators (increment and decrement) and action types (counter/increment), for you - you never hand write action type strings
counterSlice.reducer - is the actual reducer function you plug into the store


Create the store
Store.ts 
```ts
import {configureStore} from '@reduxjs/toolkit'
import counterReducer from './features/counterSlice'

export const store = configureStore({
    reducer : {
        counter : counterReducer,
    }
})

export type RootState = ReturnType<typeof store.getState>
export type AppDispatch = typeof store.dispatch
```

`configureStore` sets up ReduxDevTools extension and some useful default middleware (like one that warns you if you accidentally mutate state ourside a reducer). The reducer object here is your root reducer - each key becomes a slice of the overall state tree. SO your state shape will be `{counter : {value : 0}}`

RootState  : The type of your entire store's state tree, derived automatically from the store itself (so it stays in sync if you add more slices later)
AppDispatch : The type of your store's dispatch function, which knows about all your action creators and any middleware (like thunks) you've added
These two types are what let useSelector and useDispatch give you autocomplete and type safety instead of any


hooks.ts
```ts
import {useDispatch, useSelector} from 'react-redux'
import type {RootState, AppDispatch} from './store'

export const useAppDispatch = () => useDispatch<AppDispatch>()
export const useAppSelector = useSelector.withTypes<RootState>()
```

Then the actual counter component
Counter.tsx
```tsx
import React from 'react'
import {useAppSelector} from '../hooks'
import {useAppDispatch} from '../hooks'
import {increment, decrement} from '../features/counterSlice'

function Counter() {

    const count = useAppSelector((state) => state.counter.value);
    const dispatch = useAppDispatch();

  return (
    <div >
        <div className="  flex flex-row bg-red-400 justify-center">
            <button className="bg-blue-400 p-5" onClick={() => {dispatch(decrement())}}>-</button>
            <div className="flex flex-row justify-items-center p-5">{count}</div>
            <button className="bg-blue-400 p-5" onClick={() => {dispatch(increment())}}>+</button>
        </div>
    </div>
  )
}

export default Counter
```


main.tsx
```tsx
import { StrictMode } from 'react'
import ReactDOM from 'react-dom/client'
import { Provider } from 'react-redux'
import { store } from './app/store'
import App from './App'

ReactDOM.createRoot(document.getElementById('root')!).render(
  <StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </StrictMode>
)
```

`Provider` uses React Context internally to make the store available to every component in the tree, no matter how deeply nested — that's what solves the prop-drilling problem.